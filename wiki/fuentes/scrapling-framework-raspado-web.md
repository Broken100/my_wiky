# Scrapling: framework de raspado web (D4Vinci)

- **Origen:** [D4Vinci/Scrapling: The BEST Web Scraping framework…](<../../fuentes/D4VinciScrapling 🕷️ The BEST Web Scraping framework that handles everything from a single request to a full-scale crawl! Don't be shy, join here httpsdiscord.ggEMgGbDceNQ and follow here for daily tips and tricks httpsx.comScrapling_dev.md>). Captura del README del repositorio, conservada como original.
- **URL de origen:** <https://github.com/D4Vinci/Scrapling>
- **Autoría:** Karim Shoair (D4Vinci), según el bloque de citación del propio documento. El frontmatter de la captura no registra autor ni fecha de publicación.
- **Capturado:** 2026-11-18. Documento técnico / README de proyecto.
- **Licencia declarada:** BSD-3-Clause.

## Idea central

Scrapling es una biblioteca de Python para raspado web que unifica en una sola API cuatro cosas que normalmente se combinan a mano: peticiones HTTP rápidas, navegadores sigilosos con renderizado de JavaScript, un motor de análisis y extracción, y un framework de rastreo (arañas) con concurrencia, rotación de proxies y pausa/reanudación. La fuente la presenta como solución única «desde una sola petición hasta un rastreo a escala de sitio completo», utilizable desde Python, desde la terminal o desde un agente de IA a través de un servidor MCP.

## Qué cubre

| Bloque | Capacidades que declara la fuente |
| --- | --- |
| Obtención (fetchers) | `Fetcher` para HTTP con huella TLS de navegador, cabeceras y HTTP/3; `StealthyFetcher`/`StealthySession` con automatización de navegador (Chromium de Playwright o Chrome), suplantación de huella y resolución de Cloudflare Turnstile/Interstitial; sesiones persistentes (`FetcherSession`, `StealthySession`) para cookies y estado; `ProxyRotator` con rotación cíclica o personalizada; bloqueo de dominios y de ~3.500 dominios publicitarios o de rastreo; DNS-over-HTTPS opcional para evitar fugas de DNS; navegadores remotos por CDP con `cdp_url` o binario propio con `executable_path`; captura de APIs en segundo plano con `record_requests=True` y búsqueda en `session.network.search()`; soporte async completo. |
| Rastreo (spiders) | API similar a Scrapy (clases `Spider`, `Request`, `Response`, callbacks asíncronos), concurrencia y límites configurables por dominio, multisesión por ID (`sid`) para enrutar peticiones a sesiones HTTP o sigilosas dentro de la misma araña, pausa/reanudación con `crawldir` (Ctrl+C guarda peticiones pendientes y URL vistas), modo streaming (`spider.stream()`) con estadísticas en vivo, detección y reintento de peticiones bloqueadas, AutoThrottle que ajusta el retardo por dominio según la respuesta del sitio y respeta `Retry-After`, `robots_txt_obey` opcional (Disallow, Crawl-delay, Request-rate con caché por dominio), modo desarrollo que cachea respuestas en disco para iterar `parse()` sin volver a golpear el servidor, y plantillas listas: `CrawlSpider`, `SitemapSpider`, `XMLFeedSpider`, `CSVFeedSpider`, `ShopifySpider`. |
| Extracción | Selectores CSS y XPath con pseudo-elementos tipo Scrapy, `find_all` estilo BeautifulSoup, `find_by_text`, expresiones regulares, seguimiento adaptativo de elementos (reubicar un elemento cuando el sitio cambia), `find_similar()` y navegación DOM (parent, siblings, below_elements), generación automática de selectores y limpieza de la salida a Markdown. |
| Rendimiento declarado | Serialización JSON descrita como 10x más rápida que la biblioteca estándar, estructuras de datos optimizadas para memoria, 92% de cobertura de tests y cobertura completa de type hints, verificada con PyRight y MyPy. |
| Uso desde IA | Servidor MCP (peticiones HTTP, acciones de navegador, capturas, estado de login guardado, respuestas de API en segundo plano; transporte remoto por CDP y HTTP con autenticación), un Agent Skill instalable para que los agentes escriban código con la API vigente, y salida lista para RAG: `page.markdown()` por página o `ContentSpider` para convertir un sitio entero en un corpus Markdown sin LLM en el circuito. |
| Uso desde terminal | `scrapling shell` (IPython con atajos, conversión de curl a peticiones Scrapling) y `scrapling extract get|fetch` para volcar una página a `.txt`, `.md` o `.html` con `--css-selector`, `--wait-selector`, `--impersonate`, `--solve-cloudflare`. |
| Integración | Decorador `scrapling_response` para reutilizar los callbacks de Scrapy existentes con el parser de Scrapling; imagen Docker oficial (`pyd4vinci/scrapling`, `ghcr.io/d4vinci/scrapling`). |

## Instalación

Requiere Python 3.10 o superior. `pip install scrapling` instala solo el motor de análisis; los fetchers y las arañas fallan con `ModuleNotFoundError` si no se agregan los extras. Según la fuente: `pip install "scrapling[fetchers]"` más `scrapling install` (descarga navegadores y dependencias), `scrapling[ai]` para el servidor MCP, `scrapling[rag]` para sistemas RAG, `scrapling[shell]` para la shell y el comando `extract`, y `scrapling[all]` para todo.

## Benchmark que reporta la fuente

Extracción de texto sobre 5.000 elementos anidados, en milisegundos (promedio de más de 100 ejecuciones, según `benchmarks.py` del repositorio):

| Biblioteca | Tiempo | Relativo a Scrapling |
| --- | --- | --- |
| Scrapling | 1,99 | 1,0x |
| Parsel/Scrapy | 2,06 | 1,035x |
| Raw Lxml | 2,56 | 1,286x |
| PyQuery | 23,98 | ~12x |
| Selectolax | 197,02 | ~99x |
| MechanicalSoup | 1.545,15 | ~776,5x |
| BS4 con Lxml | 1.562,1 | ~785,0x |
| BS4 con html5lib | 3.412,73 | ~1.714,9x |

En búsqueda de texto y similitud de elementos, la fuente compara 2,3 ms de Scrapling contra 12,58 ms de AutoScraper (5,47x).

## Limitaciones de esta fuente

El documento es el README del propio proyecto: es material de presentación del autor, no una evaluación independiente. Los benchmarks, la cobertura de tests y las afirmaciones de rendimiento las mide y publica el mismo equipo, sin que se indique un tercero que las replique. El README incluye bloques de patrocinadores y enlaces de afiliado con códigos promocionales que no aportan información técnica y no se trasladaron aquí.

El 2026-11-19 se consultó el [repositorio](https://github.com/D4Vinci/Scrapling): su README presenta HTTP, navegadores, análisis y rastreo como capacidades del framework Python. Esa lectura no prueba el funcionamiento ni el rendimiento; no se inspeccionaron código, tests ni documentación enlazada. Los demás detalles siguen atribuidos a la fuente. Tampoco se verificó el estado real de la elusión de protecciones anti-bot: la afirmación del autor es comercial. El documento declara uso educativo y de investigación y responsabiliza al usuario del cumplimiento legal y de los términos de servicio de los sitios. Véase la [auditoría](../auditorias/verificacion-fuentes-2026-11-19.md).

## Relacionado

- [Raspado web y extracción de datos](../temas/raspado-web-y-extraccion-de-datos.md)
- [Flujos de trabajo agénticos](flujos-agentes-produccion-2026.md)
