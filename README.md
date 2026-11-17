# Turismo y ocupación hotelera en Colombia

Proyecto de Minería de Datos — José Luis Brito Henao · Luis Eduardo Trujillo Agudelo

## Contenido principal

- **Análisis reproducible:** `Turismov1.2/Turismov1/notebooks/Mineria_Datos_Turismo_Colombia.ipynb`
- **Dataset limpio de ocupación:** `Turismov1.2/Turismov1/data/02_turismo_colombia_limpio.xlsx`
- **Fuente EMA:** `Turismov1.2/Turismov1/data/anex-EMA-jul2026 (1).xlsx`
- **Dashboard:** `index.html`
- **Guías del curso:** `guias/`

El dataset limpio contiene 1,092 filas mensuales para 12 regiones, de enero de 2019 a julio de 2026. La ocupación tiene 1,087 valores disponibles; cinco son faltantes para San Andrés entre abril y agosto de 2020. Los establecimientos y habitaciones corresponden a capacidad regional y no deben sumarse repetidamente por cada mes.

## Ejecutar

1. Abrir la libreta principal en Jupyter y ejecutar sus celdas en orden. Las rutas de datos se resuelven desde el repositorio.
2. Servir la raíz del proyecto por HTTP para usar el dashboard, que carga el dataset limpio al iniciar. Por ejemplo: `python -m http.server 8000`.
3. Abrir `http://localhost:8000`. También se pueden cargar archivos Excel o CSV desde el dashboard; esos archivos permanecen en el almacenamiento local del navegador.

## Alcance y limitaciones

El análisis cubre limpieza, exploración, variables derivadas, agrupamiento y clasificación, además de indicadores regionales, variación de ingresos reales y una referencia de pronóstico estacional. El backtest de referencia para agosto–octubre de 2025 obtuvo un MAPE de 5.49% sobre 36 observaciones; es un pronóstico estacional ingenuo, no evidencia causal.

La fuente EMA disponible llega a julio de 2026. Por tanto, estos datos no permiten medir el efecto del terremoto en meses posteriores ni separar ocupación e ingresos mensuales por tipo de establecimiento. La hoja de ingresos presenta variaciones porcentuales anuales y no niveles de ingresos; las comparaciones encadenadas son relativas y se interpretan con esa limitación.

## Publicación

Para GitHub Pages, configurar `Settings → Pages` para publicar desde `main` y la carpeta raíz. El dashboard requiere HTTP/HTTPS para descargar el archivo Excel incluido.
