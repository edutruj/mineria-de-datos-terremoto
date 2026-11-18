# Lista de verificación: guías 1 a 6

Evidencia principal: `Turismov1.2/Turismov1/notebooks/Mineria_Datos_Turismo_Colombia.ipynb`. Los resultados se respaldan con las bases Excel versionadas y se resumen en [el informe técnico](INFORME_TECNICO.md).

| Guía | Requisito | Estado | Evidencia y observación |
|---|---|---|---|
| 1 | Importar y unir fuentes; describir las variables | Cumple | EMA mensual y resumen regional RNT: 1.092 coincidencias. La libreta y el informe incluyen nombre, tipo, significado y fuente de las variables. |
| 2 | Limpiar y justificar controles de calidad | Cumple | Se identifican 96 filas no mensuales, 5 ocupaciones nulas, cero claves duplicadas y cero ocupaciones fuera de 0–100%. Se diagnostican valores IQR de ocupación y capacidades RNT sin borrarlos automáticamente. |
| 3 | Explorar y explicar las gráficas y observaciones inusuales | Cumple | Resumen estadístico, histograma, caja, barras y serie temporal. La interpretación en lenguaje natural explica las diferencias regionales, temporales y los 20 atípicos de ocupación concentrados en 2020. |
| 4 | Construir variables, codificar y escalar de forma reproducible | Cumple | `preparar_features` contiene variables derivadas, escalado/codificación y prueba de repetibilidad exacta; se conserva la salida de 1.092 × 38. |
| 5 | Aplicar y comparar métodos de segmentación | Cumple | K-Means (`k=3`), jerárquico de Ward y DBSCAN, perfiles, cruce de particiones, prueba de `eps` y análisis del ruido. K-Means se elige por interpretabilidad, no por una superioridad no demostrada. Apriori no aplica sin transacciones. |
| 6 | Entrenar, evaluar e interpretar una clasificación | Cumple | Objetivo `ocupacion_alta` (≥60%), split estratificado 70/30 con semilla 42, baseline, árbol de profundidad 4, matriz, precisión, recall, F1, brecha entrenamiento-prueba y comparación de profundidades. Se aclara que profundidad 8 logra mejor F1 exploratorio. |
| Entregables | Presentar documentación técnica y lista de cumplimiento | Cumple | Este checklist y `INFORME_TECNICO.md` acompañan la libreta y README. |

## Límites que deben mantenerse en la presentación

- El MAPE de 5,49% corresponde solo al backtest estacional de agosto-octubre de 2025 (36 pares región-mes); no prueba precisión para el futuro.
- La comparación EMA provisional de agosto-septiembre de 2026 arroja MAPE parcial de 19,21%; falta octubre para medir el trimestre completo. Por tanto, no se afirma que el criterio de 15% se cumpla para 2026.
- Las diferencias de ocupación e ingresos de 2026 no prueban un efecto causal del sismo.
- EMA y RNT están agregados por región y no habilitan recomendaciones de ocupación por tipo de establecimiento. Ingresos es una tasa porcentual anual, no un monto.
- El árbol guardado tiene profundidad 4 según el modelo prescrito; profundidad 8 logra mayor F1 de prueba, con mayor brecha entrenamiento-prueba. No se describe profundidad 4 como la mejor puntuación.
