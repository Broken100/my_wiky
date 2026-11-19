# Cómo configurar Hermes

Esta guía resume el recorrido mostrado en [HERMES 2026: Curso COMPLETO en Español (Agente IA)](<../../fuentes/HERMES 2026 Curso COMPLETO en Español (Agente IA).md>), de Benjamín Cordero, publicado el 22 de mayo de 2026 según los metadatos de la fuente. La transcripción describe una demostración con Hostinger, Docker y Telegram; sus pantallas y opciones pueden haber cambiado y no se han comprobado de forma independiente.

## Preparación

1. Decide dónde ejecutar Hermes: el curso menciona un equipo local o un VPS. Para que funcione de forma continua, el equipo elegido debe permanecer encendido (37:41–41:29).
2. Elige un proveedor de modelo y un método de autenticación. El curso presenta OpenRouter mediante API y muestra una sesión con la opción de suscripción de Codex; la disponibilidad, los costes y las condiciones no están verificados (50:46–59:53).
3. Si quieres usar Telegram, necesitarás una cuenta y un bot. Trata los tokens y claves como datos privados; el propio curso desaconseja introducir claves en el chat (11:21, 1:04:17–1:07:20).

## Instalación y configuración inicial

1. **Instalación local:** el presentador indica abrir la página de Hermes Agent y ejecutar la línea de instalación en el equipo (37:41). La transcripción no incluye el comando ni muestra sus requisitos con suficiente detalle: no es posible reproducir este paso solo con el texto guardado.
2. **Alternativa VPS:** en Hostinger, el curso muestra la instalación de Hermes Agent mediante Docker. También menciona una instalación a nivel de raíz; la demostración continúa con Docker (41:48–48:00). Los clics y valores completos no están todos transcritos.
3. En el asistente inicial, el presentador elige **Quick setup** y después un proveedor de inferencia (50:24–50:46).
4. Para la demostración selecciona **OpenAI Codex**, abre el enlace de autenticación y utiliza el código mostrado. Durante el proceso activa la autorización mediante código de dispositivo y después selecciona un modelo (56:36–59:53). La fuente no permite asegurar que esta integración siga disponible o esté autorizada en todos los planes.
5. Elige el entorno de ejecución. El curso selecciona **local** dentro del entorno en el que está instalando Hermes (59:53–1:00:20).

## Conexión opcional con Telegram

El curso muestra cómo crear un bot con BotFather, obtener su token y autorizar al usuario de Telegram para comunicarse con él (1:03:44–1:07:20). La transcripción señala el token y el identificador numérico del usuario como datos necesarios, pero no documenta un procedimiento seguro completo para introducirlos. No guardes secretos en esta wiki.

## Comprobación y límites

El presentador menciona las funciones **Hermes Doctor** y **Gateway** al revisar la instalación (1:03:27–1:03:43). La transcripción no proporciona suficiente evidencia para garantizar que Doctor repare automáticamente cualquier problema ni una lista completa de comprobaciones.

La fuente es una transcripción de un video y omite información visual, comandos y detalles de configuración. Para completar los pasos faltantes se necesita una fuente adicional proporcionada por el usuario; no se han consultado páginas externas.
