# Registro

Historial cronológico acumulativo. Las entradas nuevas se añaden al final, sin reescribir las anteriores.

## [2026-11-18] creación | Inicio de la wiki

Se creó la estructura inicial, sin fuentes procesadas.

## [2026-11-18] organización | Ampliación de la biblioteca

Se organizó la navegación central, los espacios para fuentes y temas y el destino de adjuntos de Obsidian. No se procesaron fuentes durante esta organización.


## [2026-11-18] organización | Ubicación de las fuentes

Se estableció `fuentes/` para originales y `fuentes/adjuntos/` para adjuntos; la wiki conserva los resúmenes y temas procesados.

## [2026-11-18] ingest | Cafeterías de Otavalo

Procesada [Biblioteca.md](fuentes/Biblioteca.md): creados el [resumen de fuente](wiki/fuentes/biblioteca-cafeterias-otavalo.md) y el [tema de cafeterías](wiki/temas/cafeterias-de-otavalo.md). Las cifras y reseñas son afirmaciones de una respuesta de Gemini y no se verificaron en fichas originales; las imágenes externas no se inspeccionaron.

## [2026-11-18] query | Cafeterías con menor puntuación

Consulta basada en [cafeterías de Otavalo](wiki/temas/cafeterias-de-otavalo.md); la fuente no permite establecer un ranking de 40 negocios ni verificar las valoraciones.

## [2026-11-18] ingest | Plugins para trabajo de conocimiento

Procesada [la publicación de Yarad](<fuentes/Yarad (@Yarad_ai) en X.md>): creados el [resumen](wiki/fuentes/plugins-trabajo-conocimiento-yarad.md) y el [tema](wiki/temas/plugins-para-trabajo-de-conocimiento.md). El repositorio enlazado no se inspeccionó; las afirmaciones quedan atribuidas a la fuente.

## [2026-11-18] ingest | Flujos de trabajo agénticos en producción

Procesada [la guía de flujos agénticos](<fuentes/Cómo crear flujos de trabajo agénticos en 2026 - guía de arquitectura, ejecución y producción.md>): creados el [resumen](wiki/fuentes/flujos-agentes-produccion-2026.md) y el [tema](wiki/temas/flujos-de-trabajo-agenticos.md), y añadido contexto cruzado en el [tema de plugins](wiki/temas/plugins-para-trabajo-de-conocimiento.md). El original llegó como texto pegado en un bloque único, sin autor ni fecha, y con las tablas aplanadas por el copiado; Luis aportó después el enlace de la conversación compartida de Gemini, que se verificó como origen (el texto coincide y cierra en la misma sección) y sirvió para comprobar los encabezados y la primera columna de las siete tablas reconstruidas en el resumen. Las cifras, licencias, precios y benchmarks son afirmaciones de la fuente y no se verificaron de forma independiente.

## [2026-11-18] query | Inventario general de la bóveda

Consulta de solo lectura reportada por `consulta` sobre el contenido actual de la bóveda, revisando el [índice](indice.md), `fuentes/` (3 originales procesadas y `fuentes/adjuntos/` vacía), `wiki/fuentes/` (3 resúmenes) y `wiki/temas/` (3 temas). No se crearon ni modificaron páginas.

## [2026-11-18] lint | Eliminación de Cafeterías de Otavalo

Se eliminaron los 3 archivos de Cafeterías de Otavalo (`fuentes/Biblioteca.md`, `wiki/fuentes/biblioteca-cafeterias-otavalo.md` y `wiki/temas/cafeterias-de-otavalo.md`) a pedido y confirmación explícita de Luis, y se retiraron sus dos entradas en [indice.md](indice.md).

## [2026-11-18] query | Cómo configurar Hermes

Se creó la [guía de configuración](wiki/proyectos-2026/como-configurar-hermes.md) a partir del [curso guardado](<fuentes/HERMES 2026 Curso COMPLETO en Español (Agente IA).md>). La transcripción no incluye el comando de instalación ni todos los detalles visuales; no se consultaron fuentes externas.

## [2026-11-19] ingest | Scrapling: framework de raspado web

Procesado [el README de Scrapling](<fuentes/D4VinciScrapling 🕷️ The BEST Web Scraping framework that handles everything from a single request to a full-scale crawl! Don't be shy, join here httpsdiscord.ggEMgGbDceNQ and follow here for daily tips and tricks httpsx.comScrapling_dev.md>): creados el [resumen de fuente](wiki/fuentes/scrapling-framework-raspado-web.md) y el [tema de raspado web](wiki/temas/raspado-web-y-extraccion-de-datos.md), con un enlace cruzado añadido al [tema de flujos agénticos](wiki/temas/flujos-de-trabajo-agenticos.md). Los benchmarks y la cobertura de tests los publica el propio proyecto; no se inspeccionaron el código, los tests ni la documentación enlazada.

## [2026-11-19] ingest | HERMES 2026: curso completo en español

Procesada [la transcripción del curso](<fuentes/HERMES 2026 Curso COMPLETO en Español (Agente IA).md>): creados el [resumen de fuente](wiki/fuentes/hermes-2026-curso-completo.md) y el [tema de automatización](wiki/temas/automatizacion-con-hermes.md). La transcripción deforma nombres técnicos de forma sistemática (MCP aparece como «MSP», Claude Code como «Cloud Code», OpenClaw como «Open Cloud»), anuncia ocho proyectos pero desarrolla siete más un bonus, y da valores distintos para la ventana de contexto; quedan anotados como limitaciones, junto con el contenido promocional omitido. No se consultaron fuentes externas.

## [2026-11-19] lint | Revisión de enlaces y nombre de archivo largo

Comprobados los 62 enlaces relativos de la bóveda: los cuatro que no resuelven están en este [registro](registro.md) y apuntan a las páginas de Cafeterías de Otavalo eliminadas el 2026-11-18, de modo que se conservan como historial. Detectado además que el original de Scrapling tiene un nombre de 241 caracteres y una ruta absoluta de 281, por encima del límite de 260 de Windows: Obsidian lo abre, pero algunas herramientas no. No se renombró el original; queda pendiente de decisión de Luis.

## [2026-11-19] lint | Auditoría de salud de la biblioteca (solo lectura)

Alcance: 21 documentos .md (5 en fuentes/, 4 en wiki/fuentes/, 4 en wiki/temas/, 1 en wiki/proyectos-2026/, 2 stubs en wiki/, 4 base en raíz) y 74 enlaces internos comprobados con verificación de rutas largas. fuentes/adjuntos/ no existe todavía. No se modificó ningún archivo de la bóveda.

Verificado:
- 70 de 74 enlaces resuelven. Los 4 rotos están en registro.md (líneas 20 y 24) y apuntan a las páginas de Cafeterías de Otavalo eliminadas el 2026-11-18; se conservan como historial.
- Las 4 fuentes originales de fuentes/ tienen al menos una página de wiki que las enlaza; ninguna fuente sin procesar.
- indice.md lista las 4 fuentes, 4 temas y la página de proyecto; no faltan entradas.
- Ninguna ruta usa codificación %20.
- Scrapling: la ruta completa mide 281 caracteres y el nombre 239 caracteres (código Unicode). El asiento del 2026-11-19 anterior dice 241; la diferencia parece ser el método de conteo (no verificado). El problema de ruta larga sigue pendiente de decisión de Luis.
- El asiento lint anterior (2026-11-19) indica 62 enlaces comprobados; la comprobación corregida da 74. Las 12 diferencias eran enlaces con espacios entre <>, que la primera versión del verificador no leía.

Sugerencias (no verificadas como problemas):
- wiki/index.md y wiki/log.md son stubs sin entrada en indice.md; podrían retirarse o no.
- Inicio.md menciona "Pide a Codex"; revisar si sigue siendo la herramienta prevista.
- Crear fuentes/adjuntos/ si se quiere que exista antes del primer adjunto de Obsidian.

No se pudo comprobar: el contenido factual de las fuentes y los resúmenes frente a sus originales (no era el alcance pedido de enlaces, y no se usó la web).

## [2026-11-19] lint | Verificación parcial de fuentes

Se registraron los resultados y límites de acceso en la [auditoría](wiki/auditorias/verificacion-fuentes-2026-11-19.md) y se creó el [procedimiento manual de verificación](wiki/procedimientos/verificacion-de-fuentes.md). Se actualizaron los resúmenes de [Scrapling](wiki/fuentes/scrapling-framework-raspado-web.md), [plugins](wiki/fuentes/plugins-trabajo-conocimiento-yarad.md), [flujos agénticos](wiki/fuentes/flujos-agentes-produccion-2026.md) y [curso de Hermes](wiki/fuentes/hermes-2026-curso-completo.md), y los temas de [plugins](wiki/temas/plugins-para-trabajo-de-conocimiento.md) y [flujos agénticos](wiki/temas/flujos-de-trabajo-agenticos.md). Se corrigió la interpretación del 22% sobre controles MCP; la predicción de Gartner solo se contrastó con un fragmento de búsqueda. No se verificaron íntegramente las fuentes ni se alteraron originales.

## [2026-11-19] ingest | Avance del proyecto de placas NFC y QR

Se guardó la [síntesis de la sesión](<fuentes/adjuntos/proyecto-placas-nfc-qr-avance-2026-11-19.md>) y se crearon el [resumen](wiki/fuentes/proyecto-placas-nfc-qr-avance-2026-11-19.md) y el [tema de proyecto](wiki/temas/proyecto-placas-nfc-qr-resenas.md). La síntesis distingue el prototipo local verificado parcialmente de las funciones pendientes y de las hipótesis comerciales; no es una auditoría de seguridad.

## [2026-11-19] lint | Auditoría comparativa de entornos (OpenBot vs. Hermes)

Publicada la [auditoría comparativa de entornos](wiki/auditorias/comparativa-entornos-openbot-vs-hermes-2026-11-19.md) y actualizada su entrada en [indice.md](indice.md). Se revisaron los 13 cambios locales previos del entorno Hermes (verificación puntual de fuentes y avance del proyecto NFC/QR): 156 enlaces internos comprobados en 27 archivos `.md` con 0 enlaces rotos nuevos (se mantienen los 4 enlaces históricos de Cafeterías de Otavalo en este registro) y originales previos en `fuentes/` sin cambios.
