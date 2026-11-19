---
title: "D4Vinci/Scrapling: 🕷️ The BEST Web Scraping framework that handles everything from a single request to a full-scale crawl! Don't be shy, join here: https://discord.gg/EMgGbDceNQ and follow here for daily tips and tricks: https://x.com/Scrapling_dev"
source: "https://github.com/D4Vinci/Scrapling"
author:
published:
created: 2026-11-18
description:
tags:
  - "clippings"
---
## Raspado web sin esfuerzo para la Web moderna

[![D4Vinci%2FScrapling | Cambio de tendencia](https://camo.githubusercontent.com/e6b7cfe8b479ff37e85f6707d4319b4865a838a7f0af76370ae2c465b97cc509/68747470733a2f2f7472656e6473686966742e696f2f6170692f62616467652f7265706f7369746f726965732f3134323434)](https://trendshift.io/repositories/14244) [![Rango de historia estelar](https://camo.githubusercontent.com/056cbad452b16d6213f8aad31b86160b8e88fdcb9ccc8064340aa1841ba40135/68747470733a2f2f6170692e737461722d686973746f72792e636f6d2f62616467653f7265706f3d443456696e63692f53637261706c696e6726747970653d72616e6b)](https://www.star-history.com/d4vinci/scrapling)  

[**Métodos de selección**](https://scrapling.readthedocs.io/en/latest/parsing/selection.html) · [**Buscadores**](https://scrapling.readthedocs.io/en/latest/fetching/choosing.html) · [**Arañas**](https://scrapling.readthedocs.io/en/latest/spiders/architecture.html) · [**Rotación de proxy**](https://scrapling.readthedocs.io/en/latest/spiders/proxy-blocking.html) · [**CLI**](https://scrapling.readthedocs.io/en/latest/cli/overview.html) · [**MCP**](https://scrapling.readthedocs.io/en/latest/ai/mcp-server.html)

Scrapling es un marco de raspado web de Python que reúne solicitudes HTTP rápidas, navegadores ocultos, análisis potentes y rastreo simultáneo en una sola biblioteca.

Extraiga páginas estáticas, represente JavaScript, interactúe con sitios web y extraiga datos de las llamadas API detrás de ellos. Cree rastreadores con sesiones reutilizables, rotación de proxy, reintentos automáticos, pausa/reanudación y exportaciones de datos integradas.

Úselo desde Python, la terminal o su agente de IA a través de su servidor MCP y Agent Skill. Desde extraer algunos campos hasta recopilar un sitio web completo como Markdown limpio, Scrapling cubre el flujo de trabajo de scraping completo.

## Qué puedes hacer con el Scrapling

- **[Obtenga e interactúe: solicitudes HTTP rápidas](https://scrapling.readthedocs.io/en/latest/fetching/choosing.html)**, navegadores ocultos, renderizado de JavaScript, manejo de torniquetes/intersticiales de Cloudflare, sesiones persistentes y captura de API en segundo plano.
- **[Analizar y extraer](https://scrapling.readthedocs.io/en/latest/parsing/selection.html)**: CSS, XPath, búsqueda de texto y expresiones regulares, seguimiento adaptativo de elementos, coincidencia de elementos similares y limpieza de la salida de Markdown.
- **[Rastreo y exportación:](https://scrapling.readthedocs.io/en/latest/spiders/getting-started.html)** arañas simultáneas, rotación de proxy, reintentos, AutoThrottle, pausa/reanudar, transmisión, plantillas listas para usar y exportaciones JSON/JSONL/CSV/XML.
- **[Raspe con IA o desde su terminal](https://scrapling.readthedocs.io/en/latest/ai/mcp-server.html)**: un servidor MCP para que los agentes de IA exploren y extraigan datos, una habilidad de agente, comandos CLI y un shell interactivo.

Comience con una sola página:

```
from scrapling.fetchers import Fetcher

page = Fetcher.get('https://quotes.toscrape.com/')
quotes = page.css('.quote .text::text').getall()
print(quotes)
```

O ampliar con una araña:

```
from scrapling.spiders import Spider, Response

class QuotesSpider(Spider):
    name = "quotes"
    start_urls = ["https://quotes.toscrape.com/"]

    async def parse(self, response: Response):
        for quote in response.css('.quote'):
            yield {
                "text": quote.css('.text::text').get(),
                "author": quote.css('.author::text').get(),
            }

result = QuotesSpider().start()
print(len(result.items))
result.items.to_json("quotes.json")
```

Esto imprime`10` y guarda las citas y los autores en`quotes.json`.

[![En DataImpulse, nos especializamos en desarrollar servicios proxy personalizados para su negocio. Realice solicitudes desde cualquier lugar, recopile datos y disfrute de conexiones rápidas con nuestros servidores proxy premium.](https://raw.githubusercontent.com/D4Vinci/Scrapling/main/images/DataImpulse.png)](https://dataimpulse.com/?utm_source=scrapling&utm_medium=banner&utm_campaign=scrapling)

## Patrocinadores Platino

| [![](https://raw.githubusercontent.com/D4Vinci/Scrapling/main/images/NodeMaven.jpg)](https://go.nodemaven.com/scraplingoctober "Proxies con las puntuaciones de IP más altas") | [NodeMaven](https://go.nodemaven.com/scraplingoctober): el proveedor de proxy más eficiente para web scraping y automatización con la IP de mayor calidad del mercado. SCRAPLING2X - Tráfico 2X para el Black Friday |
| --- | --- |
| [![](https://raw.githubusercontent.com/D4Vinci/Scrapling/main/images/HyperSolutions.png)](https://hypersolutions.co/?utm_source=github&utm_medium=readme&utm_campaign=scrapling "API de omisión de protección de bots para Akamai, DataDome, Incapsula y Kasada") | El raspador maneja el torniquete Cloudflare. Para protección de nivel empresarial, [**Hyper Solutions**](https://hypersolutions.co/?utm_source=github&utm_medium=readme&utm_campaign=scrapling) proporciona puntos finales de API que generan tokens antibot válidos para **Akamai**, **DataDome**, **Kasada** e **Incapsula**. Llamadas API simples, no se requiere automatización del navegador. |
| [![](https://raw.githubusercontent.com/D4Vinci/Scrapling/main/images/evomi.png)](https://evomi.com/?utm_source=github&utm_medium=banner&utm_campaign=d4vinci-scrapling "Evomi es su proveedor proxy de calidad suizo, a partir de $0,49/GB") | [**Evomi**](https://evomi.com/?utm_source=github&utm_medium=banner&utm_campaign=d4vinci-scrapling) : servidores proxy residenciales desde $0,49/GB. Explorador raspador con Chromium completamente falsificado, IP residenciales, resolución automática de CAPTCHA y omisión de anti-bot. API de Scraper para obtener resultados sin complicaciones.   **Las integraciones MCP y N8N están disponibles.** |
| [![](https://raw.githubusercontent.com/D4Vinci/Scrapling/main/images/TikHub.jpg)](https://tikhub.io/?utm_source=github.com/D4Vinci/Scrapling&utm_medium=marketing_social&utm_campaign=retargeting&utm_content=carousel_ad "Libere el poder de los datos y la inteligencia artificial de las redes sociales") | [TikHub.io proporciona](https://tikhub.io/?utm_source=github.com/D4Vinci/Scrapling&utm_medium=marketing_social&utm_campaign=retargeting&utm_content=carousel_ad) 900+ API estables en 16+ plataformas, incluidas TikTok, X, YouTube e Instagram, con 40 millones de conjuntos de datos+. También ofrece modelos [de IA CON DESCUENTO](https://ai.tikhub.io/?ref=KarimShoair): Claude, GPT, GEMINI y más con hasta un 71% de descuento. |
| [![](https://raw.githubusercontent.com/D4Vinci/Scrapling/main/images/TWSC.png)](https://substack.thewebscraping.club/p/scrapling-hands-on-guide?utm_source=github&utm_medium=repo&utm_campaign=scrapling "El boletín número uno dedicado al Web Scraping") | Lea una reseña completa de [Scrapling en The Web Scraping Club](https://substack.thewebscraping.club/p/scrapling-hands-on-guide?utm_source=github&utm_medium=repo&utm_campaign=scrapling) (noviembre de 2025), el boletín número uno dedicado a Web Scraping. |
| [![](https://raw.githubusercontent.com/D4Vinci/Scrapling/main/images/byteful.png)](https://byteful.com/?utm_source=github&utm_medium=github-sponsor&utm_campaign=scrapling_github_sponsor "Infraestructura de proxy residencial de origen ético que impulsa el web scraping y los agentes de inteligencia artificial") | [Byteful proporciona](https://byteful.com/?utm_source=github&utm_medium=github-sponsor&utm_campaign=scrapling_github_sponsor) servidores proxy éticos para residencias, dispositivos móviles, ISP y centros de datos, además de herramientas de automatización de inteligencia artificial y scraping. Utilice el código SCRAPLING10: 10% de descuento en proxies residenciales. |
| [![](https://raw.githubusercontent.com/D4Vinci/Scrapling/main/images/proxylane.png)](https://proxylane.dev/scrapling?utm_source=scrapling&utm_medium=affiliate&utm_campaign=scrapling_platinum_202609&utm_content=github_readme "90% menos de CAPTCHA en tus ejecuciones de Scrapling.") | [ProxyLane](https://proxylane.dev/scrapling?utm_source=scrapling&utm_medium=affiliate&utm_campaign=scrapling_platinum_202609&utm_content=github_readme): 90% menos CAPTCHA en sus ejecuciones de Scrapling. 28 millones de dispositivos residenciales P2P reales, 98,7% IP limpias, 195 países. Orientación ciudad/ISP/ZIP, sesiones fijas/rotativas, el tráfico nunca expira. Pruebe 350 MB por $1,95. Código **SCRAPEANDTAKE**: *25% de descuento*. |
| [![](https://raw.githubusercontent.com/D4Vinci/Scrapling/main/images/lundproxy.png)](https://www.lundproxy.com/unlimited-residential-bandwidth.html?utm_source=github&utm_medium=sponsorship&utm_campaign=scrapling&utm_content=banner "Tráfico residencial ilimitado, dimensionado únicamente por ancho de banda.") | [LundProxy](https://www.lundproxy.com/?utm_source=github&utm_medium=sponsorship&utm_campaign=scrapling&utm_content=text): deja de pagar por GB. Tráfico residencial ilimitado, 100M+ IPs, 190+ países. Paga por ancho de banda. Solicite una prueba gratuita de servidores proxy residenciales de 50 Mbps. |
| [![](https://raw.githubusercontent.com/D4Vinci/Scrapling/main/images/resolvr.jpg)](https://resolvr.com/?ref=scrapling "Desafíos de Captcha y navegador, resueltos en dispositivos reales que cargan la página.") | Resolución: Raspar a escala significa CAPTCHA a escala. Resolvr resuelve los desafíos de reCAPTCHA y Cloudflare en dispositivos reales a través de una API. El acceso beta gratuito ya está disponible. |
| [![](https://raw.githubusercontent.com/D4Vinci/Scrapling/main/images/Nonecap.png)](https://nonecap.com/solutions/scrapling/?ref=scrapling "solucionador hCaptcha como API.") | ¿hCaptcha en tus ejecuciones de Scrapling? NoneCap devuelve tokens hCaptcha válidos, incluidas las claves de sitio empresariales, a través de una API. 1.300 créditos gratis al registrarse. |
| [![](https://raw.githubusercontent.com/D4Vinci/Scrapling/main/images/DataImpulse_Platinum.png)](https://dataimpulse.com/?utm_source=website&utm_medium=github_readme&utm_campaign=scrapling_platinum "Compre servidores proxy de pago por uso desde $1/GB") | $1/GB en [DataImpulse](https://dataimpulse.com/?utm_source=website&utm_medium=github_readme&utm_campaign=scrapling_platinum). Agentes éticos de primera parte. 90M+ IP, 195+ países. Residencial, centro de datos, móvil. Pago por uso = sin suscripción. El tráfico nunca expira. |

*<sub><font><font>¿Quieres mostrar tu anuncio aquí? Haga clic</font></font> <a href="https://github.com/sponsors/D4Vinci/sponsorships?tier_id=586646"><font><font>aquí</font></font></a></sub>*

## Patrocina

[![](https://raw.githubusercontent.com/D4Vinci/Scrapling/main/images/cloro.jpg)](https://cloro.dev/?utm_source=referral&utm_medium=scrapling "La API de búsqueda para la era de la IA")

[![](https://raw.githubusercontent.com/D4Vinci/Scrapling/main/images/SerpApi.png)](https://serpapi.com/?utm_source=scrapling "Elimine Google y otros motores de búsqueda con SerpApi") [![](https://raw.githubusercontent.com/D4Vinci/Scrapling/main/images/decodo.png)](https://visit.decodo.com/Dy6W0b "Pruebe los proxies residenciales más eficientes de forma gratuita") [![](https://raw.githubusercontent.com/D4Vinci/Scrapling/main/images/ProxyEmpire.png)](https://proxyempire.io/?ref=scrapling&utm_source=github&utm_medium=paid_referral&utm_campaign=open_source_sponsorship&utm_content=scrapling "Recopile los datos que su proyecto necesita con los mejores proxies residenciales") [![](https://raw.githubusercontent.com/D4Vinci/Scrapling/main/images/webshare.png)](https://www.webshare.io/?referral_code=48r2m2cd5uz1 "El proxy más confiable con un rendimiento incomparable") [![](https://raw.githubusercontent.com/D4Vinci/Scrapling/main/images/proxiware.png)](https://proxiware.com/?ref=scrapling "Recopilar cualquier dato. A cualquier escala.") [![](https://raw.githubusercontent.com/D4Vinci/Scrapling/main/images/SingaporeMobileProxy.png)](https://singaporemobileproxy.com/?utm_source=scrapling&utm_medium=sponsor&utm_campaign=paid_test_q4 "Proxies móviles 4G/5G dedicados en tarjetas SIM reales de Singapur") [![](https://raw.githubusercontent.com/D4Vinci/Scrapling/main/images/olostep.png)](http://olostep.com/ "Infraestructura de datos web para IA")

*<sub><font><font>¿Quieres mostrar tu anuncio aquí? ¡Haz clic aquí y</font> <font>elige el nivel que más te convenga!</font></font></sub>*

---

## Características clave

### Arañas: un marco de rastreo completo

- 🕷� **� API de Spider similar a Scrapy**: define arañas con devoluciones de llamadas asincrónicas y objetos /.`start_urls``parse``Request``Response`
- ⚡ **Rastreo simultáneo**: límites de concurrencia configurables, limitación por dominio y retrasos en las descargas.
- 🔄 **Soporte multisesión**: interfaz unificada para solicitudes HTTP y navegadores sigilosos sin interfaz gráfica en una sola araña: enruta las solicitudes a diferentes sesiones por ID.
- 💾 **Pausar y reanudar**: guarde las solicitudes pendientes y las URL vistas con`crawldir`. Presione Ctrl+C para detenerse correctamente y reiniciar con el mismo directorio.
- 📡 **Modo de transmisión**: transmite elementos extraídos a medida que llegan`async for item in spider.stream()` con estadísticas en tiempo real, ideal para interfaz de usuario, canalizaciones y rastreos de larga duración.
- 🛡� **� Detección de solicitudes bloqueadas**: detección automática y reintento de solicitudes bloqueadas con lógica personalizable.
- 🚦 **AutoThrottle**: Stop guessing delays. The spider tunes the delay of each domain on its own from how fast the website responds, then doubles it (or waits what `Retry-After` asks) whenever the website starts blocking or rate-limiting you, and speeds back up once it stops.
- 🤖 **Robots.txt Compliance**: Optional `robots_txt_obey` flag that respects `Disallow`, `Crawl-delay`, and `Request-rate` directives with per-domain caching.
- 🧪 **Development Mode**: Cache responses to disk on the first run and replay them on subsequent runs - iterate on your `parse()` logic without re-hitting the target servers.
- 🧩 **Ready-made Spider Templates**: Skip the boilerplate with `CrawlSpider` for rule-based link following, `SitemapSpider` for sitemap/robots.txt-driven crawls, `XMLFeedSpider`/`CSVFeedSpider` for iterating XML/RSS and CSV feeds, and `ShopifySpider` to collect products from public Shopify stores, one item per variant.
- 🔗 **Link Extraction**: A standalone `LinkExtractor` primitive with allow/deny patterns, domain filters, CSS/XPath scoping, extension filtering, and canonicalization - use it inside the templates or on its own.
- 📦 **Built-in Export**: Export results through hooks and your own pipeline or the built-in JSON/JSONL/CSV/XML exporters with `result.items.to_json()`, `to_jsonl()`, `to_csv()`, and `to_xml()`.

### Advanced Websites Fetching with Session Support

- **HTTP Requests**: Fast and stealthy HTTP requests with the `Fetcher` class. Can impersonate browsers' TLS fingerprint, headers, and use HTTP/3.
- **Dynamic Loading**: Fetch dynamic websites with full browser automation through the `StealthyFetcher` class supporting Playwright's Chromium and Google's Chrome.
- **Anti-bot Bypass**: Advanced stealth capabilities with `StealthyFetcher` and fingerprint spoofing. Can easily bypass all types of Cloudflare's Turnstile/Interstitial with automation.
- **Session Management**: Persistent session support with `FetcherSession` and `StealthySession` classes for cookie and state management across requests.
- **Proxy Rotation**: Built-in `ProxyRotator` with cyclic or custom rotation strategies across all session types, plus per-request proxy overrides.
- **Domain & Ad Blocking**: Block requests to specific domains (and their subdomains) or enable built-in ad blocking (~3,500 known ad/tracker domains) in browser-based fetchers.
- **DNS Leak Prevention**: Optional DNS-over-HTTPS support to route DNS queries through Cloudflare's DoH, preventing DNS leaks when using proxies.
- **Remote Browsers**: Instead of launching a browser locally, connect to one that's already running through CDP with `cdp_url`, whether it's on the same machine, another host, or a managed browser provider. You can also point any browser fetcher at your own Chromium build with `executable_path`.
- **Background API Capture**: Enable `record_requests=True` on a browser session, then search its saved responses with `session.network.search(limit=None)`. Read API data and HTTP errors across page loads without sending requests again. History is bounded and saves text response bodies.
- **Async Support**: Complete async support across all fetchers and dedicated async session classes.

### Parsing and Data Extraction

- 🔄 **Smart Element Tracking**: Relocate elements after website changes using intelligent similarity algorithms.
- 🎯 **Smart Flexible Selection**: CSS selectors, XPath selectors, filter-based search, text search, regex search, and more.
- 🔍 **Find Similar Elements**: Automatically locate elements similar to found elements.

### AI Features

- 🤖 **MCP Server**: Let AI chatbots and agents (Claude/Cursor/etc) scrape through Scrapling with HTTP requests, browser actions, screenshots, saved login state, and background API responses. Uses our stealthy browser with the right tools to enable suitable AI models to bypass any CAPTCHA is faces while working (not to mention our Cloudflare solver). It uses a lot of methods to reduce the content sent to the model, block ads, and protect it against Prompt-Injection. Remote browsers over CDP and HTTP transport with authentication options are included. ([demo video](https://www.youtube.com/watch?v=qyFk3ZNwOxE))
- 🧠 **Agent Skill**: A ready-to-install [Agent Skill](https://scrapling.readthedocs.io/en/latest/ai/agent-skill.html) that teaches coding agents the whole library, so the code they write with Scrapling matches the current API instead of guessing.
- 📚 **RAG-ready Markdown**: Turn any page into clean, sanitized, LLM-ready Markdown with one line (`page.markdown()`), or crawl a whole website into a Markdown corpus with the `ContentSpider` template, all without an LLM in the loop. ([docs](https://scrapling.readthedocs.io/en/latest/ai/building-rag-systems.html))

### High-Performance & battle-tested Architecture

- 🚀 **Lightning Fast**: Optimized performance outperforming most Python scraping libraries.
- 🔋 **Memory Efficient**: Optimized data structures and lazy loading for a minimal memory footprint.
- ⚡ **Fast JSON Serialization**: 10x faster than the standard library.
- 🏗️ **Battle tested**: Not only does Scrapling have 92% test coverage and full type hints coverage, but it has been used daily by hundreds of Web Scrapers over the past year.

### Developer/Web Scraper Friendly Experience

- 🎯 **Interactive Web Scraping Shell**: Optional built-in IPython shell with Scrapling integration, shortcuts, and new tools to speed up Web Scraping scripts development, like converting curl requests to Scrapling requests and viewing requests results in your browser.
- 🚀 **Use it directly from the Terminal**: Optionally, you can use Scrapling to scrape a URL without writing a single line of code!
- 🛠️ **Rich Navigation API**: Advanced DOM traversal with parent, sibling, and child navigation methods.
- 🧬 **Enhanced Text Processing**: Built-in regex, cleaning methods, and optimized string operations.
- 📝 **Auto Selector Generation**: Generate robust CSS/XPath selectors for any element.
- 🔌 **Familiar API**: Similar to Scrapy/BeautifulSoup with the same pseudo-elements used in Scrapy/Parsel.
- 🤝 **Drop-in Scrapy Integration**: Already invested in Scrapy? Decorate any callback with `scrapling_response` to parse the responses you already fetch with Scrapling's parser, no rewrite needed.
- 📘 **Complete Type Coverage**: Full type hints for excellent IDE support and code completion. The entire codebase is automatically scanned with **PyRight** and **MyPy** with each change.
- 🔋 **Ready Docker image**: With each release, a Docker image containing all browsers is automatically built and pushed.

## Getting Started

Let's give you a quick glimpse of what Scrapling can do without deep diving.

### Basic Usage

HTTP requests with session support

```
from scrapling.fetchers import Fetcher, FetcherSession

with FetcherSession(impersonate='chrome') as session:  # Use latest version of Chrome's TLS fingerprint
    page = session.get('https://quotes.toscrape.com/', stealthy_headers=True)
    quotes = page.css('.quote .text::text').getall()

# Or use one-off requests
page = Fetcher.get('https://quotes.toscrape.com/')
quotes = page.css('.quote .text::text').getall()
```

Advanced stealth mode

```
from scrapling.fetchers import StealthyFetcher, StealthySession

with StealthySession(headless=True, solve_cloudflare=True) as session:  # Keep the browser open until you finish
    page = session.fetch('https://nopecha.com/demo/cloudflare', google_search=False)
    data = page.css('#padded_content a').getall()

# Or use one-off request style, it opens the browser for this request, then closes it after finishing
page = StealthyFetcher.fetch('https://nopecha.com/demo/cloudflare', solve_cloudflare=True)
data = page.css('#padded_content a').getall()
```

Full browser automation

```
from scrapling.fetchers import StealthyFetcher, StealthySession

with StealthySession(headless=True, disable_resources=False, network_idle=True) as session:  # Keep the browser open until you finish
    page = session.fetch('https://quotes.toscrape.com/', load_dom=False)
    data = page.xpath('//span[@class="text"]/text()').getall()  # XPath selector if you prefer it

# Or use one-off request style, it opens the browser for this request, then closes it after finishing
page = StealthyFetcher.fetch('https://quotes.toscrape.com/')
data = page.css('.quote .text::text').getall()
```

### Spiders

Build full crawlers with concurrent requests, multiple session types, and pause/resume:

```
from scrapling.spiders import Spider, Response

class QuotesSpider(Spider):
    name = "quotes"
    start_urls = ["https://quotes.toscrape.com/"]
    concurrent_requests = 10
    
    async def parse(self, response: Response):
        for quote in response.css('.quote'):
            yield {
                "text": quote.css('.text::text').get(),
                "author": quote.css('.author::text').get(),
            }
            
        next_page = response.css('.next a')
        if next_page:
            yield response.follow(next_page[0].attrib['href'])

result = QuotesSpider().start()
print(f"Scraped {len(result.items)} quotes")
result.items.to_json("quotes.json")
```

Use multiple session types in a single spider:

```
from scrapling.spiders import Spider, Request, Response
from scrapling.fetchers import FetcherSession, AsyncStealthySession

class MultiSessionSpider(Spider):
    name = "multi"
    start_urls = ["https://example.com/"]
    
    def configure_sessions(self, manager):
        manager.add("fast", FetcherSession(impersonate="chrome"))
        manager.add("stealth", AsyncStealthySession(headless=True), lazy=True)
    
    async def parse(self, response: Response):
        for link in response.css('a::attr(href)').getall():
            # Route protected pages through the stealth session
            if "protected" in link:
                yield Request(link, sid="stealth")
            else:
                yield Request(link, sid="fast", callback=self.parse)  # explicit callback
```

Pause and resume long crawls with checkpoints by running the spider like this:

```
QuotesSpider(crawldir="./crawl_data").start()
```

With `crawldir` set, press Ctrl+C to save pending requests and seen URLs. Restart with the same `crawldir` to resume. Save scraped items separately.

Or skip writing the crawling logic altogether with the ready-made templates, like pulling an entire Shopify store's catalog:

```
from scrapling.spiders import ShopifySpider

class MyStore(ShopifySpider):
    target_website = "example.com"

result = MyStore().start()  # Every product in the store, one item per variant
```

The template reads products exposed through public collection endpoints, one item per variant.

```
from scrapling.fetchers import Fetcher

# Rich element selection and navigation
page = Fetcher.get('https://quotes.toscrape.com/')

# Get quotes with multiple selection methods
quotes = page.css('.quote')  # CSS selector
quotes = page.xpath('//div[@class="quote"]')  # XPath
quotes = page.find_all('div', {'class': 'quote'})  # BeautifulSoup-style
# Same as
quotes = page.find_all('div', class_='quote')
quotes = page.find_all(['div'], class_='quote')
quotes = page.find_all(class_='quote')  # and so on...
# Find element by text content
quotes = page.find_by_text('Albert Einstein', first_match=False)

# Advanced navigation
quote_text = page.css('.quote')[0].css('.text::text').get()
quote_texts = page.css('.quote').css('.text::text').getall()  # Chained selectors
first_quote = page.css('.quote')[0]
author = first_quote.css('.author::text').get()
next_quote = first_quote.next
parent_container = first_quote.parent

# Element relationships and similarity
similar_elements = first_quote.find_similar()
below_elements = first_quote.below_elements
```

You can use the parser right away if you don't want to fetch websites like below:

```
from scrapling.parser import Selector

page = Selector("<html>...</html>")
```

And it works precisely the same way!

### Async Session Management Examples

```
import asyncio
from scrapling.fetchers import FetcherSession, AsyncStealthySession

async def main():
    async with FetcherSession(http3=True) as http_session:  # `FetcherSession` is context-aware and can work in both sync/async patterns
        page1 = await http_session.get('https://quotes.toscrape.com/')
        page2 = await http_session.get('https://quotes.toscrape.com/page/2/', impersonate='chrome')
        print(len(page1.css('.quote')), len(page2.css('.quote')))

    async with AsyncStealthySession(max_pages=2) as browser_session:
        urls = ['https://quotes.toscrape.com/js/', 'https://quotes.toscrape.com/js/page/2/']
        pages = await asyncio.gather(*(browser_session.fetch(url, wait_selector='.quote') for url in urls))
        print([len(page.css('.quote')) for page in pages])

asyncio.run(main())
```

## CLI & Interactive Shell

Scrapling includes a powerful command-line interface:

Launch the interactive Web Scraping shell

```
pip install "scrapling[shell]"
scrapling shell
```

Extract pages to a file directly without programming. Use `--css-selector` to pick the content, or `--ai-targeted` to select and sanitize the `body` content. Use `.txt` for text, `.md` for Markdown, or `.html` for HTML.

```
scrapling extract get 'https://quotes.toscrape.com/' quotes.md
scrapling extract get 'https://quotes.toscrape.com/' quotes.txt --css-selector '.quote' --impersonate 'chrome'
scrapling extract fetch 'https://quotes.toscrape.com/js/' quotes.md --css-selector '.quote' --wait-selector '.quote' --no-headless
scrapling extract fetch 'https://nopecha.com/demo/cloudflare' captchas.html --css-selector '#padded_content a' --solve-cloudflare
```

> [!note] Note
> There are many additional features, but we want to keep this page concise, including the MCP server and the interactive Web Scraping Shell. Check out the full documentation [here](https://scrapling.readthedocs.io/en/latest/)

## Performance Benchmarks

Scrapling isn't just powerful-it's also blazing fast. The following benchmarks compare Scrapling's parser with the latest versions of other popular libraries.

### Text Extraction Speed Test (5000 nested elements)

| # | Library | Time (ms) | vs Scrapling |
| --- | --- | --- | --- |
| 1 | Scrapling | 1.99 | 1.0x |
| 2 | Parsel/Scrapy | 2.06 | 1.035 |
| 3 | Raw Lxml | 2.56 | 1.286 |
| 4 | PyQuery | 23.98 | ~12x |
| 5 | Selectolax | 197.02 | ~99x |
| 6 | MechanicalSoup | 1545.15 | ~776.5x |
| 7 | BS4 with Lxml | 1562.1 | ~785.0x |
| 8 | BS4 with html5lib | 3412.73 | ~1714.9x |

### Element Similarity & Text Search Performance

Scrapling's adaptive element finding capabilities significantly outperform alternatives:

| Library | Time (ms) | vs Scrapling |
| --- | --- | --- |
| Scrapling | 2.3 | 1.0x |
| AutoScraper | 12.58 | 5.47x |

> All benchmarks represent averages of 100+ runs. See [benchmarks.py](https://github.com/D4Vinci/Scrapling/blob/main/benchmarks.py) for methodology.

## Installation

Scrapling requires Python 3.10 or higher:

```
pip install scrapling
```

> [!important] Important
> This installation only includes the parser engine and its dependencies, without any fetchers or commandline dependencies. So importing anything from `scrapling.fetchers` or `scrapling.spiders`, like in the examples above, will raise `ModuleNotFoundError` with this installation alone. If you are going to use any of the fetchers or spiders, install the fetchers' dependencies first as shown below.

### Optional Dependencies

1. If you are going to use any of the extra features below, the fetchers, or their classes, you will need to install fetchers' dependencies and their browser dependencies as follows:
	```
	pip install "scrapling[fetchers]"
	scrapling install           # normal install
	scrapling install  --force  # force reinstall
	```
	This downloads all browsers, along with their system dependencies and fingerprint manipulation dependencies.
	Or you can install them from the code instead of running a command like this:
	```
	from scrapling.cli import install
	install([], standalone_mode=False)          # normal install
	install(["--force"], standalone_mode=False) # force reinstall
	```
2. Extra features:
	- Install the MCP server feature:
		```
		pip install "scrapling[ai]"
		```
	- Install dependencies for ([building RAG systems](https://scrapling.readthedocs.io/en/latest/ai/building-rag-systems.html)):
		```
		pip install "scrapling[rag]"
		```
	- Install shell features (Web Scraping shell and the `extract` command):
		```
		pip install "scrapling[shell]"
		```
	- Install everything:
		```
		pip install "scrapling[all]"
		```
	Run `scrapling install` when you need browser fetchers. HTTP requests do not need a browser installation.

### Docker

You can also install a Docker image with all extras and browsers with the following command from DockerHub:

```
docker pull pyd4vinci/scrapling
```

Or download it from the GitHub registry:

```
docker pull ghcr.io/d4vinci/scrapling:latest
```

This image is automatically built and pushed using GitHub Actions and the repository's main branch.

## Contributing

We welcome contributions! Please read our [contributing guidelines](https://github.com/D4Vinci/Scrapling/blob/main/CONTRIBUTING.md) before getting started.

## Disclaimer

> [!caution] Caution
> This library is provided for educational and research purposes only. By using this library, you agree to comply with local and international data scraping and privacy laws. The authors and contributors are not responsible for any misuse of this software. Always respect the terms of service of websites and robots.txt files.

## 🎓 Citations

If you have used our library for research purposes please quote us with the following reference:

```
@misc{scrapling,
  author = {Karim Shoair},
  title = {Scrapling},
  year = {2024},
  url = {https://github.com/D4Vinci/Scrapling},
  note = {A Python Web Scraping framework for everything from a single request to a full-scale crawl!}
}
```

## License

This work is licensed under the BSD-3-Clause License.

## Acknowledgments

This project includes code adapted from:

- Parsel (BSD License)-Used for [translator](https://github.com/D4Vinci/Scrapling/blob/main/scrapling/core/translator.py) submodule

---

Designed & crafted with ❤️ by Karim Shoair.