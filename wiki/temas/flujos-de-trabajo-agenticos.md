# Flujos de trabajo agénticos

Síntesis de la [guía de flujos agénticos en producción](../fuentes/flujos-agentes-produccion-2026.md). Lo que sigue procede de esa fuente salvo la interpretación final y la comprobación limitada de Postman y Gartner señalada abajo. Las restantes cifras, fechas y nombres de productos no se verificaron de forma independiente. [Auditoría](../auditorias/verificacion-fuentes-2026-11-19.md).

## Idea que organiza todo lo demás

Un agente en producción es un modelo (razonamiento) más un arnés (orquestación, herramientas, memoria, ejecución durable, aislamiento, observabilidad y seguridad). El resultado depende más del arnés y de la disciplina del contexto que del modelo que se elija. Y la regla de arranque es siempre la misma: el patrón determinista más simple que resuelva el problema; la autonomía se gana después, y solo si los datos de evaluación lo justifican.

En el [comunicado de Postman sobre su encuesta](https://www.financialcontent.com/article/bizwire-2026-11-18-one-in-four-organizations-now-run-ai-agents-on-apis-in-production-postmans-2026-state-of-the-api-report-finds), el 25% de las organizaciones ejecuta agentes que usan API en producción, el 25% tiene controles para agentes, el 22% controles para MCP y el 40% no ha abordado credenciales e identidad de agentes. El 22% no es la proporción de servidores MCP protegidos. Un fragmento de búsqueda atribuido a [Gartner](https://www.gartner.com/en/newsroom/press-releases/2025-06-25-gartner-predicts-over-40-percent-of-agentic-ai-projects-will-be-canceled-by-end-of-2027) recoge una predicción de cancelación de más del 40% de proyectos agénticos antes de fines de 2027; la página estaba bloqueada y no se trata de un hecho consumado. [Auditoría](../auditorias/verificacion-fuentes-2026-11-19.md).

## Las cinco capas

| Capa | Pregunta que responde | Riesgo si se descuida |
| --- | --- | --- |
| Topología de control | ¿Quién decide el siguiente paso: el código o el modelo? | Deriva del objetivo, sobrecoste por coordinación |
| Enrutamiento de modelos | ¿Qué modelo toca cada tipo de llamada? | Facturas que crecen 5 a 30 veces más que un chatbot |
| Contexto y memoria | ¿Qué entra en la ventana en cada turno y qué sobrevive entre sesiones? | Contaminación de la atención, coste por turno, deriva de contexto |
| Interoperabilidad y aislamiento | ¿Con qué sistemas habla el agente y dónde se ejecuta su código? | Inyección de prompts, exfiltración, escape del anfitrión |
| Ejecución durable | ¿Qué pasa si el proceso muere a mitad de una tarea? | Reintentos que duplican pagos, correos o escrituras |

Las atraviesa la seguridad determinista (validación fuera del modelo) y la observabilidad continua (trazas desde el primer prototipo).

## Topología: el patrón más simple que alcance

Siete patrones ordenados de mayor a menor control, del pipeline determinista al bucle autónomo ReAct. La guía insiste en no saltar directamente al multiagente: el impuesto de coordinación crece de forma no lineal, los beneficios se aplanan alrededor de los cuatro agentes y con frecuencia los supuestos especialistas son el mismo modelo con distintos prompts.

El criterio que propone para justificar una arquitectura multiagente es concreto: hace falta al menos uno de estos requisitos reales, y son dos o más a la vez: usar modelos distintos, segregar permisos de herramientas, ejecutar trabajo verdaderamente concurrente o aislar ventanas de contexto. El dato que respalda el extremo bueno es que el patrón Orquestador-Trabajador de Anthropic superó en un 90.2% a un agente único, y un caso logístico pasó de más de una semana a menos de 72 horas.

Los antipatrones a evitar desde el diseño inicial: el «Agente Dios» con 15 o más herramientas (la guía propone un máximo aproximado de cinco por agente), la iteración sin techo (límites duros de pasos, traspasos y tokens en código, nunca en el prompt), los contratos en prosa libre entre agentes (esquemas tipados en cada frontera), las salas de chat abiertas en rutas críticas y la actuación sin barreras en el mundo real.

## Herramientas, protocolos y aislamiento

La regla de decisión entre protocolos es simple y resolve la mayoría de las decisiones: llamadas de función en proceso para lógica local, MCP para acceder a herramientas o bases de datos sin autonomía, y A2A solo para delegar trabajo entre agentes independientes que no comparten confianza.

MCP es el estándar consolidado para agente-a-herramienta, con OAuth 2.1 y PKCE para servidores HTTP que protegen datos privados, Client ID Metadata Documents como sustituto del registro dinámico de clientes (deprecado tras comprobarse que solo el 4% de 660 endpoints lo soportaban) y prohibición de reenviar el token del cliente hacia APIs upstream. A2A cubre la colaboración agente-a-agente con Agent Cards firmadas y nueve estados de tarea. Cuando el agente ejecuta código, el aislamiento no es opcional: microVM o kernel, con salida de red denegada por defecto.

En la automatización de navegador, el cambio de 2026 es de píxeles a árbol de accesibilidad (Playwright MCP), pero el techo es el mismo: ningún enfoque superó el 52% de éxito autónomo en una evaluación de 30 flujos complejos, todos fallan ante MFA y el gasto de tokens puede superar tres millones por ejecución. Por eso la aprobación de acciones irreversibles no puede quedar en manos del propio agente.

Para la capa de obtención de datos —la que alimenta a estos flujos— ver [raspado web y extracción de datos](raspado-web-y-extraccion-de-datos.md).

## Contexto y memoria

El contexto se arma en dos capas: un prefijo estático cacheable (instrucciones en la «altitud Ricitos de Oro», definiciones de herramientas, pocos ejemplos buenos y malos) y un sufijo dinámico por turno (estado de la tarea, recuperación con presupuesto fijo, historial reciente). La pauta operativa es no pasar del 60–80% de la ventana. Para horizontes largos: compactación, notas estructuradas externas y subagentes que devuelven síntesis de 1.000 a 2.000 tokens en vez de material crudo.

La memoria madura separa cuatro capas (trabajo, sesión, episódica, semántica o relacional) y converge en diseños híbridos de vectores más grafo, porque los vectores pierden relaciones y evolución temporal. Lalesson operativa de la fuente: no elegir proveedor por sus propios benchmarks, conservar las transcripciones crudas en la propia base de datos, y recordar que la memoria individual de veinte agentes no se compone sola sin un grafo o un servidor MCP compartido.

## Ejecución durable

El historial de chat no es ejecución durable. Un motor durable persiste cada frontera completada en un diario inmutable y reconstruye el estado exacto si el proceso falla, sin volver a pagar llamadas ya completadas. Las opciones que la fuente compara son Temporal, DBOS sobre Postgres, Inngest, Restate y el checkpointer de LangGraph.

La regla que no se negocia: claves de idempotencia deterministas en toda operación que escriba en sistemas externos, porque ningún motor evita duplicados si el worker cae entre la mutación en Stripe o el correo y el registro del recibo. Y los secretos nunca viajan en claro dentro del historial del workflow.

## Economía de tokens

El multiplicador de consumo de un flujo agéntico es de 5 a 30 veces el de un chatbot convencional, y un caso real de la fuente muestra una diferencia de 416x por consulta entre el modelo más caro y el más barato. Las cuatro palancas: enrutamiento escalonado 70/20/10, esfuerzo de razonamiento siempre explícito, disciplina estricta de caché de prefijos y topes duros con disyuntores automáticos.

El dato más accionable: mover la memoria de trabajo dinámica fuera del prefijo estático (del system prompt a un mensaje de usuario final) llevó el acierto de caché del 7% al 84% y recortó el gasto total un 59%. La caché de prefijos no descuenta tokens de salida, y el orden estable-a-volátil del contenido es lo que sostiene el acierto.

## Seguridad

El principio es Mínima Agencia: no basta con mínimo privilegio, hay que acotar cuánta autonomía tiene el agente para actuar sin verificación externa. Una restricción escrita en el prompt no es un control de seguridad; el modelo propone y un servicio determinista externo autoriza validando identidad, límites, reglas de negocio y estado de la transacción.

El catálogo de referencia es el OWASP Top 10 for Agentic Applications (2026), de ASI01 a ASI10, con su control mínimo por caso: separar instrucciones del contenido recuperado (secuestro de objetivo), Identidades No Humanas con credenciales efímeras por agente, inventario AIBOM con versiones fijadas y procedencia firmada para servidores MCP y skills, sandboxes con egress denegado, validación de escrituras en memoria, Agent Cards firmadas, límites de fan-out y circuit breakers, consentimiento fuera del chat con Step-Up MFA, y un kill switch probado. CaMeL se presenta como horizonte de investigación (77% de tareas resueltas con garantías frente a 84% sin defensas en AgentDojo), con la advertencia de que no está listo para producción.

## La métrica que decide si se despliega

Aquí está el punto que más se confunde en la práctica. `pass@k` (al menos uno de k intentos acierta) sirve para flujos asistidos por humanos; `pass^k` (todos los k intentos aciertan) es obligatoria en pipelines automatizados que envían o modifican cosas sin revisión. Con un 75% de acierto unitario, `pass@3` da un 98.4% engañoso y `pass^3` da un 42.2%. Con un 90% unitario, tres ejecuciones impecables bajan al 72.9% y diez al 34.9%. Para exigir `pass^3 >= 95%`, la fiabilidad individual tiene que superar el 98.3%.

La consecuencia práctica: empezar desde la primera semana con 20 a 50 tareas reales, verificadores deterministas en código y jueces LLM con rúbricas de 0 a 5, con oráculo determinista para que la métrica no se contamine.

## Interpretación aplicada a esta biblioteca

Esta sección es interpretación propia, no contenido de la fuente. Aplicada al funcionamiento de esta bóveda, la guía sugiere un orden de trabajo que ya coincide con el que se está usando: empezar por el patrón determinista (capturar el original sin tocarlo, resumir, indexar, registrar) antes de introducir automatizaciones con más capacidad de decisión; tratar cada agente con permisos acotados a su función; y usar la evaluación como criterio de mejora, no la intuición.

## Fuentes y límites

- [Resumen de la fuente](../fuentes/flujos-agentes-produccion-2026.md): guía de arquitectura, ejecución y producción de sistemas agénticos en 2026, recibida como texto pegado.
- [Plugins para trabajo de conocimiento](plugins-para-trabajo-de-conocimiento.md): otra entrada de la biblioteca que toca el mismo terreno desde los skills y conectores.
- [Agentes de IA en Discord](agentes-ia-en-discord.md): aplicación de arquitectura agéntica, MCP, controles deterministas, memoria y enrutamiento de modelos a las restricciones de Discord (fuente independiente; modela un reparto 80/20 frente al 70/20/10 de esta guía).

Límites: el original es una respuesta generada por Gemini, sin autor humano ni fecha de diálogo. El [registro histórico](../../registro.md) documenta una comparación previa de las tablas; la conversación no permitió acceso al texto el 2026-11-19. La [auditoría](../auditorias/verificacion-fuentes-2026-11-19.md) solo contrasta las cifras delimitadas de Postman y una predicción de Gartner mediante fragmento de búsqueda. Las demás cifras, licencias, precios y benchmarks no se verificaron independientemente; varias afirmaciones son de proveedor.
