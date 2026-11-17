# Turismo y Ocupación Hotelera en Risaralda

Proyecto de Minería de Datos — José Luis Brito Henao · Luis Eduardo Trujillo Agudelo


## Flujo de trabajo

1. **`notebooks/Mineria_Datos_Turismo_Colombia.ipynb`** procesa los archivos crudos de `data/raw/` y genera los 3 xlsx en `data/`.
2. **`index.html`** carga automáticamente `data/02_turismo_colombia_limpio.xlsx` al abrirse (si se sirve por HTTP). Si se abre en local (`file://`), muestra un botón para subir el archivo manualmente.
3. El dashboard calcula indicadores, gráficas y hallazgos automáticamente.

## Despliegue

- **GitHub Pages**: subir el repo y activar Pages en `Settings → Pages → Branch: main / root`.
- **Local**: usar Live Server (VSCode) o `python -m http.server 8000` desde la raíz del repo.

## Versión

- v2.1.0 — Carga automática desde `data/`, detección ampliada de columnas EMA/DANE, listado de documentos locales.
- v2.0.0 — Subida manual de Excel/CSV y análisis automático.
- v1.0.0 — Versión inicial.

