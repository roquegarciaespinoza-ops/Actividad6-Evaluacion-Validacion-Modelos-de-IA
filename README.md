# Actividad 6 | Evaluación y validación de modelos de IA

**Módulo 2 · Semana 7 · Temas 13 y 14**

## Proyecto
**Predicción de incumplimiento de pago de tarjetas de crédito mediante aprendizaje automático.**

Este repositorio documenta la evaluación y validación de modelos de clasificación binaria sobre el dataset público *Default of Credit Card Clients* (UCI). El propósito es comparar modelos, ajustar umbrales y justificar decisiones técnicas en función de los errores y su posible impacto financiero.

> **Estado:** Fase 1 — diseño metodológico. Todavía no se han ejecutado modelos ni calculado métricas. Los objetivos numéricos son metas, no resultados.

## Pregunta de negocio
¿Podemos identificar anticipadamente a clientes con mayor probabilidad de incumplimiento y seleccionar un modelo con equilibrio entre desempeño, interpretabilidad y costos de error?

## Objetivo SMART
Durante la Actividad 6, desarrollar y evaluar al menos dos modelos supervisados de clasificación para predecir incumplimiento del siguiente mes, con validación cruzada estratificada de cinco particiones y prueba final independiente, buscando ROC-AUC >= 0.75 y una mejora del recall frente al baseline. La meta se revisará con evidencia empírica.

## Datos
- Fuente: [UCI — Default of Credit Card Clients](https://archive.ics.uci.edu/dataset/350/default+of+credit+card+clients).
- 30,000 registros, 23 variables predictoras y una variable objetivo binaria.
- **0:** sin incumplimiento; **1:** incumplimiento en el siguiente mes.
- No se utilizará el identificador como predictor.
- Los datos son históricos de Taiwán; no representan directamente la cartera actual de una institución mexicana.

## Metodología prevista
1. Inspeccionar calidad, clases, tipos y posibles fugas de información.
2. Separar 80% entrenamiento/desarrollo y 20% prueba final, estratificando y fijando semilla.
3. Entrenar DummyClassifier (baseline), regresión logística y Random Forest usando pipelines.
4. Validar con StratifiedKFold (5 folds); calcular accuracy, precision, recall, F1 y ROC-AUC.
5. Ajustar umbral únicamente sobre datos de desarrollo, evitando tocar el test.
6. Evaluar matriz de confusión y curva ROC en el conjunto de prueba.
7. Comparar alternativas A/B **offline** sobre los mismos casos; usar una prueba pareada adecuada (p. ej. McNemar para desacuerdos de clasificación), informando sus supuestos y limitaciones.
8. Examinar sensibilidad a costos hipotéticos de falsos positivos y falsos negativos.
9. Documentar resultados, significancia, limitaciones y decisión final.

**Importante:** una comparación offline no equivale a un experimento A/B aleatorizado en producción.

## Entregables en GitHub
- **Reporte técnico:** `reports/reporte_tecnico.md` (previsto, 6–8 páginas equivalentes).
- **Notebook/script:** `notebooks/` y `src/` (pendientes de Fase 2).
- **Tablero visual:** `dashboard/` (pendiente; se propone HTML/GitHub Pages).
- **Evidencias:** `results/` (métricas, matrices, gráficas y pruebas).

La consigna original solicita ZIP, PDF/Word y Trello; aquí se utilizará GitHub por decisión del equipo. La aceptación de formatos alternativos depende del docente.

## Documentación de la Fase 1
- [Problema y contexto](docs/01_definicion_problema.md)
- [Objetivo SMART](docs/02_objetivo_smart.md)
- [Dataset](docs/03_dataset.md)
- [Diseño técnico](docs/04_diseno_tecnico.md)

## Ejecución
La ejecución reproducible se documentará cuando exista el notebook funcional. No se publicarán métricas simuladas como resultados reales.

## Consideraciones éticas
El dataset contiene atributos demográficos: se analizarán sesgos, representatividad y consecuencias de decisiones automatizadas. Este proyecto es académico y **no** constituye un motor listo para aprobar o rechazar crédito.
