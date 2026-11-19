# Automatización con Hermes

Página de tema construida sobre el [curso completo de Hermes 2026](../fuentes/hermes-2026-curso-completo.md), de Benjamín Cordero. Todo lo que sigue procede de esa transcripción; la única sección marcada como interpretación es la última. Las cifras, precios y nombres de productos son afirmaciones del autor y no se verificaron de forma independiente.

## La tesis: el arnés es el lugar donde se automatiza

El curso ordena todo su argumento alrededor de una distinción: el **modelo** razona, el **arnés** hace. El modelo es potencia sin dirección; el arnés aporta el bucle de ejecución, el contexto, la memoria, las herramientas y el reloj. Hermes, Claude Code, Codex y OpenClaw son arneses distintos sobre modelos que en buena medida se solapan.

De esa distinción se sigue la consecuencia práctica que interesa a esta página: automatizar no es elegir un modelo más capaz, es **construir el entorno** en el que el modelo puede actuar sin supervisión. Y ese entorno está hecho de piezas nombrables, cada una con su función y su límite.

## Cuatro piezas con relojes distintos

El curso usa un esquema de cuatro elementos, y lo más útil es notar que cada uno vive en una escala temporal diferente. Esa diferencia explica por qué un agente «se olvida» de lo que hizo ayer.

| Pieza | Cuánto dura | Dónde vive | Qué problema resuelve |
| --- | --- | --- | --- |
| Loop | Lo que dura la tarea | En la ejecución | Reintentar y decidir el paso siguiente hasta cerrar el objetivo |
| Contexto | Lo que dura la sesión | En la ventana del modelo | Dar material para razonar en el turno actual |
| Memoria | Entre sesiones | En archivos de una carpeta | No repetir el contexto desde cero |
| Herramientas | Permanente | API y servidores MCP | Tocar el mundo: correo, CRM, anuncios, archivos |

La confusión más común que el curso señala —«mi agente no se acuerda de nada»— es en realidad un problema de expectativa sobre esa tabla: el contexto se compacta y se pierde; solo lo que se escribió en los archivos sobrevive.

## Skills: conocimiento operativo que se acumula

Un skill es un procedimiento en Markdown: pasos, criterios y las conexiones que hay que usar. Su valor no está en la sofisticación, sino en que **se escribe después de la conversación, no antes**. El patrón que el curso enseña es siempre el mismo: iterar con el agente hasta que el resultado sirve, y entonces pedirle que convierta ese ida y vuelta en un procedimiento reutilizable.

Eso convierte al agente en algo acumulativo. La primera miniatura cuesta veinte mensajes; la número cincuenta cuesta uno. El curso insiste en que los skills se generan también por iniciativa del agente al detectar patrones, y en que sobreviven a la compactación porque son archivos.

Dos cautelas que la fuente aporta: mantener los archivos base cortos, porque se precargan en cada sesión, y no instalar skills de autores desconocidos, porque se han publicado skills con código malicioso.

## MCP: el puente, y la regla de combinarlos

MCP estandariza la conversación entre el agente y una aplicación externa. El curso lo describe como un traductor universal: en lugar de una integración a medida por servicio, una interfaz común para correo, CRM, generación de imágenes, repositorios o navegador.

Lo importante no es la definición sino la **composición**. Cada conexión por separado es un juguete; el sistema aparece cuando se encadenan. Generar una creatividad en una herramienta de imagen, publicarla en el administrador de anuncios y desplegar la página donde aterriza son tres MCP distintos que, juntos, producen un resultado que ninguno produce solo.

El límite también queda claro: lo que no expone API ni MCP se resuelve con automatización de navegador, que es más frágil y consume más.

## Cron: el agente que trabaja sin que lo llames

El curso distingue tres disparadores de automatización: manual, por evento y por tiempo. El cron es el tercero, y su aporte específico es que se define **en lenguaje natural**, sin sintaxis de programador: «todos los días a las nueve, un reporte de noticias con fuentes primarias».

Ahí aparece la diferencia estructural con otros arneses. Una rutina local exige el equipo encendido; una rutina en la nube tiene cupo. Un agente en un servidor con cron no depende de que nadie esté frente a la pantalla, y esa es, en el argumento del curso, la razón por la que vale la pena pagar un VPS.

## Canales: el agente se opera desde donde estás

El canal es el intermediario entre la persona y el agente: Telegram, WhatsApp, Slack, Discord, correo o SMS. La consecuencia no es cosmética. Cambia el tipo de trabajo que se puede delegar: si el agente solo existe en una terminal, hay que sentarse frente a la terminal; si existe en Telegram, se le puede pedir una campaña desde la calle.

El curso trata la autorización del usuario como parte del diseño y no como un trámite: el identificador numérico autorizado es lo que separa un agente personal de un agente expuesto.

## Contexto: el recurso que se gasta

La página de tema no puede dejar esto fuera, porque el curso lo repite en cada proyecto. El contexto se llena, se compacta y pierde matices; cuando está inundado, el agente parece menos inteligente aunque el modelo sea el mismo. Las tres salidas que la fuente muestra son abrir sesión nueva, compactar a mano y repartir el trabajo entre subagentes que devuelven solo la conclusión.

El ejemplo de subagentes que ofrece el curso —de diez minutos a dos, de contexto saturado a un 20 %, de 100.000 tokens a 5.000— es ilustrativo y no una medición.

## Cómo se encadenan: los siete proyectos

Los proyectos del curso son, vistos en conjunto, un catálogo de patrones de automatización. Cada uno añade una pieza al arnés y reutiliza las anteriores.

| Patrón | Proyecto | Piezas que combina |
| --- | --- | --- |
| Ejecutar y publicar | Landing page por nota de voz | Transcripción local, generación de archivos, despliegue con token |
| Ejecutar y notificar | Daily briefing por Gmail | Cron, cliente de correo, skill de formato |
| Operar un sistema externo | Matías, agente de Meta Ads | App autorizada, permisos acotados, token en el entorno |
| Producir y publicar en volumen | Máquina de anuncios | Generación de imágenes, skill creativo, API de anuncios |
| Producir medios | Fábrica de reels | Video por MCP, plantillas, campaña de retargeting |
| Protegerse | Backup en GitHub | Cron, script saneado, token de escritura |
| Gestionar relaciones | CRM con GoHighLevel | Token de integración, identificador de cuenta, pipeline |

Leídos en secuencia, van de «hacer una cosa y mostrarla» a «operar el sistema donde viven los clientes». El último es el que el curso presenta como más vendible, porque toca el embudo completo del negocio y no una tarea aislada.

## Confianza y secretos

El curso tiene una regla que se repite en cada integración: las claves no se pegan en el chat, se cargan como variables de entorno en el servidor y se redespliega. Las claves de demostración llevan caducidad corta precisamente porque se muestran en el video.

La razón de fondo es que un agente con acceso al correo, al CRM y a la cuenta publicitaria es un objetivo de inyección de prompts. De ahí la distinción entre agentes **inhouse**, que operan dentro de la empresa con datos sensibles, y agentes **outhouse**, que atienden al cliente final y solo deben ver información no sensible. Hermes pertenece a los primeros; para los segundos el curso propone herramientas más simples y acotadas.

## Lectura de conjunto (interpretación)

Lo que el curso describe, más que un producto, es un **modelo de madurez**. En el nivel uno está el agente que responde; en el dos, el que ejecuta una tarea con herramientas; en el tres, el que recuerda y estandariza lo que aprendió; en el cuatro, el que se ejecuta solo según un calendario; en el cinco, el que combina varios sistemas externos en un flujo de negocio.

Esa lectura es mía, no del autor, pero encaja con el orden en que el curso construye las cosas: cada proyecto presupone el anterior y ninguno funciona bien si falta la base. Un cron sin skill produce el mismo error todas las mañanas; un skill sin memoria se reescribe cada semana; un MCP sin contexto genera creatividades genéricas, como el propio presentador comprueba cuando las imágenes que recibe le parecen malas.

La consecuencia práctica de esa lectura: antes de añadir automatizaciones conviene comprobar que las capas anteriores están resueltas, porque un agente que olvida y un agente que se programa solo producen fallos que se multiplican a diario en lugar de una vez.

## Vacíos de esta página

La transcripción no permite reproducir la instalación ni las configuraciones: faltan comandos, pantallas y valores. Tampoco hay mediciones: los números de contexto son inconsistentes en el propio texto y los de subagentes son ilustrativos. Todo lo relativo a precios, planes, modelos disponibles y funciones de terceros refleja lo que el autor describe en la fecha de grabación.

## Páginas relacionadas

- [HERMES 2026: Curso COMPLETO en Español (Agente IA)](../fuentes/hermes-2026-curso-completo.md): resumen de la fuente, con el detalle y los tiempos de cada sección.
- [Cómo configurar Hermes](../proyectos-2026/como-configurar-hermes.md): guía de instalación derivada de la misma transcripción.
- [Flujos de trabajo agénticos](flujos-de-trabajo-agenticos.md): otra fuente, independiente de esta, sobre arquitectura y operación de agentes en producción. Comparte el vocabulario de arnés, contexto y subagentes, pero sus datos y recomendaciones no provienen del curso.
