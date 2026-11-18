# Informe técnico: minería de datos de turismo y ocupación hotelera

## Resumen

El análisis integra la Encuesta Mensual de Alojamiento (EMA) del DANE con una extracción del Registro Nacional de Turismo (RNT). La unidad analítica es región turística-mes: 12 regiones entre enero de 2019 y julio de 2026, 1.092 filas y 1.087 valores de ocupación disponibles. La ocupación media es 44,74% y la mediana 43,00%. La libreta principal documenta la integración, calidad, análisis exploratorio, preparación de variables, segmentación y clasificación.

## Fuentes, unidad y diccionario

La EMA aporta ocupación mensual por región. El RNT aporta capacidad registrada por establecimiento; se agrupa por región y se cruza con EMA mediante una relación muchos-a-uno. La capacidad no representa una medición mensual: sus valores regionales se repiten en cada mes para poder perfilar los registros y no deben sumarse a través del tiempo.

| Variable | Tipo en la base analítica | Significado | Fuente |
|---|---|---|---|
| `Año` | Entero | Año de la observación mensual | EMA, DANE |
| `Meses` | Texto categórico | Nombre del mes de la observación | EMA, DANE |
| `region` | Texto categórico | Región turística usada por EMA | EMA, DANE; homologación regional del proyecto |
| `ocupacion` | Numérico decimal (%) | Porcentaje de ocupación de habitaciones | EMA, DANE |
| `establecimientos` | Entero | Número de establecimientos RNT registrados en la región | RNT, extracción `thwd-ivmp.xlsx` |
| `habitaciones` | Entero | Habitaciones registradas en los establecimientos regionales | RNT, extracción `thwd-ivmp.xlsx` |
| `camas` | Entero | Camas registradas en los establecimientos regionales | RNT, extracción `thwd-ivmp.xlsx` |
| `empleados` | Entero | Personal reportado por los establecimientos regionales (`num_emp1`) | RNT, extracción `thwd-ivmp.xlsx` |
| `_merge` | Categórico de control | Indicador del resultado del cruce EMA-RNT; `both` significa coincidencia | Generado durante la integración |

El RNT se suma una vez por región a partir de sus registros de establecimiento. La extracción utilizada es de 2026 y no se mezcla con el CSV histórico de 2019.

## Integración y calidad de datos

La base integrada inicial contiene 1.188 filas: 1.092 observaciones mensuales y 96 filas de notas u otros contenidos que no son meses. Se conservan las 1.092 claves mensuales al filtrar y conciliar la base analítica. El cruce de EMA con el resumen regional RNT produce 1.092 coincidencias y ninguna región sin correspondencia.

| Control | Resultado | Tratamiento |
|---|---:|---|
| Claves `(Año, Meses, region)` duplicadas en la base mensual | 0 | No se requiere deduplicación |
| Ocupaciones fuera de 0–100% | 0 | Se mantiene la escala porcentual observada |
| Ocupaciones faltantes | 5 | Se conservan como nulos; no se imputan como cero |
| Filas del integrado que no son observaciones mensuales | 96 | Se excluyen al construir la base mensual |
| Filas mensuales con correspondencia EMA-RNT | 1.092 de 1.092 | Se valida el cruce muchos-a-uno |
| Valores de ocupación señalados por el criterio IQR | 20 | Se inspeccionan; el criterio estadístico no los convierte por sí solo en errores |

Los cinco nulos son ocupaciones de San Andrés entre abril y agosto de 2020. La inspección IQR de ocupación y capacidades RNT se incluye en la libreta. Las capacidades se diagnostican una vez por región, no sobre las filas mensuales repetidas. Los extremos que pasen los controles de dominio se conservan; no se eliminan por ser inusuales sin evidencia de error de origen.

## Análisis exploratorio

El histograma y el resumen descriptivo sitúan la ocupación típica alrededor de 43–45% (mediana 43,00%, media 44,74%). La media ligeramente mayor que la mediana y la cola superior hasta 78,14% muestran que hay observaciones altas que separan la distribución; no describen una ocupación uniforme entre regiones o meses. El diagrama de caja marca 20 observaciones por la regla IQR. Se concentran en 2020, durante la disrupción de la pandemia, por lo que se interpretan como eventos para contextualizar, no como errores automáticos.

Las barras por región muestran promedios más altos en San Andrés y Providencia (63,89%) y Cartagena (59,03%), y más bajos en Central (34,32%). En el gráfico mensual, noviembre promedia 50,34% y mayo 38,65%. La serie anual evidencia el mínimo de 2020 (26,27%), frente a 47,17% en 2019 y 53,01% en 2022; 2026 es parcial (enero-julio, 45,91%). Estos patrones describen diferencias y estacionalidad; no identifican sus causas.

## Preparación de variables

La función `preparar_features` deriva variables temporales y de ocupación, codifica categorías y escala variables numéricas de forma reproducible. Dos ejecuciones con los mismos datos producen el mismo resultado. La salida registrada en `03_turismo_colombia_modelado.xlsx` es de 1.092 filas y 38 columnas. Esta preparación se conserva; los cinco nulos de ocupación no se convierten artificialmente en valores observados.

## Segmentación no supervisada

Los métodos se aplican sobre variables escaladas. K-Means con `k=3` produce grupos de 433, 366 y 288 observaciones. Sus perfiles distinguen, respectivamente, baja ocupación y menor capacidad (38,11%; 934 habitaciones medias), mayor ocupación y capacidad intermedia (57,31%; 2.422 habitaciones), y ocupación también menor (38,74%) pero capacidad mayor (4.051 habitaciones). La ocupación sola no explica la separación: también interviene la capacidad regional.

El agrupamiento jerárquico de Ward con tres grupos obtiene tamaños 632, 273 y 182, con ocupaciones medias de 47,54%, 39,49% y 42,89%. Su cruce con K-Means muestra que las particiones no son equivalentes. DBSCAN con `eps=0,10` y `min_samples=5` detecta cuatro grupos y un registro de ruido; la prueba de `eps` varía desde 18 grupos/22 registros de ruido (`eps=0,05`) hasta tres grupos/sin ruido (`eps=0,15`). El único registro marcado como ruido es San Andrés, septiembre de 2020, con 0,8333% de ocupación. Es una observación que merece contexto, no una prueba de error.

Se adopta K-Means como segmentación principal para describir perfiles comparables y de lectura sencilla, no porque se haya probado una superioridad estadística frente a los otros métodos. La sensibilidad de DBSCAN a `eps` y las diferencias entre particiones se conservan como evidencia y limitación. Apriori no es pertinente: las fuentes no contienen transacciones ni canastas de compra.

## Clasificación de ocupación alta

El objetivo binario marca como alta la ocupación igual o superior a 60%. La partición estratificada, con semilla 42, separa 70% para entrenamiento y 30% para prueba: 760 y 327 registros. El baseline que predice siempre la clase mayoritaria alcanza 84,71% de exactitud. El árbol de decisión de profundidad 4 guardado en `05_turismo_colombia_prediccion.xlsx` obtiene 91,74% de exactitud, 76,74% de precisión, 66,00% de recall y 70,97% de F1 en prueba; la matriz TN/FP/FN/TP es 267/10/17/33.

En la prueba había 50 observaciones de ocupación alta. El árbol de profundidad 4 identificó 33 y omitió 17; por eso su exactitud global, favorecida por la clase mayoritaria, no basta para valorar su desempeño. La exploración de profundidades obtiene mejor F1 de prueba con profundidad 8 (0,78 frente a 0,71) y recall 0,78, pero amplía la brecha de exactitud entrenamiento-prueba (98,68% frente a 93,27%, diferencia de 5,41 puntos porcentuales). Profundidad 4 se mantiene como el modelo prescrito y guardado, no como el ganador de todas las métricas. Los resultados son predictivos y no implican causalidad.

## Pronóstico y alcance

La referencia trimestral repite la ocupación del mismo mes del año anterior. El backtest temporal de agosto-octubre de 2025 alcanza MAPE de 5,49% en 36 pares región-mes, por debajo del umbral de 15% en ese conjunto histórico. No garantiza desempeño futuro. En el seguimiento provisional del DANE para el Eje Cafetero, el MAPE parcial de agosto-septiembre de 2026 es 19,21%, por encima del umbral; todavía no permite evaluar el trimestre completo. La base principal termina en julio de 2026 y el anexo complementario solo aporta agosto y septiembre.

El sismo ocurrió durante agosto, que combina días previos y posteriores al evento. Los cambios EMA describen una asociación temporal, no demuestran causalidad. Las fuentes agregadas no permiten desagregar ocupación o ingresos mensuales por tipo de establecimiento ni por municipio, de modo que no sustentan recomendaciones específicas por tipo de alojamiento. La hoja de ingresos contiene variaciones porcentuales, no niveles monetarios.

## Reproducibilidad

Ejecutar en orden `Turismov1.2/Turismov1/notebooks/Mineria_Datos_Turismo_Colombia.ipynb`. La libreta valida las etapas guardadas `01`–`05`, la conciliación de claves, las métricas del conjunto de prueba y el seguimiento EMA. Las cifras de clasificación se leen de `05`; las evaluaciones de pronóstico y 2026 se calculan desde las fuentes correspondientes.
