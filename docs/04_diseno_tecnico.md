# 04. Diseño técnico y plan de evaluación

## Arquitectura
Dataset UCI → auditoría y limpieza → división estratificada 80/20 → pipelines → validación cruzada en desarrollo → selección de modelo y umbral → prueba final independiente → reporte y dashboard.

## Modelos
- **DummyClassifier:** baseline ingenuo.
- **Regresión logística:** candidato interpretable.
- **Random Forest:** candidato no lineal.

## Métricas
Accuracy, precision, recall y F1 para la clase positiva; matriz de confusión con TN, FP, FN, TP; ROC y ROC-AUC usando probabilidades o scores. Se reportará el soporte de cada clase y, cuando proceda, intervalos de incertidumbre.

## Validación
- Separación estratificada: 80% desarrollo / 20% test.
- StratifiedKFold de 5 particiones en desarrollo.
- Pipelines para prevenir fuga de datos.
- Selección de hiperparámetros y umbral con datos de desarrollo, idealmente predicciones fuera de fold o validación interna.
- Test final reservado para evaluación única.

## Umbral
Probar umbrales sobre probabilidades y construir tabla de trade-offs de FP, FN, precision, recall, F1 y costo esperado. Seleccionar sin optimizar contra el conjunto de prueba.

## A/B y significancia
Comparación **offline** de modelos A y B sobre las mismas observaciones. Para diferencias de aciertos/errores binarios se considerará McNemar; se informará estadístico, p-valor, tamaño del efecto y limitaciones. La selección basada en validación debe anteceder a la evaluación final. No se afirmará que es un experimento aleatorizado con clientes.

## Impacto financiero
Escenarios explícitamente hipotéticos de costo por FP y FN. Análisis de sensibilidad para evitar que una conclusión dependa de un único supuesto inventado.

## Tecnologías
Python, pandas, numpy, scikit-learn, scipy, statsmodels, matplotlib, seaborn, Jupyter/Colab, GitHub. El dashboard se prevé en HTML estático compatible con GitHub Pages.

## Criterios de aceptación
- Problema y objetivo SMART.
- Matriz de confusión interpretada.
- Accuracy, precision, recall, F1.
- Ajuste y justificación de umbral.
- Curva ROC y AUC.
- Validación cruzada y dispersión entre folds.
- Comparación contra baseline.
- Comparación A/B y prueba estadística.
- Relación técnica–impacto financiero.
- Reporte, notebook/script y dashboard.
- Evidencia reproducible en GitHub.

## Estado
Fase 1 documentada. Experimentos, métricas, pruebas y conclusiones pendientes.
