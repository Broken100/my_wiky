# HERMES 2026: Curso COMPLETO en Español (Agente IA)

- **Origen:** [HERMES 2026: Curso COMPLETO en Español (Agente IA)](<../../fuentes/HERMES 2026 Curso COMPLETO en Español (Agente IA).md>). El archivo contiene la descripción del video y la transcripción completa del audio.
- **URL de origen:** <https://www.youtube.com/watch?v=FQZGoSwdS_0> (según los metadatos del archivo).
- **Autor:** Benjamín Cordero (metadatos del archivo).
- **Publicado:** 2026-05-22, según los metadatos del archivo.
- **Tipo:** transcripción de un curso en video de más de cuatro horas, con 56 temas anunciados y proyectos construidos en vivo.
- **Capturado:** 2026-11-18.

## Idea central

El curso sostiene que en 2026 la inteligencia se está volviendo un insumo intercambiable y que la ventaja competitiva se desplazó al **arnés**: el entorno que rodea al modelo y que aporta el bucle de ejecución, el contexto, la memoria persistente, las herramientas y la automatización temporal. Hermes, un agente de código abierto de Nous Research con licencia MIT, se presenta como un arnés que corre de forma continua en un servidor y se opera desde un canal de mensajería, de modo que el trabajo del agente no depende de que el usuario esté frente a la computadora.

Sobre esa base, el curso desarrolla una progresión práctica: entender qué diferencia a un agente de un chatbot, instalar Hermes en un VPS, conectarlo a un canal y a un modelo, construir los archivos que definen su comportamiento, acumular *skills* y conexiones MCP, programar tareas con *cron* y, finalmente, encadenar todo en siete proyectos de negocio.

## De chatbot a agente: loop, contexto, memoria y herramientas

El punto de partida conceptual (15:33–27:16) es la distinción entre un modelo de lenguaje que responde y un agente que actúa:

- Un **chatbot** recibe una entrada y devuelve un mensaje; no tiene forma de publicar una página ni de entrar a un correo. El presentador lo ilustra con la respuesta típica de un asistente que dice que sabe hacer algo pero no tiene acceso.
- Un **agente** itera: crea el archivo, comprueba que falta publicarlo, busca la herramienta que lo publica, se conecta y vuelve a comprobar. Ese ciclo de decisión y reintento es el *agent loop*.
- Las conexiones con el exterior se hacen por **API** o por **MCP**; el curso describe el segundo como el estándar en curso y el primero como lo que todavía se sigue usando porque no todo migró.

Los cuatro elementos que el curso usa como esquema de todo agente son: **loop**, **contexto**, **memoria** y **herramientas**. El contexto dura lo que dura la sesión y se llena hasta un umbral, tras el cual se compacta; la memoria, en cambio, vive en archivos dentro de una carpeta y sobrevive a la compactación y al cierre de sesión. Las herramientas son las llamadas a aplicaciones externas, porque el modelo por sí solo no accede al mundo.

El curso advierte además que el rendimiento se degrada a medida que el contexto se satura, incluso en ventanas grandes, y que por eso conviene reiniciar sesión y compactar en lugar de acumular conversación (2:03:19, 3:38:13–3:39:11).

## El arnés por encima del modelo

Con la analogía del jinete y el caballo (27:20–31:23), el curso separa dos cosas que suelen confundirse: el **modelo** es el caballo, mucha potencia y ninguna dirección; el **arnés** es lo que le da dirección y encuadre. Hermes, Claude Code, Codex y OpenClaw pertenecen a la segunda categoría.

De ahí se derivan tres afirmaciones que el presentador repite:

1. El arnés es el entorno donde el modelo ejecuta, y las mejoras de producto de 2026 se concentran tanto o más en el arnés que en el modelo.
2. Conviene estructurar los proyectos para poder migrar de arnés sin rehacer el trabajo.
3. El arnés no sustituye al modelo ni al revés: el curso insiste en que Hermes «no es mejor» que Claude Code, sino distinto, y que el presentador usa ambos con propósitos diferentes: Claude Code para trabajo concentrado frente a la pantalla, Hermes para operar desde el teléfono (38:57–40:42).

## Nous Research y la licencia MIT

El curso identifica a Nous Research como creador de Hermes y atribuye parte de su adopción a dos decisiones (31:23–35:19): el código abierto y la licencia MIT, que permite construir productos propietarios encima. Como ejemplo menciona a Higgsfield, que según el presentador habría construido su *supercomputer* sobre el tiempo de ejecución de Hermes; la comprobación que muestra es una pregunta hecha al propio agente, no una verificación independiente.

También atribuye al proyecto más de 150.000 estrellas en GitHub (15:05) y lo describe como uno de los proyectos de código abierto de crecimiento más rápido. Son afirmaciones del presentador, no datos comprobados en la fuente.

## Los cinco pilares

El curso enumera cinco rasgos que, según el presentador, explican el comportamiento de Hermes (6:48, 36:01–37:15, 4:06:17):

| Pilar | Qué aporta |
| --- | --- |
| **Memoria** | Archivos que persisten entre sesiones; el presentador la destaca como la función que mejor resuelve el «no se acuerda de nada» |
| **Skills** | Procedimientos estandarizados en Markdown que se generan solos cuando detecta un patrón repetido |
| **Soul** | Archivo que define carácter, tono y forma de comportarse |
| **Cron** | Tareas programadas en lenguaje natural que se ejecutan sin intervención |
| **Self-improving loop** | El agente reescribe y mejora sus propios archivos y procedimientos con el uso |

El cierre conceptual del curso es que, quitando la capa de modelo, todo el sistema son «archivos en una carpeta que se referencian entre sí».

## Instalación: local, VPS y Docker frente a root

La instalación que muestra el curso (37:41–50:24) consiste en ejecutar una única línea obtenida en la página de Hermes. La transcripción no reproduce el comando, de modo que ese paso no es reproducible solo con este texto.

- **Local:** funciona en macOS o Windows, pero exige dejar el equipo encendido de forma permanente; el presentador sugiere un equipo dedicado antes que la computadora de trabajo.
- **VPS:** la demostración usa Hostinger, con planes KVM-1 o KVM-2, plantilla de Hermes Agent sobre Ubuntu y un coste que el curso cifra desde 5 dólares al mes con el enlace promocional.
- **Docker frente a root:** *root* instala Hermes a nivel del sistema y reserva el servidor para eso; *Docker* lo aísla en un contenedor y deja espacio para otros servicios en la misma máquina. La demostración sigue la ruta Docker porque se instala con un clic desde el panel.
- **Onboarding:** elección entre *quick setup* y *full setup*, proveedor de inferencia, autenticación, modelo y entorno de ejecución. El curso elige *quick setup* y luego *local* dentro del contenedor (50:24–1:00:20).

Un detalle operativo que aparece más tarde (1:11:24): al correr dentro de Docker, la instalación del gateway como servicio persistente falla, y el curso lo deja anotado sin resolverlo en ese momento.

## Modelos y proveedores

El curso distingue modelos abiertos, que pueden ejecutarse en local o en la propia nube, de modelos cerrados, que se pagan por uso al proveedor (50:54–56:20). Sobre esa base presenta tres vías:

- **API directa** de cada proveedor, con una clave por proveedor.
- **OpenRouter** como intermediario de pago por uso que unifica modelos abiertos y cerrados con una sola clave.
- **Suscripción**, que es la que usa la demostración: autenticación OAuth contra la suscripción de ChatGPT que incluye Codex, con autorización por código de dispositivo y selección posterior del modelo. El presentador señala que el mismo mecanismo existió para los planes de Anthropic y que ya no estaba disponible al grabar.

El argumento sobre los modelos gratuitos es que el pago puede hacerse con dinero o con datos, y que conviene tenerlo presente al elegir proveedor. El curso también registra que el uso de la suscripción se agota y que el presentador amplía el plan a mitad del curso (2:25:19–2:26:53); no detalla los límites exactos.

## Canales y gateway

El canal es el intermediario entre el usuario y el agente: el mensaje entra por Telegram, WhatsApp, Slack, Discord, correo, SMS o Teams, el agente lo procesa en el servidor, ejecuta acciones y responde por el mismo canal (1:00:46–1:02:44).

La configuración de Telegram que muestra el curso (1:04:17–1:12:21) tiene tres piezas: un bot creado en BotFather —advirtiendo que hay que usar la cuenta verificada—, el token del bot y el identificador numérico del usuario, que se autoriza enviando un mensaje al bot para que el agente lea el último *update*. Esa autorización se presenta como la capa de seguridad principal: solo ese usuario puede hablar con el agente.

El curso añade que la interfaz de Telegram y la de terminal son prácticamente la misma, y que a partir de ese momento todo el trabajo se hace desde Telegram.

## Comandos, interfaz y diagnóstico

Los comandos que el curso usa y explica (1:17:06–1:19:48) son: `/new` o `reset` para iniciar sesión, `/sessions` para recuperar sesiones previas, `/compress` para compactar, `/title`, `/rollback`, `/goal` para sostener un objetivo hasta cumplirlo, `/status` para ver consumo de tokens, `/help` y el modo *yolo*, que omite las confirmaciones de permisos.

El diagnóstico se apoya en `hermes doctor`, que revisa el estado, y `hermes doctor --fix`, que intenta reparar los errores encontrados (1:03:43, 1:15:57–1:16:52). El curso lo describe como un mecanismo de autocuración y recomienda volver a él cuando algo se rompe. También menciona `hermes setup tools`, `hermes gateway` y `hermes chat` como comandos de configuración y arranque.

## Skills: procedimientos que se escriben solos

Un *skill* es un SOP —procedimiento operativo estándar— en formato Markdown: la receta de cómo se hace algo, con pasos, criterios y las conexiones MCP que hay que usar (1:22:23–1:25:04). El ejemplo recurrente es la miniatura de YouTube: se itera con el agente hasta que el resultado gusta y entonces se le pide convertir ese ida y vuelta en un skill, para no repetir la conversación la próxima vez.

Dos propiedades importan en el argumento del curso:

- **Se generan de forma autónoma.** El agente detecta un patrón repetido y estandariza el procedimiento sin que se lo pidan, algo que el presentador usa como argumento de venta frente a otros arneses.
- **Sobreviven al contexto.** Al estar en archivos, no se pierden cuando la sesión se compacta; por eso se presentan como ventaja acumulativa y no como configuración puntual.

El curso menciona 91 skills incluidos y 684 disponibles para instalar (8:23), y dos catálogos: agentskills.io y el Skills Hub de Hermes, con skills oficiales de Nous Research y de terceros verificados. La advertencia es explícita (4:12:59–4:14:22): no instalar skills de cualquier autor, porque se han publicado skills con código malicioso y el presentador atribuye a OpenClaw problemas de ese tipo.

## MCP: el puente hacia las herramientas

El curso define MCP —Model Context Protocol, transcrito casi siempre como «MSP»— como el protocolo creado por Anthropic que actúa de traductor universal entre un agente y aplicaciones o bases de datos (1:25:04–1:30:04). La ventaja frente a la integración por API es la estandarización: una misma interfaz sirve para conectar cosas distintas.

Conexiones que se configuran en el curso: Playwright para automatizar interfaces, Google Workspace, HyperFrames para video, Higgsfield para imagen y video, GitHub para respaldos y GoHighLevel para CRM. La instalación de Higgsfield (2:28:19–2:32:49) se resuelve con autorización por enlace y un `reset` o reinicio para que las herramientas MCP aparezcan en la sesión.

La tesis práctica del curso es que el valor aparece al **combinar** MCP: generar la imagen en Higgsfield, publicarla en Meta Ads, desplegar una página en Vercel. También señala el límite: lo que no tiene API ni MCP se resuelve con automatización de navegador.

## Archivos del cerebro

La estructura que muestra el curso (1:48:16–1:58:47) es una carpeta con archivos que se referencian entre sí: `agents.md` con el rol, `soul.md` con la identidad y el carácter, `user.md` con datos del usuario, `memory.md` con memoria persistente, más carpetas de skills y de sesiones.

Dos ideas se subrayan. Primero, que al inicio de cada sesión se precarga una capa que el usuario no ve —el contenido de esos archivos— además del mensaje escrito; entender eso explica el consumo de tokens. Segundo, que esos archivos deben mantenerse cortos, con una recomendación de no pasar de 200 líneas, porque se cargan siempre.

## Cron: las tres formas de automatizar

El curso distingue tres disparadores de automatización (2:03:23–2:09:20):

1. **Manual:** una persona acciona el disparador.
2. **Por evento:** un webhook o una llamada HTTP reacciona a algo, como un formulario enviado.
3. **Programado:** un *cron* que se ejecuta cada cierto tiempo o en un horario fijo.

La novedad que el curso destaca es que el cron se define **en lenguaje natural**: «todos los días a las 9 de la mañana, un reporte de noticias de IA». El planificador trabaja en UTC y el agente hace la conversión horaria, como muestra al programar las 9:00 de Santiago a las 13:00 UTC (2:11:57).

La comparación con otros arneses es concreta: las rutinas locales de Claude Code requieren el equipo encendido y las rutinas en la nube tienen un cupo acotado —el presentador menciona quince—, mientras que un cron en un VPS permite muchas más y de forma más fiable, según su experiencia.

## Contexto, compactación y subagentes

El curso vuelve varias veces sobre el manejo del contexto porque lo considera la habilidad que separa a quien usa bien estos sistemas de quien los usa mal:

- La compactación automática reduce la sesión cuando llega a la mitad de la ventana y conserva lo relevante, pero pierde matices (1:59:12–2:00:19).
- Cuando la sesión ya está inundada, lo correcto es abrir una nueva con `/new` y dejar que los archivos aporten el contexto permanente (3:38:13–3:39:11).
- Los **subagentes** resuelven el problema por otra vía: un agente orquestador reparte tareas a subagentes en paralelo y recibe solo la conclusión (4:17:39–4:22:14).

El ejemplo que usa es ilustrativo, no una medición: cinco tareas de investigación y redacción pasan de diez minutos en secuencia a dos minutos en paralelo, el contexto del agente principal baja de una saturación total a cerca del 20 % y los 100.000 tokens de material se condensan en unos 5.000. El curso cita un artículo de Anthropic sobre subagentes sin dar referencia.

## Seguridad y exposición

Las prácticas que el curso insiste en repetir (11:21, 1:41:01–1:43:10, 4:03:11–4:05:19):

- **No pegar claves en el chat.** La vía recomendada es cargarlas como variables de entorno en el panel del VPS y redesplegar. Las claves creadas para la demostración llevan fecha de expiración corta precisamente porque se muestran en el video.
- **Autorizar un único usuario** en el canal, mediante su identificador numérico.
- **Asumir el riesgo de inyección de prompts.** El curso lo describe como un ataque común y la razón por la que un agente con acceso al correo, al banco o al CRM no debe exponerse al público.
- **Aislar cuando haga falta:** cuentas propias para el agente, Tailscale para cerrar la comunicación y firewall en el servidor.

De ahí sale una distinción que el curso considera clave para vender el servicio: agentes **inhouse**, que operan dentro de la empresa y pueden tocar datos sensibles —el caso de Hermes—, y agentes **outhouse**, que son interfaz con el cliente final y solo deben ver información no sensible; para esos últimos el curso menciona Chatbase como alternativa más simple (3:56:29–4:02:59).

## Los siete proyectos en vivo

| # | Proyecto | Qué se construye | Piezas del arnés |
| --- | --- | --- | --- |
| 1 | Landing page por memorándum de voz | Página HTML para una empresa, publicada con dominio de Vercel | Transcripción local, navegador, token de Vercel en el entorno (1:31:22–1:47:08) |
| 2 | Daily briefing por Gmail | Cron diario que envía un reporte HTML de noticias de IA | Cron, Himalaya CLI, contraseña de aplicación de Gmail, skill de newsletter (2:10:17–2:28:19) |
| 3 | Matías, agente de Meta Ads | Aplicación de Meta y agente con rol de *media buyer* | App de desarrollador, permisos, variables de entorno, token (2:36:05–2:48:54) |
| 4 | Máquina de anuncios | Generación de creatividades y publicación en campañas | Higgsfield MCP, Meta Ads, skill de creatividades (2:48:54–3:03:44) |
| 5 | Fábrica de reels | Videos cortos para campañas de retargeting | HyperFrames MCP, Marketing Studio de Higgsfield, Meta Ads (3:03:44–3:18:26) |
| 6 | Backup automático en GitHub | Copia diaria del agente en un repositorio privado | Cron, script saneado, token *fine-grained*, repositorio creado por el agente (3:18:26–3:30:27) |
| 7 | Pipeline de agencia con GoHighLevel | CRM operado desde Telegram: contactos, oportunidades, correos | Token de integración privada, location ID, variables de entorno (3:30:27–3:51:33) |

**Proyecto 1.** El pedido se hace con una nota de voz desde Telegram y el agente la entiende porque el servidor trae un modelo de transcripción local; el curso contrasta con OpenClaw, donde había que instalar uno. El agente crea el HTML, lo sirve por un túnel temporal y luego lo despliega en Vercel con un token de un día cargado en el entorno. Los cambios posteriores se piden en lenguaje natural —otro enlace de agenda, paleta más oscura— y se vuelven a desplegar.

**Proyecto 2.** El cron se crea en una frase y el agente resuelve el envío instalando Himalaya CLI y guiando la conexión de Gmail con contraseña de aplicación. El curso muestra dos tropiezos reales: un problema de permisos entre el usuario que creó la credencial y el usuario con el que corre Hermes, y un reporte que llegó con el HTML sin interpretar. El segundo error se convierte en el insumo del skill de newsletter: fuentes más fiables, aplicación al negocio propio, color predominante y la estructura correcta del correo.

**Proyecto 3.** La aplicación se crea en el panel de desarrolladores de Meta con los permisos de lectura y gestión de anuncios, gestión del negocio y páginas, se publica y se genera un token. El identificador de la app, el secreto, el token y el identificador de la cuenta publicitaria se cargan como variables de entorno, nunca en el chat. El curso advierte que la integración es legítima pero que un volumen desmesurado de campañas puede traer problemas con Meta.

**Proyecto 4.** Con el rol de *media buyer* y un skill de creatividades instalado, el agente analiza el historial, genera tres gráficos para cinco países y crea una campaña con diez dólares diarios. El presentador verifica la campaña en el administrador de anuncios y luego la desactiva. Señala que la calidad creativa es pobre sin contexto de marca y que conviene un paso de aprobación humana antes de publicar.

**Proyecto 5.** HyperFrames —descrito como framework abierto y gratuito— produce un video de diez segundos con animaciones de estilo HTML; Higgsfield aporta *hyper motion* y plantillas de contenido UGC a partir de una sola foto. El video se sube a una campaña de retargeting, en un caso sin audio porque la pista venía en inglés.

**Proyecto 6.** El agente crea un script de respaldo que excluye secretos y sesiones, programa el cron diario y, al recibir el token, crea el repositorio privado y hace el primer envío. El token se crea con permisos de contenido y administración y caducidad corta. El argumento es que un agente que se reescribe a sí mismo necesita poder volver atrás.

**Proyecto 7.** GoHighLevel se conecta con un token de integración privada y el identificador de ubicación, ambos como variables de entorno. A partir de ahí el agente crea contactos y oportunidades, los coloca en el pipeline y envía correos de seguimiento desde Telegram. El curso explica el embudo —lead, contacto, calificación, propuesta, negociación, cierre— y sostiene que casi cualquier negocio de servicios comparte esa estructura y tiene un cuello de botella identificable.

**Proyecto adicional.** Fuera del guion, el presentador prueba un *prompt* de plantilla de página web y la despliega en Vercel en un solo intento (3:53:04–3:55:45). Lo presenta como muestra de que el entregable puede salir de una sola instrucción cuando el arnés ya tiene las conexiones hechas.

## Migración desde OpenClaw

El cierre menciona que Hermes incluye un comando de migración que abre un proceso de onboarding para traer la configuración y los skills de OpenClaw sin empezar de cero (4:08:17–4:09:37). La transcripción lo recoge como «Hermes Cloud migrate», una forma que no coincide con el nombre del producto y que conviene verificar en la documentación antes de usarlo.

## Cierre

El curso termina con el argumento de que la brecha entre la tecnología disponible y lo que las empresas aplican es la oportunidad de negocio, y con el anuncio de un producto propio —descrito como un motor construido sobre Hermes— que el presentador dice que liberará a su comunidad (4:26:54–4:30:44).

## Limitaciones

- **Es una transcripción de video, no documentación.** Faltan los comandos completos, las pantallas, los valores exactos y buena parte de lo que el presentador hace con el ratón. La instalación, los permisos de Meta, la creación de tokens y las variables de entorno quedan descritas en prosa, sin el detalle necesario para reproducirlas.
- **La transcripción automática deforma nombres técnicos.** «MCP» aparece casi siempre como «MSP»; Claude Code como «Cloud Code»; OpenClaw como «Open Cloud» o «Open Clo»; Skool como «School»; Higgsfield como «Hixfield» o «Xfield». El resumen usa los nombres correctos, pero esa corrección es una interpretación y no un dato del texto.
- **Las cifras del contexto son inconsistentes.** A lo largo del curso se mencionan 128.000, 130.000, 232.000 y 272.000 tokens como ventana, y la compactación se describe como activada cerca de la mitad. No es posible determinar el valor real a partir de la fuente.
- **Hay contradicciones menores.** La descripción anuncia ocho proyectos, los capítulos numeran siete y el recuento final también lista siete; la comunidad pasa de «más de 900 miembros» en la introducción a «1.000 miembros» en el cierre. Se registran tal como aparecen.
- **Acceso al video:** en la comprobación del 2026-11-19 se observó un título coincidente en [YouTube](https://www.youtube.com/watch?v=FQZGoSwdS_0), sin revisar el contenido completo; un intento posterior encontró bloqueo por tráfico inusual. Esto no valida la transcripción ni las afirmaciones. El número de estrellas, la adopción por Higgsfield, los skills, HyperFrames, precios y mecanismos de autenticación siguen sin verificarse de forma independiente. Precios, planes, modelos y funciones pueden haber cambiado. Véase la [auditoría](../auditorias/verificacion-fuentes-2026-11-19.md).
- **El material tiene finalidad comercial.** El curso promociona enlaces de afiliado, una comunidad de pago y productos propios del autor; esas recomendaciones no son neutrales y no se evaluaron.
- **El ejemplo de subagentes son cifras ilustrativas** del presentador, no resultados medidos, y el artículo de Anthropic que cita no se identifica.
- No se inspeccionó ningún producto, repositorio, panel ni servicio mencionado, ni se ejecutó ninguno de los procedimientos descritos.

## Temas y páginas relacionadas

- [Automatización con Hermes](../temas/automatizacion-con-hermes.md)
- [Cómo configurar Hermes](../proyectos-2026/como-configurar-hermes.md): guía previa centrada solo en la instalación, derivada de esta misma transcripción.
