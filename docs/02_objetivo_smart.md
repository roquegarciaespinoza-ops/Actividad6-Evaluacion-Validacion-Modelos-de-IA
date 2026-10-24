# 02. Objetivo SMART

> Desarrollar y evaluar durante la Actividad 6 al menos dos modelos supervisados de clasificación para predecir incumplimiento de pago de tarjetas de crédito, utilizando un conjunto público, validación cruzada estratificada de cinco particiones y un conjunto de prueba independiente; se buscará ROC-AUC >= 0.75 y mejorar recall frente al baseline, documentando métricas, costos de error y significancia de las diferencias.

| Dimensión | Definición |
| --- | --- |
| Específico | Predecir incumplimiento del siguiente mes. |
| Medible | ROC-AUC objetivo >= 0.75; recall mejor que baseline; reporte de accuracy, precision, recall, F1 y matriz de confusión. |
| Alcanzable | Datos públicos y modelos de ML tabular con herramientas abiertas. |
| Relevante | Prevención de riesgo y mejor asignación de acciones de seguimiento. |
| Temporal | Dentro de la Actividad 6 del Módulo 2, Semana 7. |

**Nota:** los umbrales son metas preliminares, no métricas alcanzadas. El recall por sí solo no basta: se vigilarán precision, falsos positivos y costos estimados. El baseline DummyClassifier puede tener recall nulo según su estrategia; también se comparará con regresión logística.
