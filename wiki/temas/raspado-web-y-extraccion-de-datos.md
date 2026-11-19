# Raspado web y extracción de datos

Página de tema construida a partir de una sola fuente: el README de Scrapling. Describe cómo se organiza hoy una pila de raspado y qué decisiones quedan abiertas; no hay todavía una segunda fuente que permita comparar enfoques.

## La pila, por capas

Raspado web se puede descomponer en cuatro capas que conviene decidir por separado:

1. **Obtención.** Elegir entre una petición HTTP (rápida, barata, no ejecuta JavaScript) y un navegador automatizado (ejecuta JavaScript, resuelve retos anti-bot, cuesta mucho más en CPU, memoria y tokens si hay un modelo en el medio). La fuente insiste en que muchas páginas no necesitan navegador y que confundir ambas capas es la causa más común de coste innecesario.
2. **Análisis y extracción.** Selectores CSS o XPath sobre el DOM, con el problema abierto de la fragilidad: cuando el sitio cambia su maquetación, los selectores se rompen. La fuente propone seguimiento adaptativo y búsqueda de elementos similares como mitigación, sin explicar el mecanismo.
3. **Rastreo.** Recorrer un sitio completo implica concurrencia, límites por dominio, reintentos, pausa/reanudación y cumplimiento de `robots.txt`. Es donde aparecen los fallos de producción: rastreos que no terminan, bloqueos por exceso de tasa y pérdida de progreso.
4. **Salida.** Exportar a JSON, JSONL, CSV o XML, o convertir directamente a Markdown limpio para alimentar un índice de recuperación o un modelo.

## Qué aporta el caso Scrapling

Según la fuente, Scrapling integra las cuatro capas en una biblioteca con una API familiar (similar a Scrapy y BeautifulSoup) y añade tres piezas que normalmente se arman a mano: AutoThrottle que ajusta el retardo por dominio según la respuesta del servidor, modo desarrollo con respuestas cacheadas en disco para iterar la lógica de `parse()` sin volver a golpear el sitio, y salida Markdown sin LLM en el circuito para construir corpus de recuperación. El detalle está en el [resumen de la fuente](../fuentes/scrapling-framework-raspado-web.md).

## Punto de contacto con los flujos agénticos

La fuente presenta un servidor MCP y un Agent Skill como las vías para que un agente raspe sin escribir código de scraping. Esto cruza con el tema de [flujos de trabajo agénticos](flujos-de-trabajo-agenticos.md) en dos lugares concretos: la interfaz agente-computadora (herramientas acotadas y con descripciones precisas, en lugar de un agente libre con un navegador) y el riesgo de inyección de prompts, ya que cualquier texto raspado que llega al modelo es entrada no confiable. La fuente menciona que el servidor MCP reduce el contenido enviado al modelo y aplica protección contra inyección de prompts, sin detallar cómo.

## Decisiones que quedan abiertas

- Cuándo vale la pena un framework completo frente a una petición HTTP y un parser.
- Cómo se mide el coste real de la capa anti-bot (proxies, resolución de CAPTCHA, tiempo de navegador).
- Qué obligaciones legales aplican al uso comercial de los datos raspados: la fuente se limita a una advertencia genérica de uso educativo.

Con una sola fuente, estas preguntas quedan como preguntas, no como conclusiones.

## Evidencia

- [Scrapling: framework de raspado web (D4Vinci)](../fuentes/scrapling-framework-raspado-web.md) — README del proyecto, capturado el 2026-11-18.
