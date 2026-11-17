# Turismo y ocupación hotelera en Colombia

Proyecto de Minería de Datos — José Luis Brito Henao · Luis Eduardo Trujillo Agudelo

## Contenido principal

- **Análisis reproducible:** `Turismov1.2/Turismov1/notebooks/Mineria_Datos_Turismo_Colombia.ipynb`
- **Dataset limpio de ocupación:** `Turismov1.2/Turismov1/data/02_turismo_colombia_limpio.xlsx`
- **Fuente EMA:** `Turismov1.2/Turismov1/data/anex-EMA-jul2026 (1).xlsx`
- **Dashboard:** `index.html`
- **Guías del curso:** `guias/`

El dataset limpio contiene 1,092 filas mensuales para 12 regiones, de enero de 2019 a julio de 2026. La ocupación tiene 1,087 valores disponibles; cinco son faltantes para San Andrés entre abril y agosto de 2020. Los establecimientos y habitaciones corresponden a capacidad regional y no deben sumarse repetidamente por cada mes.

## Fuentes, flujo y conciliación

- `anex-EMA-jul2026 (1).xlsx` contiene 25 hojas. El análisis usa la hoja mensual de ocupación y la de variación anual de ingresos reales; no combina las demás porque sus métricas y niveles de agregación son distintos.
- `thwd-ivmp.xlsx` es la extracción RNT de alojamientos de 2026. Tras normalizar los nombres regionales, se concilia con las 12 regiones: 498 establecimientos, 27,109 habitaciones, 61,286 camas y 15,346 empleados. El `thwd-ivmp.csv` histórico de 2019 no se mezcla con esta extracción.
- `01_turismo_colombia_integrado.xlsx` tiene 1,188 filas: 1,092 registros mensuales y 96 filas de notas u otros contenidos. Las 1,092 claves mensuales coinciden con `02_turismo_colombia_limpio.xlsx`.
- `02_turismo_colombia_limpio.xlsx` es la base analítica del dashboard: 12 regiones por 91 meses. Mantiene los cinco faltantes reales de ocupación, no como ceros; no presenta claves mensuales duplicadas ni ocupaciones fuera de 0–100%.
- `03_turismo_colombia_modelado.xlsx` conserva 1,092 registros y los faltantes; `04_turismo_colombia_segmentado.xlsx` usa las 1,087 observaciones completas. `05_turismo_colombia_prediccion.xlsx` contiene 327 observaciones de prueba, no una proyección futura: TN/FP/FN/TP = 267/10/17/33, exactitud 91.74%, precisión 76.74%, recall 66.00% y F1 70.97%.

La ocupación media simple es 44.74% sobre 1,087 observaciones. Los cuadernos alternativos de la raíz y el CSV RNT de 2019 son históricos, no fuentes canónicas para reproducir los resultados actuales. La libreta principal incluye una celda de auditoría para volver a comprobar la cobertura, conciliación y métricas del flujo.

## Ejecutar

1. Abrir la libreta principal en Jupyter y ejecutar sus celdas en orden. Las rutas de datos se resuelven desde el repositorio.
2. Servir la raíz del proyecto por HTTP para usar el dashboard, que carga el dataset limpio al iniciar. Por ejemplo: `python -m http.server 8000`.
3. Abrir `http://localhost:8000`. También se pueden cargar archivos Excel o CSV desde el dashboard; esos archivos permanecen en el almacenamiento local del navegador.

## Alcance y limitaciones

El análisis cubre limpieza, exploración, variables derivadas, agrupamiento y clasificación, además de indicadores regionales, variación de ingresos reales y una referencia de pronóstico estacional. El backtest de referencia para agosto–octubre de 2025 obtuvo un MAPE de 5.49% sobre 36 observaciones; es un pronóstico estacional ingenuo, no evidencia causal.

La fuente EMA disponible llega a julio de 2026. Por tanto, estos datos no permiten medir el efecto del terremoto en meses posteriores ni separar ocupación e ingresos mensuales por tipo de establecimiento. La hoja de ingresos presenta variaciones porcentuales anuales y no niveles de ingresos; las comparaciones encadenadas son relativas y se interpretan con esa limitación.

## Publicación

Para GitHub Pages, configurar `Settings → Pages` para publicar desde `main` y la carpeta raíz. El dashboard requiere HTTP/HTTPS para descargar el archivo Excel incluido.
