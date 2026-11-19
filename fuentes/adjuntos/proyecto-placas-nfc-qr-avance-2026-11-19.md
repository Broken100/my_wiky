# Proyecto de placas NFC y QR para reseñas: proceso y estado

**Fecha:** 2026-11-19  
**Origen:** síntesis de la conversación de Luis con Hermes y de las comprobaciones informadas durante la sesión. No es una transcripción literal.  
**Estado:** desarrollo inicial, solo local; no apto para clientes ni para producción.

## Objetivo y alcance

Luis quiere ofrecer a negocios locales placas que faciliten el acceso a su enlace de reseñas de Google mediante NFC o QR. La administración de negocios, enlaces y estadísticas será inicialmente exclusiva de Luis. La idea de mini-sitios y acceso a redes sociales se considera un negocio separado y queda fuera de este primer proyecto.

La placa no garantiza mejoras en el posicionamiento de Google. Un acceso registrado no demuestra que una persona haya publicado una reseña ni identifica de manera fiable a una persona única.

## Oferta e hipótesis comerciales

- Formato tipo estante con NFC y QR: USD 15.
- Tarjeta plegable NFC: USD 20.
- Tarjeta NFC personalizada: desde USD 25; precio a tratar por WhatsApp.
- Pago único, sin suscripción planteada por ahora. El costo de fabricación estimado para la placa de USD 15 es de aproximadamente USD 2. Quedan fuera tiempo, adquisición, entrega, plataforma, soporte e impuestos.
- Se planteó entregar un PDF mensual de accesos. Tres meses incluidos son una propuesta para evaluar, no una decisión final. La duración del enlace y la de los reportes son decisiones separadas y pendientes.
- Presupuesto objetivo de plataforma y mapa: USD 50–70 mensuales; no es un gasto ya medido. Objetivo de 50 negocios el primer mes y 200 al tercer mes; no son ventas realizadas.
- Disponibilidad para ventas y entregas: seis horas por día, de lunes a viernes. Aún no hay ventas ni tiempos de venta por placa observados.

## Piloto comercial en Otavalo

Primer grupo: cafeterías de Otavalo, Ecuador. La herramienta de prospección futura mostrará cafeterías con hasta 50 reseñas, inclusive, ordenadas de menor a mayor; los casos sin dato disponible se mostrarán separados. Las estrellas y la cantidad de reseñas son señales iniciales, no pruebas de intención de compra. Luis elegirá visitas y caminará entre ellas.

Se propone registrar visitas, interés, ventas, tiempo y gastos para comprobar si la oferta funciona. La búsqueda no garantizará encontrar todos los establecimientos y el sistema no promete datos instantáneos.

## Flujo funcional acordado

1. Crear la ficha de un negocio y comprobar manualmente su enlace de reseñas.
2. Generar identificadores distintos para QR y NFC cuando el formato incluya ambos.
3. Imprimir el QR y grabar el enlace NFC; comprobarlos con un teléfono antes de la entrega.
4. Permitir cambiar el destino manteniendo los enlaces de la placa.
5. Registrar accesos por medio y fecha, distinguiendo pruebas de tráfico normal.
6. Consultar datos y generar un PDF mensual para revisión y entrega manual.
7. Más adelante, buscar prospectos en un mapa, registrar visitas y abrir navegación a pie en Google Maps.

## Antecedente revisado

Se revisaron una hoja de Google Sheets de prueba con pestañas `Config` y `Hits`, dos scripts de Apps Script compartidos por Luis y el archivo `index.html` del repositorio https://github.com/Broken100/system_QR. El script de descarga de QR entregado codifica el destino de la columna C; si allí se coloca directamente el enlace de Google, se omite la redirección intermediaria y el registro. La hoja y el sistema anterior contenían datos de prueba, por lo que no se requiere preservar su implementación.

## Stack propuesto

Aplicación Next.js y TypeScript, Supabase/PostgreSQL y autenticación para la futura versión de producción, Vercel para alojamiento, y Google Maps/Places para prospección posterior. Se propuso generar QR y PDF con bibliotecas en la aplicación. La búsqueda de Google debe quedar fuera del recorrido de redirección de las placas.

Estimación orientativa investigada durante la sesión: Vercel Pro USD 20/mes y Supabase Pro desde USD 25/mes para un proyecto pequeño; Google Places depende de las consultas y de los cupos. El subtotal de USD 45 excluye dominio, impuestos y excedentes y no es un presupuesto garantizado. Las condiciones, precios, disponibilidad y reglas de almacenamiento de datos de Google deben volver a comprobarse antes del despliegue.

## Implementación realizada hasta ahora

Se creó un prototipo local en `C:/Users/Luis/nfc-reviews`, con Next.js, TypeScript y SQLite. La documentación técnica está en `README.md` del proyecto.

- Permite crear identificadores de enlaces, cambiar destinos, realizar redirecciones temporales y contar solicitudes normales y de prueba por separado.
- El almacenamiento local persiste entre reinicios. La API administrativa rechaza secretos ausentes, demasiado cortos o incorrectos.
- La compilación pasó. Cinco pruebas HTTP pasaron en una copia aislada, incluyendo cambio de destino, contadores y persistencia tras reinicio.
- Una ejecución de pruebas en el proyecto original falló en la preparación por el bloqueo de un servidor de desarrollo Next.js. En una comprobación posterior el puerto usado por el verificador no respondió; no se debe asumir que el proceso siga activo sin volver a verificar.
- Se comprobó validación básica de formularios y rechazo sin credenciales en el navegador. El recorrido autenticado completo por la interfaz no se verificó.

**Limitaciones importantes:** es exclusivamente local. No hay integración de Supabase, dominio público, creación de imágenes QR, grabación NFC, generación de PDF, mapa, autenticación de producción ni MFA. La protección actual con secreto no es adecuada para producción; falta revisión independiente de seguridad. La medición es de solicitudes y puede incluir repeticiones y accesos automáticos; el marcado de prueba es manipulable. Fallas de lectura de la base pueden impedir redirecciones. No se crearon cuentas de pago ni se publicó una aplicación. El directorio no era un repositorio Git en la última comprobación.

## Próximos pasos

1. Verificar el estado de los procesos locales y reproducir las pruebas del proyecto sin interferir con otros servidores.
2. Probar el flujo autenticado completo mediante la interfaz.
3. Elegir y probar el diseño de autenticación de administrador y persistencia en Supabase antes de cualquier publicación.
4. Implementar QR, soporte para los medios de cada placa y pruebas físicas; después reportes y mapa.
5. Realizar una revisión independiente de seguridad y probar respaldos, recuperación y fallos antes de usar placas con clientes.
6. Definir duración del enlace y de los reportes, y medir costos y tiempos reales del piloto.

## Alcance de esta nota

Resume lo hablado y los resultados de herramientas comunicados en esta sesión. No constituye una auditoría completa de código, una estimación contractual, asesoría legal ni prueba de que el sistema esté listo para operar.
