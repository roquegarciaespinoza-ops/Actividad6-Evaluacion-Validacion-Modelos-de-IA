# Fase 3 — Diseño de evaluación estadística y financiera

## Base empírica: Fase 2
Según el artefacto `fase2-evidencias.zip` aportado para esta actividad, en el conjunto de prueba de 6,000 casos se obtuvo:

| Modelo | Accuracy | Precision | Recall | F1 | ROC-AUC |
|---|---:|---:|---:|---:|---:|
| Baseline | 0.778833 | 0 | 0 | 0 | 0.500000 |
| Regresión logística | 0.807667 | 0.686825 | 0.239638 | 0.355307 | 0.707636 |
| Random Forest | 0.816667 | 0.656121 | 0.359457 | 0.464460 | 0.765564 |

**Importante:** estos resultados son de Fase 2; no se reutilizan las etiquetas de test para seleccionar el umbral en Fase 3.

## Metodología de Fase 3
1. Reproducir el mismo dataset, split 80/20, semilla 42, preprocesamiento y modelos de Fase 2.
2. Generar probabilidades fuera de fold (OOF) mediante validación cruzada estratificada de 5 particiones en el 80% de desarrollo.
3. Examinar umbrales de 0.10 a 0.90 en incrementos de 0.05, comparando TP, FP, FN, TN, precision, recall y F1.
4. Evaluar costos **hipotéticos**: FN = 5,000 unidades monetarias; FP = 300. Minimizar costo total sobre desarrollo y desempatar por F1.
5. Ejecutar una comparación A/B **offline** a umbral fijo 0.5, con las mismas observaciones OOF para regresión logística (A) y Random Forest (B). Aplicar McNemar exacto (binomial bilateral) a los desacuerdos en aciertos, reportando p-valor con alfa 0.05. Esto **no** constituye experimento A/B aleatorizado.
6. Análisis de sensibilidad con FP = 100, 300, 600, 1,000 y 2,000, manteniendo FN = 5,000.
7. Entrenar el modelo seleccionado sobre desarrollo y evaluar **una sola vez** sobre el 20% reservado.

## Evidencias esperadas (generadas al ejecutar)
- `results/fase3/umbrales_desarrollo.csv`
- `results/fase3/comparacion_ab_mcnemar.csv`
- `results/fase3/sensibilidad_costos.csv`
- `results/fase3/evaluacion_final_test.csv`
- `results/fase3/costos_por_umbral.png`

## Precauciones
- Los costos son escenarios didácticos, **no pérdidas estimadas** de un banco.
- McNemar contrasta tasa de error pareada a umbral 0.5, no diferencias de AUC, utilidad ni impacto causal.
- Las probabilidades OOF se usan para elegir el umbral; se requiere validación externa antes de cualquier uso real.
- No se atribuye significancia estadística hasta contar con el resultado ejecutado.
- No declarar la Fase 3 concluida hasta que GitHub Actions termine en verde y los artefactos sean revisados.
