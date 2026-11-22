# Guía Técnica para Crear Agentes de IA en Discord (Arquitectura, Herramientas y Producción 2026)

- **Origen:** [Guía Técnica para Crear Agentes de IA en Discord (Arquitectura, Herramientas y Producción 2026)](<../../fuentes/Guía Técnica para Crear Agentes de IA en Discord (Arquitectura, Herramientas y Producción 2026).md>). Texto pegado por el usuario (`pasted-text.txt`) en bloque continuo sin saltos de línea y conservado intacto como original.
- **URL de origen:** no consta.
- **Autor:** no consta.
- **Fecha de publicación:** no consta (el contenido referencia cambios y revisiones hasta noviembre de 2026).
- **Capturado:** 2026-11-22.

## Idea central y cuatro cambios clave de 2026

Según la fuente, construir un agente de IA en Discord consiste en acoplar una aplicación del Discord Developer Portal (conectada por Gateway WebSocket o endpoint HTTP de interacciones) a un bucle agéntico multipaso con memoria persistente, herramientas externas (RAG, APIs, MCP) y controles deterministas de seguridad, como pedir confirmación humana mediante botones nativos. En producción resulta clave desacoplar la capa de transporte de Discord (eventos, firmas, colas de límites de tasa y renderizado) del núcleo agéntico (orquestación, memoria, herramientas y barreras) para que la latencia del LLM no bloquee ni expire las interacciones.

El texto plantea tres rutas técnicas iniciales: un servicio continuo 24/7 conectado al Gateway en Python (`discord.py 2.7+`) o TypeScript (`discord.js v14.28+`) junto a un framework o bucle propio cuando se requiere escucha de canales, moderación proactiva, memoria contextual o voz; un endpoint HTTP serverless cuando basta con comandos slash (`/preguntar`), menús contextuales o apps instalables por usuario (`USER_INSTALL`); y plataformas visuales como n8n o Dify, ponderando sus restricciones en Discord (sin disparador oficial de mensajes en tiempo real en n8n y fin de soporte de Flowise en agosto de 2026).

Cuatro cambios consolidados en 2026 que la guía destaca como condicionantes:
1. **Privileged Intents (desde el 10 de junio de 2026):** el umbral de revisión pasó de 100 servidores a 10,000 usuarios únicos con renovación anual.
2. **Cifrado de voz DAVE (obligatorio desde el 1 de marzo de 2026):** afecta la recepción de audio en varias librerías abiertas.
3. **Components V2:** amplió el techo de componentes por mensaje a 40 elementos.
4. **Caché de prompts y enrutamiento híbrido:** permite reducir hasta en un 86.2% la factura mensual modelada en cargas agénticas típicas frente a un despliegue sin optimizar.

## 1. Arquitectura de transporte, contextos e Intents

Discord ofrece dos mecanismos mutuamente excluyentes para recibir interacciones: Gateway WebSocket persistente o webhooks salientes a una `Interactions Endpoint URL` HTTP. En ambos casos rige un límite estricto de **3 segundos** para la respuesta inicial: si expira, el token queda invalidado, Discord no reintenta y muestra *"This interaction failed"*. El patrón estándar consiste en ejecutar `defer` de inmediato (tipo 5 o `interaction.deferReply()`), extendiendo la validez del token a **15 minutos** para editar o enviar seguimientos.

| Dimensión arquitectónica | Bot con Gateway WebSocket | Endpoint HTTP Interactions (Serverless) | App Instalable por Usuario (`USER_INSTALL`) |
| --- | --- | --- | --- |
| Conectividad y runtime | Proceso continuo 24/7 (VPS, contenedor o worker) | Bajo demanda vía HTTPS POST (Workers, Cloud Run) | Compatible con Gateway o HTTP (habitualmente HTTP) |
| Escucha de mensajes en canales | Sí (requiere `Message Content Intent` salvo excepciones) | No; solo responde a comandos, botones y modales | No; solo comandos slash o menú contextual sobre un mensaje |
| Moderación proactiva y voz | Sí (eventos en tiempo real y cliente de voz DAVE) | No | No en servidores donde la app no esté instalada como bot |
| Seguridad de entrada | Token de bot en sesión WSS + Intents declarados | Verificación obligatoria de firma Ed25519 en cada petición | Según transporte; diferenciación vía `authorizing_integration_owners` |
| Principal cuello de botella | Reconexiones, memoria caché y sharding | Arranques en frío frente al límite de 3 s y tareas en segundo plano | Límite de 5 follow-ups por interacción en servidores externos |

Detalles operativos señalados en la fuente:
- **Gateway y sharding:** límite técnico de 2,500 servidores por shard y práctica recomendada de ~1 shard por cada 1,000 servidores.
- **HTTP Interactions:** exige validar `X-Signature-Ed25519` y `X-Signature-Timestamp` sobre los bytes exactos del cuerpo crudo sin reserializar y responder al PING (tipo 1) con `{"type": 1}`; Discord envía firmas inválidas periódicas y deshabilita la URL si se aceptan.
- **Riesgos serverless:** los arranques en frío (como el servicio web gratuito de Render, que hiberna tras 15 minutos y tarda ~1 minuto en despertar) rompen el límite de 3 segundos; en Cloudflare Workers, las tareas asíncronas se detienen tras responder salvo que se desacoplen a colas o ejecución en segundo plano, y el entorno devuelve HTTP 403 al descargar adjuntos desde rutas CDN no efímeras de Discord.
- **Contextos de instalación:** `GUILD_INSTALL` (ID 0) frente a `USER_INSTALL` (ID 1, configurado con `integration_types` y `contexts` como `BOT_DM` y `PRIVATE_CHANNEL`). `USER_INSTALL` solo admite el alcance `applications.commands`, no lee mensajes ni administra servidores externos y tiene un máximo de 5 mensajes de seguimiento por interacción fuera de sus servidores.

### Privileged Intents y reglas desde junio de 2026

Los tres Privileged Intents son `GUILD_PRESENCES`, `GUILD_MEMBERS` y `MESSAGE_CONTENT` (bit `1 << 15`). Si falta habilitar o aprobar `MESSAGE_CONTENT`, los campos `content`, `embeds`, `attachments`, `components` y `poll` llegan vacíos (o `poll` omitido); si el código solicita un intent privilegiado sin activación ni aprobación en el portal, Discord cierra la sesión con el código **4014** (*Disallowed intent(s)*).

Reglas atribuidas al cambio del 10 de junio de 2026:
- **Umbral de 10,000 usuarios únicos:** por debajo de 10,000 usuarios únicos sumados entre todos los servidores instalados pueden activarse libremente en el Developer Portal.
- **Ventana de 90 días sin bloqueo:** al cruzar 10,000 usuarios únicos con un intent privilegiado activo, Discord notifica y concede 90 días para enviar revisión sin bloquear la entrada a nuevos servidores durante la evaluación.
- **Renovación anual y desvinculación:** las aplicaciones aprobadas deben rejustificar el acceso cada año (con otro plazo de 90 días), y este trámite quedó separado de la Verificación de Aplicación (que sigue aplicando al superar 100 servidores).
- **Cuatro excepciones sin `MESSAGE_CONTENT`:** los comandos slash no lo requieren, y en eventos de mensaje Discord conserva el contenido para mensajes propios de la app, DMs con la app, mensajes donde es mencionada explícitamente y el mensaje objetivo de un comando de menú contextual.

## 2. Límites de tasa, streaming y formato

Discord aplica cuatro capas simultáneas de limitación de tráfico:
1. **Límite global REST:** 50 solicitudes por segundo por token de bot (exentas las respuestas a interacciones y webhooks, que usan buckets propios).
2. **Buckets por ruta y recurso:** encabezados dinámicos `X-RateLimit-Limit`, `X-RateLimit-Remaining`, `X-RateLimit-Reset-After`, `X-RateLimit-Bucket` y `X-RateLimit-Scope`. Según guías operativas citadas por la fuente, cambiar nombre o tema de un canal se limita a 2 modificaciones cada 10 minutos por canal, y editar con frecuencia mensajes de más de una hora dispara límites adicionales con error **30046**.
3. **Límite de solicitudes inválidas (baneo de IP en Cloudflare):** alcanzar 10,000 respuestas HTTP 401, 403 o 429 en 10 minutos desde una IP provoca bloqueo temporal (no cuentan los 429 con `X-RateLimit-Scope: shared`). Es crítico obedecer `retry_after`.
4. **Envío por Gateway:** máximo 120 eventos cada 60 segundos por conexión (incluidos heartbeats); superarlo cierra el WebSocket con código **4008**.

Para simular streaming sin agotar límites, la guía recomienda acumular tokens en un búfer y editar por lotes espaciados (por ejemplo, cada 500 ms o más), usar la caché local (`get_channel`) en vez de llamadas REST repetidas (`fetch_channel`) y emplear respuestas efímeras para estados intermedios. Discord rechaza con HTTP 400 cualquier mensaje que exceda sus topes sin recortarlo:

| Recurso de Discord | Límite técnico vigente según la fuente (2026) | Implicación directa para agentes de IA |
| --- | --- | --- |
| Texto estándar (`content`) | 2,000 caracteres (4,000 de Nitro no aplica a bots) | Dividir respuestas extensas por párrafos o bloques de código cerrando delimitadores Markdown. |
| Embeds clásicos | Hasta 10 embeds; descripción 4,096 chars; 6,000 chars combinados por mensaje | Sumar `title`, `description`, `field.name`, `field.value`, `footer.text` y `author.name` para evitar HTTP 400. |
| Components V2 (`IS_COMPONENTS_V2`) | Flag `1 << 15` (irreversible en ese mensaje); hasta 40 componentes totales | Deshabilita `content`, `embeds`, `poll` y stickers; usa `Text Display`, `Container`, `Section`, `Separator`, `Media Gallery` y `File`. Reportes prácticos citan topes de 4,000 chars de texto visible (con límites de carga no documentados) o hasta 6,000 según tarjeta. |
| Adjuntos y archivos | 20 MiB por defecto (elevado desde 10 MiB el 3 de septiembre de 2026); hasta 10 archivos por mensaje | Útil para entregar informes extensos (`.md`, `.csv`, gráficos, transcripciones) sin fragmentar respuestas. |

## 3. Selección del stack tecnológico en 2026

### Librerías cliente y frameworks agénticos

- **Python — `discord.py` (v2.7.1, marzo de 2026):** soporte completo API v10 sobre `asyncio`, voz DAVE mediante `davey`, nuevos componentes de modales (`CheckboxGroup`, `RadioGroup`, `Label`) y consumo estimado en reposo de ~60–90 MB en 100 servidores (según estimaciones de proveedores de hosting citadas en la fuente).
- **JavaScript/TypeScript — `discord.js` (v14.28.1 en noviembre de 2026):** consumo estimado en reposo de ~80–120 MB en 100 servidores. La fuente recomienda mantenerse en v14 para producción, porque v15 sigue en pre-lanzamiento con cambios incompatibles (WebSocket, `ready` -> `clientReady` y `fetchReply` -> `withResponse`).

La guía sugiere evitar un framework cuando basta un agente de rol único con pocas herramientas sin estado complejo entre pasos (un bucle directo de *tool calling* es más simple de depurar), y adoptar un framework cuando hacen falta checkpointing duradero, ramificaciones, múltiples especialistas, salidas tipadas o trazabilidad:

| Framework (Estado 2026 según fuente) | Lenguajes | Modelo mental principal | Fortaleza clave para bots de Discord | Cuándo elegirlo |
| --- | --- | --- | --- | --- |
| LangGraph | Python, JS/TS | Grafo explícito (`StateGraph`) | Checkpointing por `thread_id`, control determinista de ciclos y pausas `interrupt()` para botones | Flujos con estado por hilo/canal, moderación escalonada o procesos auditables |
| PydanticAI | Python | Funciones tipadas con validación Pydantic | Inyección de dependencias sin exponer credenciales al modelo y salidas estructuradas estrictas | Servicios en Python que alimentan embeds, modales o bases de datos |
| OpenAI Agents SDK (v0.24.0) | Python, TS | `Agent`, `Runner`, `Handoffs`, `Guardrails` | Delegación multiagente, sesiones, trazado y MCP nativo (cierre del Agent Builder visual el 30 nov 2026) | Asistentes centrados en modelos OpenAI con enrutamiento entre especialistas |
| CrewAI | Python | `Crews` por roles + `Flows` por eventos | Prototipado rápido de equipos y soporte MCP (`stdio`, SSE, Streamable HTTP) | Pipelines de investigación, síntesis diaria o creación de contenido |
| Google ADK (v2.14.1) | Python, TS, Java, Go, Kotlin | Flujos híbridos (deterministas + agénticos) | Filtrado y resumen automático de contexto; interoperabilidad MCP, A2A y OpenAPI | Equipos en Google Cloud / Vertex AI o centrados en Gemini |
| Microsoft Agent Framework (1.0+) | Python, .NET | Workflows en grafo | Checkpointing, OpenTelemetry, MCP/A2A (AutoGen y Semantic Kernel pasan a mantenimiento para nuevos proyectos) | Organizaciones en Azure / .NET o migraciones desde AutoGen/Semantic Kernel |
| Claude Agent SDK | Python, TS | Arnés derivado de Claude Code | Hooks de ciclo de vida para interceptar herramientas, subagentes aislados, MCP y sesiones reanudables | Agentes técnicos, soporte de ingeniería o automatizaciones nativas con Claude |
| Mastra / Vercel AI SDK | TypeScript | Primitivas JS/TS + `WorkflowAgent` | Aprobaciones a nivel de herramienta, memoria y flujos en TS (core de Mastra bajo Apache 2.0) | Equipos `discord.js` que construyen bot y panel web en un solo lenguaje |

### Plataformas visuales (No-Code / Low-Code) y MCP bidireccional

- **n8n (v2.42+):** >400 integraciones y nodo oficial de Discord con 14 operaciones, pero **sin `Discord Trigger` oficial** para escuchar mensajes en tiempo real. Requiere polling (alta latencia), nodo comunitario no oficial (`n8n-nodes-discord-trigger-new`, con riesgo de rotura o abandono) o un bot puente propio hacia un Webhook. Su *Sustainable Use License* permite uso interno gratuito pero restringe revenderlo como servicio administrado.
- **Dify (v1.17.1):** Knowledge Pipelines para RAG, memoria, nodo `Human Input` (febrero de 2026) y publicación como API/MCP bajo Apache 2.0 modificada (restringe hospedaje multi-tenant comercial y quitar marca).
- **Langflow (v1.12.6):** de DataStax, orientado a Python bajo licencia MIT; delega autenticación y aislamiento multi-tenant en capas externas. Patrón limpio recomendado: usar Dify o Langflow como API detrás de un cliente delgado en `discord.py`/`discord.js`.
- **Flowise (descartado):** congelado el 29 de julio de 2026, repositorio archivado en agosto y fin de soporte el 31 de agosto de 2026 (~1 año tras adquisición por Workday).

En cuanto a **MCP en Discord**, la guía distingue dos arquitecturas opuestas:
- **Discord como Servidor MCP (desaconsejado en producción multi-servidor):** paquetes como `barryyip0625/mcp-discord`, `SaseQ/discord-mcp`, Pipedream o Kite permiten que un cliente externo (Claude Desktop, Cursor, Claude Code) administre Discord. Según un análisis técnico de Scalekit citado en la fuente, las configuraciones locales suelen almacenar tokens sin expiración en texto plano, carecen de aislamiento multi-tenant, omiten atribución de usuario final en auditoría y añaden riesgos de cadena de suministro. Asimismo, cualquier servidor MCP tipo *self-bot* (con token de cuenta personal) infringe los Términos de Servicio y expone la cuenta a suspensión permanente.
- **Bot de Discord como Cliente MCP (recomendado en producción):** el bot en `discord.py` o `discord.js` consume servidores MCP externos mediante Streamable HTTP sin estado (revisión 2026) o `stdio` en procesos aislados, aplicando *allowlists*, *denylists* de herramientas destructivas y *tool pinning* (fijación de lista de herramientas).

## 4. Implementación por fases, memoria estratificada y RAG con ACL

1. **Fase 1 (Developer Portal):** resguardar el token (no caduca) en variables de entorno o gestor de secretos (AWS/GCP Secrets Manager, HashiCorp Vault) y rotarlo ante filtración; activar `MESSAGE_CONTENT` solo si hace falta leer canales sin mención (y reiniciar tras cambios); conceder permisos mínimos (`View Channels`, `Send Messages`, `Send Messages in Threads`, `Read Message History`, `Embed Links`, `Attach Files`) y evitar `Administrator`.
2. **Fase 2 (Ejecución asíncrona y formato):** ejecutar `defer` antes de consultar bases de datos o APIs; persistir tareas largas en cola duradera antes de diferir para no dejar cargas colgadas si el contenedor reinicia; enviar ediciones en lotes; validar límites localmente antes de transmitir; abrir un hilo (`Thread`) por consulta larga y asociar su `thread_id`.
3. **Fase 3 (Memoria en cuatro capas y RAG con ACL):**
   - *Memoria de trabajo:* system prompt, esquemas de herramientas y ventana acotada con presupuesto fijo de tokens.
   - *Estado duradero (checkpointing):* persistir el grafo (PostgreSQL o Redis con LangGraph) indexado por `(guild_id, channel_or_thread_id)`.
   - *Memoria semántica/episódica de largo plazo:* herramientas como Mem0 o Zep (`add()`, `search()`) aisladas estrictamente por `(guild_id, user_id)`.
   - *RAG con control de acceso (ACL):* búsqueda híbrida (vectores densos + BM25) en Qdrant, pgvector, Pinecone o Chroma con metadatos `(guild_id, channel_id, required_role_ids)` filtrados obligatoriamente contra los permisos vigentes del usuario solicitante antes de inyectar fragmentos al prompt (para prevenir fuga de canales privados, `LLM02:2026`).

## 5. Agentes de voz en tiempo real y cifrado DAVE (2026)

Desde el **1 de marzo de 2026**, el protocolo **DAVE** (*Discord Audio & Video End-to-End Encryption*) es obligatorio en todas las llamadas de audio y video en Discord (servidores, DMs, grupos y Go Live), con excepción únicamente de los *Stage Channels*. Funciona en dos niveles: intercambio de claves de grupo con **MLS (RFC 9420)** y cifrado **AES-128-GCM** por fotograma Opus sobre WebRTC Encoded Transforms.

La guía advierte de una brecha práctica relevante en 2026 (Discord no ofrece SDK oficial de recepción de audio para bots):
- **Envío de audio (TTS):** estable en `discord.py 2.7+` (con `davey`) y `@discordjs/voice` (con `@snazzah/davey`).
- **Recepción de audio (STT):** rota tras marzo de 2026 en `discord-ext-voice-recv` (entrega flujos corruptos y depende de PRs comunitarios para descifrar con `DaveSession`) y con fallos documentados en `@discordjs/voice v0.19.x` (`discord.js#11419`: pérdida de eventos de habla y bucles de reconexión). Como alternativa verificada por la comunidad en Python se cita `py-cord` (rama/PR `#3159`) con `davey >= 0.1.6`, evitando mezclar `py-cord` y `discord.py` en el mismo entorno virtual porque comparten el espacio de nombres `discord`.

Cuatro reglas operativas para voz en tiempo real:
1. **Desacople RTP:** Discord entrega ~50 paquetes/s por hablante; el callback de recepción solo debe encolar bytes asíncronamente sin E/S, red ni inferencia.
2. **Eco propio, SSRC y silencios:** descartar paquetes de `user.bot`, almacenar temporalmente paquetes iniciales con `user = None` por SSRC hasta completar el mapeo y reconstruir silencios mediante marcas de tiempo RTP.
3. **Modular vs. Speech-to-Speech nativo:** tubería `VAD → STT → LLM → TTS` con Pipecat o LiveKit frente a OpenAI Realtime API (`gpt-realtime` / `gpt-realtime-mini` con VAD de servidor por `threshold`, `prefix_padding_ms` y `silence_duration_ms`) o Gemini Live API.
4. **Rotación en Gemini Live:** anticipar desconexiones periódicas del WebSocket cada ~10 minutos abriendo nueva sesión hacia el minuto 9 y transfiriendo el contexto.

## 6. Seguridad, privacidad y Zero Data Retention (ZDR)

La edición de agosto de 2026 del **OWASP Top 10 para Aplicaciones LLM** (75% votación de especialistas + 25% análisis de 7,714 incidentes reales, 6,639 clasificables) coloca en cabeza: `LLM01:2026` Inyección de Prompts, `LLM02:2026` Divulgación de Información Sensible y `LLM03:2026` Agencia Excesiva (junto al *OWASP Top 10 for Agentic Applications 2026*, `ASI01–ASI10`). `LLM08:2026` (*Hidden Context Exposure*) subraya que el system prompt no es un control de seguridad.

Controles deterministas fuera del modelo propuestos para Discord:
1. **Prevención de Confused Deputy y jerarquía de roles:** validar permisos reales del solicitante frente al objetivo en código; situar el rol del bot por debajo de administradores y moderadores.
2. **Aprobación humana (HITL) en el límite de la acción:** usar al LLM como triaje/alerta y pausar acciones destructivas o irreversibles tras botones (`Aprobar` / `Rechazar`) restringidos a moderadores.
3. **Atribución en Audit Log (`X-Audit-Log-Reason`):** enviar en el encabezado HTTP (1–512 caracteres UTF-8 URL-encoded) el ID del solicitante, caso y motivo; replicar eventos en base de datos propia y `#mod-log` porque Discord solo retiene el Audit Log **45 días**.
4. **Validación de salidas (`LLM10:2026`) y cuotas (`LLM06:2026`):** tratar salidas del LLM como entrada no confiable; imponer límites de gasto, solicitudes, pasos del bucle y presupuestos de tokens en servidor ignorando parámetros del cliente.

En cumplimiento normativo (RGPD, UK GDPR, LGPD, CCPA/CPRA, Discord Developer Policy y Sección 5 de los Developer ToS):
- **Cláusula 21:** prohíbe usar mensajes obtenidos vía API de Discord para entrenar ML/IA sin permiso expreso, así como scraping, venta de datos o cesión a redes publicitarias o corredores.
- **Transparencia y borrado:** publicar Política de Privacidad en el portal y en el bot (`/privacy` o `/help`) detallando proveedores, ofrecer borrado/modificación (`/delete-my-data`) y purgar registros al salir de un servidor.
- **No entrenamiento vs. retención vs. ZDR:**

| Proveedor / Plataforma LLM (Verificado oct–nov 2026 según fuente) | Entrenamiento en API comercial | Retención por defecto en inferencia síncrona | Disponibilidad y alcance de Zero Data Retention (ZDR) |
| --- | --- | --- | --- |
| OpenAI API | No por defecto (opt-in disponible) | Registros de monitoreo de abuso hasta 30 días | Previa aprobación organizacional para Chat Completions y Responses API (con Private Safety Processing desde sept. 2026 para frontera). Excluye `/v1/files` y `/v1/batches`. |
| Anthropic Claude API | No bajo términos comerciales | Reducida en 2026 para llamadas sin estado; 30 días obligatorios en Covered Models (hasta 2 años si hay infracción marcada) | Acuerdo ZDR comercial por organización para APIs sin estado (Messages). Excluye Covered Models (Fable/Mythos 5 y 5.1 sin autorización expresa), Files API, Batch y conector MCP administrado. |
| Google Gemini Developer API | No en nivel de pago (revisar términos según nivel) | Monitoreo de abuso; interacciones almacenadas por defecto (`store=true`) hasta 55 días en nivel de pago | Sin programa ZDR documentado; Google remite a Vertex AI para exenciones. |
| Google Cloud Vertex AI / Agent Platform | No sin permiso expreso | Caché implícita hasta 24 h (desactivable) y logs de abuso salvo exención | Soporta ZDR vía exención con equipo de cuenta, fijando `store=false` y desactivando caché durable. |
| Amazon Bedrock / Fireworks AI | No | Sin escritura duradera en inferencia síncrona (modo `none` en Bedrock; modelos abiertos en Fireworks) | ZDR por defecto en llamadas síncronas sin estado (existen excepciones legales como CSAM en AWS/OpenAI). |
| DeepSeek API | Consultar términos vigentes | No se documenta exención de retención | Sin programa ZDR documentado en revisiones de octubre de 2026. |

## 7. Economía (TCO), GPU propia, hosting y checklist de producción

La fuente afirma una brecha de **100x a más de 150x** entre modelos ligeros y de frontera (noviembre de 2026) y modela un escenario agéntico típico: ~3,500 tokens de entrada × 1.6 llamadas por interacción = **5,600 tokens de entrada** (con 65% de prefijo estático atendido por caché de prompts) y **450 tokens de salida** (92.6% entrada). Al comparar un modelo de frontera sin caché frente a un **enrutador híbrido 80/20 con caché** (80% a `GPT-6 Luna` o `Claude Haiku 5.5` a `$0.10/$0.50` por MTok y 20% complejo a `GPT-6.1 Sol` o `Claude Sonnet 5.5` a `$2.00/$10.00` por MTok), el ahorro modelado es del **86.2%**.

Sobre auto-hospedaje en GPU dedicada (citas a precios de Spheron de noviembre de 2026):
- **NVIDIA H100 (`$2.39/h`, ~`$1,721/mes` 24/7):** solo supera en economía a la API de frontera con caché (`$1.4820/MTok` mixta) a partir de **1,161 millones de tokens/mes** (~38.7 millones/día).
- **L40S (referencia ilustrativa ~`$1.00/h`, ~`$720/mes`):** frente a modelos ligeros con caché (`$0.0756/MTok` mixta) requeriría más de **9,500 millones de tokens/mes**.
- Un análisis independiente de Spheron citado frente a DeepSeek ubica el punto de equilibrio entre **831 y 933 millones de tokens diarios**. Salvo soberanía estricta de datos o volúmenes extremos, las APIs comerciales con caché y enrutamiento resultan más económicas según este modelo.

| Plataforma (Precios verificados 2026 según fuente) | Tipo de arquitectura | Costo de entrada estimado | Ventajas y salvedades clave |
| --- | --- | --- | --- |
| Railway (Hobby) | PaaS / Contenedores 24/7 | `$5/mes` (incluye `$5` de uso; por segundo) | Despliegue desde Git, límites duros de gasto, BD no administradas. |
| Fly.io | Micro-VMs globales 24/7 | Desde `$2.19/mes` (`shared-cpu-1x`, 256 MB RAM) | Baja latencia regional; 256 MB es justo para stacks Python con SDKs de IA (mejor 512 MB–1 GB). |
| Render (Background Worker) | PaaS 24/7 | `$7/mes` (512 MB RAM, 0.5 CPU) | Sin hibernación para Gateway; evitar Web Service gratuito (duerme tras 15 min). |
| Hetzner Cloud / DigitalOcean | VPS Linux (Docker / Coolify) | DigitalOcean `$4/mes` (512 MB); Hetzner CX23 ~`€5.99/mes` (`€5.49` + `€0.50` IPv4 antes de IVA en DE/FI tras ajuste del 15 jun 2026) | Control total para bot + Redis + Postgres + vector DB (Coolify gratis); Hetzner EE. UU./Singapur desde `€15–17/mes`. |
| Sparked Host / PebbleHost | Hosting gestionado de bots 24/7 | `$2–$3/mes` (1 GB RAM) | Tarifa fija predecible para bots conectados a BD gestionadas externas (Supabase, Qdrant Cloud). |
| Cloudflare Workers | Serverless HTTP Interactions | Plan gratuito; Workers Paid `$5/mes` (10M req, 30M ms CPU) | Sin Gateway; exige colas para tareas diferidas tras responder y no descarga adjuntos CDN no efímeros. |

La guía cierra con **cinco bloques de verificación antes de pasar a producción**: (1) intents mínimos y cero cierres 4014 (documentar justificación antes de 10,000 usuarios únicos); (2) `defer` en `<1 s`, E/S asíncrona y colas persistentes ante reinicios; (3) streaming por lotes, respeto estricto de `retry_after` y validación local de longitudes; (4) pruebas adversariales de inyección directa/indirecta, permisos de moderador, ACL en RAG, botones HITL y `X-Audit-Log-Reason`; y (5) trazabilidad (LangSmith, OpenTelemetry, Sentry), topes diarios de gasto, Política de Privacidad publicada y comando `/delete-my-data`.

## Limitaciones y procedencia

- **Metadatos ausentes:** el archivo recibido (`pasted-text.txt`) no indica autor, URL ni fecha de publicación; no se infirieron ni completaron externamente.
- **Formato original:** el texto llegó como un único bloque continuo sin saltos de línea; en este resumen se estructuró en tablas y apartados manteniendo el original sin cambios en `fuentes/`.
- **Sin verificación externa:** todas las cifras, precios, versiones, fechas de 2026, nombres de modelos (`GPT-6 Luna`, `GPT-6.1 Sol`, `Claude Haiku 5.5`, `Claude Sonnet 5.5`, `Fable`/`Mythos 5` y `5.1`), políticas ZDR y notas de deprecación provienen exclusivamente del documento fuente y no fueron contrastados en la web.
- **Modelos frente a mediciones:** el ahorro del 86.2% y los umbrales de equilibrio frente a GPU propia dependen de los supuestos de tráfico, longitud de contexto y mezcla de modelos asumidos por la propia guía, no de mediciones en producción.

## Páginas relacionadas

- [Agentes de IA en Discord](../temas/agentes-ia-en-discord.md): síntesis de decisiones de diseño, seguridad, voz y economía aplicada a bots en Discord.
- [Flujos de trabajo agénticos](../temas/flujos-de-trabajo-agenticos.md): guía general de arquitectura de sistemas agénticos en producción (fuente independiente, con contrastes en el modelo de enrutamiento).