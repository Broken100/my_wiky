# Auditoría comparativa de entornos: OpenBot (wiky_llm) frente a Hermes — 2026-11-19

**Alcance y límites:** auditoría de solo verificación sobre los 13 cambios locales producidos en el entorno Hermes (2026-11-19, entre las 03:24 y las 03:28 hora local) y comparación basada en evidencia entre **OpenBot (`wiky_llm`)** y **Hermes** a partir de los artefactos, enlaces e historial Git de la bóveda. No se realizaron consultas web ni se midieron latencia de ejecución, consumo de tokens, coste ni canales de mensajería.

## 1. Auditoría de los cambios locales recientes (entorno Hermes)

### Inventario y estado

- **8 archivos rastreados modificados:** [indice.md](../../indice.md), [registro.md](../../registro.md), cuatro resúmenes en `wiki/fuentes/` ([flujos agénticos](../fuentes/flujos-agentes-produccion-2026.md), [curso de Hermes](../fuentes/hermes-2026-curso-completo.md), [plugins de Yarad](../fuentes/plugins-trabajo-conocimiento-yarad.md) y [Scrapling](../fuentes/scrapling-framework-raspado-web.md)) y dos temas ([flujos agénticos](../temas/flujos-de-trabajo-agenticos.md) y [plugins para trabajo de conocimiento](../temas/plugins-para-trabajo-de-conocimiento.md)).
- **5 archivos nuevos:** la síntesis de sesión [proyecto-placas-nfc-qr-avance-2026-11-19.md](../../fuentes/adjuntos/proyecto-placas-nfc-qr-avance-2026-11-19.md), su resumen [proyecto-placas-nfc-qr-avance-2026-11-19.md](../fuentes/proyecto-placas-nfc-qr-avance-2026-11-19.md), el tema [proyecto-placas-nfc-qr-resenas.md](../temas/proyecto-placas-nfc-qr-resenas.md), la auditoría [verificacion-fuentes-2026-11-19.md](verificacion-fuentes-2026-11-19.md) y el procedimiento [verificacion-de-fuentes.md](../procedimientos/verificacion-de-fuentes.md).

### Aspectos verificados como correctos

- **Integridad de enlaces:** antes de añadir esta auditoría, los 26 documentos `.md` sumaban 117 enlaces internos con **0 enlaces rotos nuevos**; los únicos 4 enlaces rotos corresponden al historial de *Cafeterías de Otavalo* en [registro.md](../../registro.md) (eliminadas el 2026-11-18).
- **Respeto de originales previos e historial:** las 4 fuentes originales previas en `fuentes/` permanecieron intactas y [registro.md](../../registro.md) conservó todas las entradas anteriores al añadir los asientos `lint` e `ingest` al final.
- **Rigor epistémico y trazabilidad:** en [verificacion-fuentes-2026-11-19.md](verificacion-fuentes-2026-11-19.md) se corrigió una lectura inexacta de Postman (el 22% corresponde a organizaciones con controles para MCP, no al 22% de servidores MCP protegidos) y se documentaron bloqueos de acceso reales (HTTP 403 en X, pantalla de acceso en Gemini, tráfico inusual en YouTube y acceso denegado en Gartner) sin inventar datos. En el proyecto de placas NFC/QR se separaron los hechos del prototipo local (`C:/Users/Luis/nfc-reviews`, compilación y 5 pruebas HTTP aisladas frente a recorrido autenticado no verificado y bloqueo previo del servidor Next.js) de las funciones pendientes y de las hipótesis comerciales.

### Hallazgos y desviaciones respecto a las reglas de la bóveda

1. **Cambios sin sincronizar en Git:** los 13 archivos permanecían solo en local mientras `origin/main` seguía en `0d0481b`, sin respaldo remoto ni commit convencional asociado.
2. **Ubicación y metadatos de la nueva fuente:** [proyecto-placas-nfc-qr-avance-2026-11-19.md](../../fuentes/adjuntos/proyecto-placas-nfc-qr-avance-2026-11-19.md) se guardó en `fuentes/adjuntos/` (reservada según [README.md](../../README.md) y [AGENTS.md](../../AGENTS.md) para adjuntos insertados desde Obsidian) y sin bloque *frontmatter* YAML, a diferencia de las 4 fuentes existentes en `fuentes/`.
3. **Subcarpetas no documentadas:** se crearon `wiki/auditorias/` y `wiki/procedimientos/` (que se suman a `wiki/proyectos-2026/`) sin actualizar su existencia en [README.md](../../README.md) ni en [AGENTS.md](../../AGENTS.md).
4. **Ruta acoplada a una máquina:** el paso 6 de [verificacion-de-fuentes.md](../procedimientos/verificacion-de-fuentes.md) fija una ruta local de usuario (`C:/Users/Luis/AppData/Local/hermes/skills/note-taking/obsidian/scripts/check_links.py`) y depende de la skill local `verificador-fuentes-wiky`.
5. **Tensión explícita con la regla de no consultar la web:** [verificacion-de-fuentes.md](../procedimientos/verificacion-de-fuentes.md) reconoce que [AGENTS.md](../../AGENTS.md) prohíbe navegar la web de forma general e introduce una excepción manual bajo petición explícita sin haber actualizado el propio `AGENTS.md`.
6. **Mezcla de saltos de línea:** 7 archivos rastreados modificados (`registro.md`, 4 resúmenes en `wiki/fuentes/` y 2 páginas en `wiki/temas/`) quedaron con finales `LF` dentro de un repositorio Windows con `CRLF`.

## 2. Comparativa basada en evidencia: OpenBot (`wiky_llm`) frente a Hermes

### Evidencia observada sobre OpenBot (`wiky_llm`)

- **Fortalezas verificadas:**
  - **Gobernanza Git continua:** delegación dedicada en `registro` (`registro_llm`) con rutina cada 15 minutos, commits convencionales sin `Co-Authored-By`, respeto estricto de `.gitignore` (`.obsidian/workspace.json` y `.obsidian/workspace-mobile.json` excluidos) y empuje limpio a `origin/main` (commits `3336f47` a `0d0481b`).
  - **Disciplina estructural y confirmaciones:** respeto consistente de `fuentes/` con *frontmatter* YAML, separación estricta entre originales y `wiki/`, y tarjetas de confirmación explícita antes de borrados irreversibles (commit `af09a78`).
- **Debilidades verificadas:**
  - **Fricción y errores de traspaso multiagente:** el 2026-11-19 `consulta` falló inicialmente al recorrer el archivo de Scrapling (ruta de 282 caracteres en Windows, >260) y `auditor` devolvió un estado parcial tras escribir dos imprecisiones factuales en el asiento `lint` del commit `0d0481b` en [registro.md](../../registro.md) (afirmó que `fuentes/adjuntos/` no existía cuando existía vacía, y contó 4 archivos base en raíz en lugar de 5), lo que obligó al orquestador `wiky` a re-verificar el repositorio directamente.
  - **Rigidez frente a fuentes con errores externos:** la aplicación estricta de no consultar la web impidió detectar el error de lectura del 22% de Postman en la guía de flujos agénticos.

### Evidencia observada sobre Hermes

- **Fortalezas verificadas:**
  - **Precisión epistémica y profundidad en una sola sesión:** combinó desarrollo local (`C:/Users/Luis/nfc-reviews`), verificación externa acotada bajo permiso explícito y documentación en la bóveda sin pérdida de contexto entre subagentes, produciendo 13 cambios con 0 enlaces rotos nuevos.
  - **Separación clara entre hechos, bloqueos e hipótesis:** evidente tanto en [verificacion-fuentes-2026-11-19.md](verificacion-fuentes-2026-11-19.md) como en [proyecto-placas-nfc-qr-avance-2026-11-19.md](../../fuentes/adjuntos/proyecto-placas-nfc-qr-avance-2026-11-19.md).
- **Debilidades verificadas:**
  - **Convenciones ad hoc y falta de cierre Git:** ubicó una fuente nueva en `fuentes/adjuntos/` sin *frontmatter*, abrió subcarpetas sin documentarlas en `README.md`/`AGENTS.md`, dejó una ruta local de equipo en un procedimiento de la bóveda, escribió `LF` en archivos `CRLF` y no ejecutó ni automatizó commit/push.

## 3. Interpretación y recomendación: cuál conviene más

*Esta sección es una **interpretación** basada en la evidencia anterior y en el principio documentado en [Flujos de trabajo agénticos](../temas/flujos-de-trabajo-agenticos.md): empezar por el patrón más simple que resuelva la tarea y no introducir coordinación multiagente salvo donde el contexto o la segregación de dominios lo justifique.*

- **Cuándo es mejor Hermes:** para sesiones interactivas profundas donde se mezclan programación, verificación externa autorizada y redacción de análisis o auditorías complejas. Un único arnés con contexto continuo evita los errores de traspaso y resume con mayor rigor epistémico, siempre que se le exija respetar las carpetas y *frontmatter* de `AGENTS.md` y cerrar cada sesión con sincronización Git.
- **Cuándo es mejor OpenBot (`wiky_llm`):** para la operación continua, el mantenimiento disciplinado y el respaldo automático de la bóveda (rutina Git cada 15 minutos, cumplimiento estricto de `fuentes/`, confirmaciones explícitas para acciones destructivas y acceso desde chat en cualquier dispositivo), siempre que se reduzcan delegaciones innecesarias en lecturas simples y todos los agentes usen un mismo verificador compatible con rutas largas de Windows.
- **Advertencia crítica si usas ambos en paralelo:** operar Hermes y OpenBot simultáneamente sobre `D:/2026/bovedas/wily_llm/wiky_llm` sin delimitar quién escribe y cuándo entra en juego la rutina de 15 minutos de `registro` crea riesgo de colisión en [indice.md](../../indice.md) o [registro.md](../../registro.md) y de commits prematuros a mitad de una sesión de Hermes. Si mantienes ambos entornos, conviene usar Hermes en sesiones acotadas de creación/código (pausando la rutina o dejando que `registro` sincronice solo al cerrar la sesión) o designar un único entorno escritor y el otro como lector.

## 4. Qué no se pudo comprobar

- Latencia real, consumo de tokens y coste económico por tarea en ninguno de los dos entornos.
- Fiabilidad de canales externos (por ejemplo Telegram o gateway) ni ejecución práctica del prototipo `C:/Users/Luis/nfc-reviews`.
- Disponibilidad o contenido vivo de las URLs externas citadas en [verificacion-fuentes-2026-11-19.md](verificacion-fuentes-2026-11-19.md).

## Páginas relacionadas

- [Verificación de fuentes del 2026-11-19](verificacion-fuentes-2026-11-19.md)
- [Procedimiento: verificador de fuentes](../procedimientos/verificacion-de-fuentes.md)
- [Flujos de trabajo agénticos](../temas/flujos-de-trabajo-agenticos.md)
- [Proyecto de placas NFC y QR para reseñas](../temas/proyecto-placas-nfc-qr-resenas.md)
- [Cómo configurar Hermes](../proyectos-2026/como-configurar-hermes.md)
- [Índice](../../indice.md) y [Registro](../../registro.md)
