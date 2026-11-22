# Agentes de IA en Discord

Síntesis de decisiones técnicas y operativas a partir de la [Guía Técnica para Crear Agentes de IA en Discord (2026)](../fuentes/agentes-ia-discord-produccion-2026.md), contrastada en los puntos comunes con [Flujos de trabajo agénticos](flujos-de-trabajo-agenticos.md) y [Automatización con Hermes](automatizacion-con-hermes.md). Todas las cifras, versiones, nombres de modelos, tarifas y políticas de 2026 proceden de esas fuentes y no se han verificado externamente.

## Decisiones de arquitectura sobre Discord

La regla estructural de partida es separar el transporte de Discord (recepción de eventos, firma criptográfica, colas de límites de tasa y renderizado de componentes) del núcleo agéntico (razonamiento, herramientas, memoria y controles de seguridad), de modo que la latencia del LLM nunca bloquee el bucle de eventos ni expire interacciones.

| Necesidad del agente | Ruta recomendada según la fuente | Condición operativa clave |
| --- | --- | --- |
| Escucha continua de canales, moderación proactiva, memoria contextual o voz | Gateway WebSocket 24/7 (`discord.py` o `discord.js`) | Proceso estable sin hibernación; sharding desde ~1,000 servidores por shard (límite 2,500) |
| Comandos slash, menús contextuales o uso personal sin servidor 24/7 | Endpoint HTTP Interactions serverless | Verificación Ed25519 sobre cuerpo crudo; riesgo de *cold starts* frente al límite de 3 s |
| Asistente personal en cualquier servidor o DM sin instalar bot | Contexto `USER_INSTALL` (ID 1) | Solo `applications.commands`, sin lectura pasiva de canales y máx. 5 follow-ups externos |

Dos reglas transversales condicionan cualquier arquitectura en Discord:
- **Plazo de 3 segundos:** toda interacción respaldada por un LLM debe ejecutar `defer` como primera operación para disponer de 15 minutos, y en tareas largas debe persistirse en cola duradera antes de diferir para no dejar al usuario colgado si el servidor reinicia.
- **Mínimos Privileged Intents:** bajo las reglas vigentes desde el 10 de junio de 2026 descritas en la fuente, el umbral de revisión cambió de 100 servidores a **10,000 usuarios únicos** (con 90 días de margen sin bloqueo y renovación anual). Evitar `MESSAGE_CONTENT` cuando bastan comandos slash, menciones o DMs simplifica operación y cumplimiento.

## Stack, MCP y límites operativos

- **Librería cliente:** `discord.py` (v2.7.1) alinea mejor con el ecosistema agéntico en Python y menor consumo base estimado (~60–90 MB frente a ~80–120 MB en `discord.js` v14.28.1 en 100 servidores), mientras `discord.js` (serie v14 estable; evitar v15 pre-lanzamiento) favorece equipos que unifican bot y panel web en TypeScript.
- **Orquestación:** la guía coincide en no introducir un framework si basta un bucle simple de llamadas a herramientas; reservar LangGraph, PydanticAI, OpenAI Agents SDK, CrewAI, Google ADK, Microsoft Agent Framework (sucesor de AutoGen y Semantic Kernel), Claude Agent SDK o Mastra para necesidades reales de estado duradero, tipado estricto, pausas humanas o multiagente.
- **Plataformas visuales:** n8n carece de disparador oficial de mensajes en tiempo real (obliga a polling, nodo comunitario o bot puente); Dify y Langflow funcionan mejor detrás de un cliente propio en `discord.py`/`discord.js`; Flowise queda descartado tras su congelamiento y fin de soporte en agosto de 2026.
- **MCP en dos direcciones:** usar el **bot como cliente MCP** (Streamable HTTP o `stdio` aislado, con *allowlists*, *denylists* y *tool pinning*) es el patrón seguro en producción. Exponer **Discord como servidor MCP** se desaconseja en producción multi-servidor (por tokens en texto plano, falta de aislamiento y atribución según Scalekit), y los *self-bots* violan los Términos de Servicio.
- **Cuotas y formato:** agrupar streaming por lotes (`>=500 ms`), obedecer `retry_after` para evitar cierres `4008` en Gateway o bloqueos de IP en Cloudflare (10,000 errores 401/403/429 en 10 min) y validar localmente longitudes antes de enviar (2,000 chars en texto, 6,000 combinados en embeds, hasta 40 elementos con `IS_COMPONENTS_V2` y 20 MiB en adjuntos desde el 3 de septiembre de 2026).

## Memoria, RAG con ACL y voz bajo DAVE

La fuente estructura la memoria en cuatro niveles: memoria de trabajo con presupuesto de tokens, estado conversacional duradero (checkpointing indexado por `(guild_id, channel_or_thread_id)`), memoria semántica/episódica aislada por `(guild_id, user_id)` (por ejemplo con Mem0 o Zep) y **RAG híbrido (denso + BM25) con filtrado ACL obligatorio** por `(guild_id, channel_id, required_role_ids)` antes de inyectar fragmentos al prompt para impedir filtraciones entre canales privados y públicos (`LLM02:2026`).

En **canales de voz**, el cifrado extremo a extremo **DAVE** (MLS + AES-128-GCM, obligatorio desde el 1 de marzo de 2026 salvo *Stage Channels*) introduce una asimetría crítica en librerías abiertas: mientras el envío TTS con `davey` funciona estable, la recepción STT presenta roturas en `discord-ext-voice-recv` y `@discordjs/voice v0.19.x`, requiriendo validar soluciones como `py-cord` (PR `#3159`) sin mezclarlo con `discord.py` en el mismo entorno virtual. Además, el receptor debe desacoplar la lectura RTP (~50 paquetes/s por hablante) hacia colas asíncronas, filtrar `user.bot`, manejar búferes por SSRC y, en Gemini Live, rotar sesiones antes del corte periódico de ~10 minutos.

## Seguridad determinista, privacidad y economía

En canales públicos donde cualquier usuario puede enviar texto o adjuntos, las defensas deben residir fuera del prompt (`LLM08:2026`):
- Verificar permisos reales del usuario solicitante frente al objetivo para evitar *Confused Deputy* y respetar jerarquía de roles.
- Exigir botones de confirmación humana (HITL) para acciones destructivas o moderación sensible.
- Enviar `X-Audit-Log-Reason` y replicar registros en base de datos propia y `#mod-log` (el Audit Log de Discord caduca a los 45 días).
- Cumplir la cláusula 21 de la Discord Developer Policy (prohibido entrenar modelos con mensajes de Discord, hacer scraping o vender datos), publicar Política de Privacidad, implementar `/delete-my-data` y distinguir entre no entrenamiento, retención temporal y acuerdos **Zero Data Retention (ZDR)** reales por proveedor.

En economía, el gasto de un agente de Discord está dominado por los tokens de entrada (~92.6% en el escenario modelado). La combinación de caché de prefijos con enrutamiento entre modelos ligeros y de frontera reduce drásticamente el coste frente a usar frontera sin caché, y mantiene a las APIs comerciales por delante de alquilar GPU dedicada salvo volúmenes masivos (centenares de millones de tokens al mes) o requisitos estrictos de soberanía. Para Gateway 24/7, descartar planes gratuitos que hibernan procesos.

## Conexiones y diferencias con otras páginas de la bóveda

- **Con [Flujos de trabajo agénticos](flujos-de-trabajo-agenticos.md):** ambas guías comparten principios centrales —comenzar con el patrón determinista más simple, separar controles de seguridad fuera del modelo, referenciar el catálogo OWASP Agentic 2026 (`ASI01–ASI10`), restringir MCP y optimizar costos ordenando prefijos para maximizar caché de prompts—. Difieren en el perfil numérico de enrutamiento que modelan: la guía general propone una mezcla **70/20/10** (ahorro ilustrativo de 59% al mover memoria dinámica para pasar de 7% a 84% de caché), mientras la guía de Discord modela un reparto **80/20** con 65% de prefijo cacheado (ahorro modelado del 86.2% frente a frontera sin caché). Son escenarios ilustrativos con supuestos distintos, no mediciones empíricas comparables.
- **Con [Automatización con Hermes](automatizacion-con-hermes.md):** *(interpretación de síntesis entre ambas fuentes)* el curso de Hermes distingue entre agentes *inhouse* (datos internos de empresa) y *outhouse* (orientados a usuario externo con datos acotados). Un agente en un servidor comunitario de Discord opera expuesto a entradas no confiables de terceros, lo que explica por qué la guía de Discord prioriza defensas externas deterministas como ACL en RAG, comprobaciones de *Confused Deputy*, HITL y cuotas estrictas por usuario y servidor.

## Vacíos y decisiones abiertas

- Solo hay una fuente dedicada específicamente a Discord en la biblioteca, recibida sin autor, URL ni fecha de publicación declarados.
- No se han verificado en la web ni en código propio las versiones concretas de `discord.py`, `discord.js` o `py-cord`, el estado actual de los PRs de recepción de audio bajo DAVE, las tarifas y nombres de modelos citados para 2026 ni los términos vigentes de ZDR de cada proveedor.
- Faltan pruebas empíricas de latencia (`defer`, RAG híbrido, streaming por lotes) y consumo real en un bot desplegado.

## Fuentes y páginas relacionadas

- [Guía Técnica para Crear Agentes de IA en Discord (Arquitectura, Herramientas y Producción 2026)](../fuentes/agentes-ia-discord-produccion-2026.md): resumen detallado y límites de la fuente original.
- [Flujos de trabajo agénticos](flujos-de-trabajo-agenticos.md): base general sobre topologías, ejecución durable, evaluación y economía de tokens.
- [Automatización con Hermes](automatizacion-con-hermes.md): contraste entre agentes internos (*inhouse*) y orientados a terceros (*outhouse*).