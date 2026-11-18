# Turismo y ocupación hotelera en Colombia

Proyecto de Minería de Datos — José Luis Brito Henao · Luis Eduardo Trujillo Agudelo

## Contenido principal

- **Análisis reproducible:** `Turismov1.2/Turismov1/notebooks/Mineria_Datos_Turismo_Colombia.ipynb`
- **Informe técnico:** `INFORME_TECNICO.md`
- **Lista de verificación de guías:** `CHECKLIST_GUIAS.md`
- **Dataset limpio de ocupación:** `Turismov1.2/Turismov1/data/02_turismo_colombia_limpio.xlsx`
- **Fuente EMA de la base principal:** `Turismov1.2/Turismov1/data/anex-EMA-jul2026 (1).xlsx`
- **Seguimiento EMA complementario:** `Turismov1.2/Turismov1/data/anex-EMA-sep2026.xlsx`
- **Dashboard:** `index.html`
- **Guías del curso:** `guias/`

El dataset limpio contiene 1,092 filas mensuales para 12 regiones, de enero de 2019 a julio de 2026. La ocupación tiene 1,087 valores disponibles; cinco son faltantes para San Andrés entre abril y agosto de 2020. Los establecimientos y habitaciones corresponden a capacidad regional y no deben sumarse repetidamente por cada mes.

## Fuentes, flujo y conciliación

- `anex-EMA-jul2026 (1).xlsx` contiene 25 hojas. El análisis usa la hoja mensual de ocupación y la de variación anual de ingresos reales; no combina las demás porque sus métricas y niveles de agregación son distintos.
- `anex-EMA-sep2026.xlsx` es una publicación oficial posterior del DANE, actualizada el 13 de noviembre de 2026. Se usa para seguimiento de agosto (mes del evento) y septiembre (primer mes completo posterior); las cifras de 2026 son provisionales y el archivo no se mezcla con la base principal.
- `thwd-ivmp.xlsx` es la extracción RNT de alojamientos de 2026. Tras normalizar los nombres regionales, se concilia con las 12 regiones: 498 establecimientos, 27,109 habitaciones, 61,286 camas y 15,346 empleados. El `thwd-ivmp.csv` histórico de 2019 no se mezcla con esta extracción.
- `01_turismo_colombia_integrado.xlsx` tiene 1,188 filas: 1,092 registros mensuales y 96 filas de notas u otros contenidos. Las 1,092 claves mensuales coinciden con `02_turismo_colombia_limpio.xlsx`.
- `02_turismo_colombia_limpio.xlsx` es la base analítica del dashboard: 12 regiones por 91 meses. Mantiene los cinco faltantes reales de ocupación, no como ceros; no presenta claves mensuales duplicadas ni ocupaciones fuera de 0–100%.
- `03_turismo_colombia_modelado.xlsx` conserva 1,092 registros y los faltantes; `04_turismo_colombia_segmentado.xlsx` usa las 1,087 observaciones completas. `05_turismo_colombia_prediccion.xlsx` contiene 327 observaciones de prueba, no una proyección futura: TN/FP/FN/TP = 267/10/17/33, exactitud 91.74%, precisión 76.74%, recall 66.00% y F1 70.97%.

La ocupación media simple es 44.74% sobre 1,087 observaciones. Los cuadernos alternativos de la raíz y el CSV RNT de 2019 son históricos, no fuentes canónicas para reproducir los resultados actuales. La libreta principal incluye celdas de auditoría para comprobar la cobertura, conciliación y métricas del flujo.

## Ejecutar

1. Abrir la libreta principal en Jupyter y ejecutar sus celdas en orden. Las rutas de datos se resuelven desde el repositorio.
2. Servir la raíz del proyecto por HTTP para usar el dashboard, que carga el dataset limpio al iniciar. Por ejemplo: `python -m http.server 8000`.
3. Abrir `http://localhost:8000`. También se pueden cargar archivos Excel o CSV desde el dashboard; esos archivos permanecen en el almacenamiento local del navegador.

La libreta incluye el diccionario de datos, controles de calidad y lectura en lenguaje natural de las visualizaciones, segmentaciones y métricas de clasificación. El informe y la lista resumen esos resultados y sus límites sin reemplazar la ejecución reproducible de la libreta.

## Alcance y limitaciones

El pronóstico trimestral es una referencia estacional simple: repite para cada mes la ocupación observada en el mismo mes del año anterior. El backtest de agosto–octubre de 2025 obtuvo un MAPE de 5.49% en 36 pares región-mes. Al contrastar esta referencia con los datos provisionales de agosto y septiembre de 2026 para el Eje Cafetero, el MAPE parcial es 19.21%, por encima del criterio de 15% planteado en el proyecto. Desde el corte de septiembre, la estimación de octubre–diciembre de 2026 es 45.82%, 45.09% y 41.78%. Son dos meses observados y no la evaluación completa del trimestre; las estimaciones no deben presentarse como garantía.

El DANE reportó para el Eje Cafetero ocupación de 35.49% en agosto (mes del evento, que incluye días previos y posteriores) y 36.62% en septiembre (primer mes completo posterior) de 2026, 9.61 y 4.16 puntos porcentuales menos que en esos meses de 2025. También informó variaciones anuales de ingresos reales de -45.0% y -36.7%. Estas diferencias no prueban que el sismo sea su causa; además, las cifras de 2026 son provisionales. La base principal del dashboard todavía termina en julio de 2026, y ninguna de las fuentes permite separar ocupación e ingresos mensuales por tipo de establecimiento. La hoja de ingresos reporta variaciones porcentuales anuales, no niveles monetarios.

Fuente complementaria: [Encuesta Mensual de Alojamiento (EMA), DANE](https://www.dane.gov.co/index.php/estadisticas-por-tema/servicios/encuesta-mensual-de-alojamiento-ema), [anexo de septiembre de 2026](https://www.dane.gov.co/files/operaciones/EMA/anex-EMA-sep2026.xlsx).

## Publicación

Para GitHub Pages, configurar `Settings → Pages` para publicar desde `main` y la carpeta raíz. El dashboard requiere HTTP/HTTPS para descargar el archivo Excel incluido.
