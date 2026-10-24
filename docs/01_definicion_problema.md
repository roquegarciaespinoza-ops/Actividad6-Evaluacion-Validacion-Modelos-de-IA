# 01. Definición del problema y contexto

## Contexto
Una institución financiera desea anticipar el incumplimiento del siguiente periodo de pago para orientar estrategias de seguimiento preventivo. Se estudia un caso académico con datos históricos públicos; no se usan registros internos ni información de clientes actuales.

## Problema
Clasificar incorrectamente a un cliente puede tener costos diferentes:
- **Falso negativo (FN):** se predice cumplimiento, pero ocurre incumplimiento. Puede aumentar la exposición a pérdidas.
- **Falso positivo (FP):** se predice incumplimiento, pero el cliente cumple. Puede ocasionar intervenciones innecesarias y trato injustificado.

## Pregunta
¿Qué modelo y qué umbral permiten identificar mejor el riesgo de incumplimiento sin generar una cantidad desproporcionada de falsas alarmas?

## Alcance
Clasificación binaria del incumplimiento del siguiente mes. Se compararán modelos supervisados con datos tabulares y se explicarán métricas, generalización, significancia estadística y costos hipotéticos.

## Fuera de alcance
Decisiones reales de otorgamiento de crédito, inferencias causales, cálculo de pérdidas reales y despliegue productivo.

## Criterios de éxito
Desempeño contrastado con baseline, métricas completas, validación cruzada, conjunto de prueba independiente, ajuste de umbral, comparación A/B offline y justificación técnica y financiera.
