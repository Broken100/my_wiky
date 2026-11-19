# Auditoría de fuentes — 2026-11-19

**Alcance:** comprobación puntual de disponibilidad y de las afirmaciones identificadas abajo, solicitada explícitamente. La disponibilidad de una página no demuestra la veracidad de su contenido. Esta revisión no valida íntegramente ninguna fuente, transcripción ni resumen. Los originales en `fuentes/` se conservaron sin cambios.

## Hallazgos

- **Scrapling — declaración del proyecto:** el [repositorio y su README](https://github.com/D4Vinci/Scrapling) estaban disponibles y presentan Scrapling como framework Python que reúne peticiones HTTP, navegadores, análisis y rastreo. Son capacidades declaradas por el autor; no se ejecutó el software, no se inspeccionaron los tests y no se verificó el rendimiento ni los benchmarks. [Resumen de la captura](../fuentes/scrapling-framework-raspado-web.md).
- **Plugins de Anthropic — declaración del repositorio:** el [README y la página del repositorio](https://github.com/anthropics/knowledge-work-plugins) estaban disponibles; la página muestra licencia Apache-2.0 y el README declara 11 plugins para Cowork, compatibles con Claude Code, con skills, conectores, comandos y subagentes. No se probó cada componente ni su funcionamiento. [Resumen de Yarad](../fuentes/plugins-trabajo-conocimiento-yarad.md).
- **Publicación de Yarad:** [el post en X](https://x.com/Yarad_ai/status/2121595480094167435) devolvió HTTP 403; su texto en vivo no pudo comprobarse. El repositorio accesible no confirma por sí solo el contenido del post.
- **Curso de Hermes:** en la revisión previa de esta sesión se abrió [el video](https://www.youtube.com/watch?v=FQZGoSwdS_0) con título coincidente con la captura; el contenido no se revisó íntegramente. Un intento posterior encontró un bloqueo por tráfico inusual. El título no demuestra la fidelidad de la transcripción ni las afirmaciones del curso. [Resumen](../fuentes/hermes-2026-curso-completo.md).
- **Conversación compartida de Gemini:** [el enlace](https://share.gemini.google/xc2O63zQoBt3) redirigió a Gemini y solo mostró una pantalla de acceso; su texto no pudo verificarse en esta sesión. El [registro histórico](../../registro.md) documenta una comparación anterior con el archivo y las tablas: se conserva esa constatación previa, sin presentarla como una nueva comprobación. [Resumen](../fuentes/flujos-agentes-produccion-2026.md).
- **Postman — resultados atribuidos a la encuesta:** el [comunicado de Postman publicado por Business Wire en FinancialContent](https://www.financialcontent.com/article/bizwire-2026-11-18-one-in-four-organizations-now-run-ai-agents-on-apis-in-production-postmans-2026-state-of-the-api-report-finds) afirma que el 25% de las organizaciones ejecuta agentes que usan API en producción; el 25% tiene controles para agentes de IA, el 22% controles para MCP y el 40% no ha abordado credenciales e identidad de agentes. La cifra del 22% **no** afirma que el 22% de servidores MCP esté protegido. Son resultados comunicados por Postman sobre encuestados, no estimaciones generalizables automáticamente ni una verificación independiente de la metodología.
- **Gartner — evidencia limitada:** la [página de Gartner](https://www.gartner.com/en/newsroom/press-releases/2025-06-25-gartner-predicts-over-40-percent-of-agentic-ai-projects-will-be-canceled-by-end-of-2027) denegó el acceso directo. Una búsqueda devolvió un fragmento identificado con esa página: «Over 40% of agentic AI projects will be canceled by the end of 2027». Es una predicción, no un hecho consumado. Se comprobó solo ese fragmento, no el comunicado completo; los resultados de búsqueda pueden cambiar.

## Límites comunes

No se revisaron otras afirmaciones, cifras, licencias, precios, fechas, comparativas, recomendaciones o contenidos de estas fuentes salvo lo indicado expresamente. La lectura de un comunicado o README verifica que su autor hace una declaración, no que esta sea verdadera en todos los contextos. No se ejecutaron productos, benchmarks ni procedimientos del curso. El acceso y los contenidos pueden cambiar.

## Páginas relacionadas

- [Flujos de trabajo agénticos](../temas/flujos-de-trabajo-agenticos.md)
- [Plugins para trabajo de conocimiento](../temas/plugins-para-trabajo-de-conocimiento.md)
- [Procedimiento reutilizable de verificación](../procedimientos/verificacion-de-fuentes.md)
