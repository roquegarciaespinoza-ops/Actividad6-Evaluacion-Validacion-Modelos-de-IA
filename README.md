# Actividad 6 | Evaluación y validación de modelos de inteligencia artificial

**Módulo 2 · Semana 7 · Temas 13 y 14**  
**Caso de estudio:** predicción de incumplimiento de pago de tarjetas de crédito (clasificación binaria).  
**Estado:** experimentación completada, evidencias documentadas y tablero publicado en GitHub Pages.

> [**📊 Abrir tablero visual de resultados**](https://roquegarciaespinoza-ops.github.io/Actividad6-Evaluacion-Validacion-Modelos-de-IA/) · [**📄 Leer reporte técnico integral**](docs/07_reporte_tecnico_integral.md)

## 1. Problema de negocio y objetivo SMART

Identificar anticipadamente posibles incumplimientos de pago y seleccionar un modelo que equilibre desempeño predictivo y costos de error. **Clase 1:** incumplimiento; **clase 0:** cumplimiento.

**Objetivo SMART:** durante la Actividad 6, evaluar al menos dos modelos supervisados, utilizando validación cruzada estratificada de cinco particiones y prueba independiente; reportar matriz de confusión, accuracy, precision, recall, F1 y ROC-AUC; buscar ROC-AUC ≥ 0.75 y mejorar la detección de incumplimientos frente al baseline. La meta ROC-AUC se alcanzó con Random Forest (**0.7656**).

## 2. Dataset y metodología

- **Fuente:** [UCI — Default of Credit Card Clients (ID 350)](https://archive.ics.uci.edu/dataset/350/default+of+credit+card+clients).
- **Tamaño:** 30,000 registros; 23 predictores y objetivo binario. Se excluye el identificador.
- **Partición:** desarrollo 80 % (24,000) y prueba reservada 20 % (6,000), estratificada; semilla 42.
- **Modelos:** DummyClassifier (baseline), regresión logística y Random Forest.
- **Validación:** StratifiedKFold (5 folds), pipelines de preprocesamiento, comparación A/B offline pareada, prueba exacta de McNemar, ajuste de umbral con predicciones out-of-fold (OOF), y análisis de sensibilidad a costos.
- **Control metodológico:** el umbral se selecciona con datos de desarrollo; el conjunto de prueba se utiliza para evaluación final, no para optimización.

## 3. Resultados principales

### Comparación inicial — umbral 0.50

| Modelo | Accuracy | Precision | Recall | F1 | ROC-AUC |
|---|---:|---:|---:|---:|---:|
| Baseline | 77.88 % | 0 % | 0 % | 0 % | 0.5000 |
| Regresión logística | 80.77 % | 68.68 % | 23.96 % | 35.53 % | 0.7076 |
| **Random Forest** | **81.67 %** | 65.61 % | **35.95 %** | **46.45 %** | **0.7656** |

### Modelo seleccionado — Random Forest, umbral 0.10

| Métrica en prueba | Resultado |
|---|---:|
| Accuracy | 46.18 % |
| Precision | 27.83 % |
| Recall | **89.98 %** |
| F1 | 42.51 % |
| ROC-AUC | **0.7656** |
| Verdaderos negativos (TN) | 1,577 |
| Falsos positivos (FP) | 3,096 |
| Falsos negativos (FN) | 133 |
| Verdaderos positivos (TP) | 1,194 |

El umbral 0.10 incrementa la detección de incumplimientos, pero genera **3,096 falsas alarmas** sobre 6,000 casos. Por ello, la elección se justifica solo para el escenario hipotético estudiado y no implica aprobación para producción.

### A/B offline, significancia y costos

- **McNemar exacto, umbral 0.50:** A acierta/B falla = 598; A falla/B acierta = 816; **p = 7.39 × 10⁻⁹**. Se observa diferencia significativa de aciertos pareados, favorable a Random Forest. No es una prueba A/B aleatorizada ni una prueba de significancia de AUC.
- **Costos hipotéticos:** falso negativo = 5,000; falso positivo = 300 unidades monetarias.
- **Costo en prueba, umbral 0.50:** 4,325,000.
- **Costo en prueba, umbral 0.10:** 1,593,800.
- **Diferencia hipotética:** 2,731,200 (63.15 %). **No equivale a ahorro financiero real ni ROI**.

## 4. Entregables oficiales

| Entregable | Acceso |
|---|---|
| **Reporte técnico integral** | [Abrir reporte](docs/07_reporte_tecnico_integral.md) |
| **Notebook Fase 2 — evaluación inicial** | [Abrir notebook](notebooks/actividad6_fase2.ipynb) |
| **Notebook Fase 3 — umbral, A/B, significancia y costos** | [Abrir notebook](notebooks/actividad6_fase3.ipynb) |
| **Tablero visual publicado** | [Abrir GitHub Pages](https://roquegarciaespinoza-ops.github.io/Actividad6-Evaluacion-Validacion-Modelos-de-IA/) |
| **Resultados y conclusiones de Fase 3** | [Abrir documento](docs/06_resultados_fase3.md) |
| **Datos estructurados de resultados** | [Abrir carpeta](results/fase3/) |
| **Evidencia de ejecución Fase 2** | [GitHub Actions](actions/workflows/validar-fase2.yml) |
| **Evidencia de ejecución Fase 3** | [GitHub Actions](actions/workflows/validar-fase3.yml) |
| **Publicación del tablero** | [GitHub Actions](actions/workflows/publicar-dashboard.yml) |

**Reproducibilidad:** ejecutar los notebooks en orden (Fase 2 y Fase 3) con las dependencias de [requirements.txt](requirements.txt). Los workflows de GitHub Actions ejecutan notebooks y conservan artefactos de evidencia.

## 5. Fases del proyecto

1. **Diseño metodológico:** [problema](docs/01_definicion_problema.md), [objetivo SMART](docs/02_objetivo_smart.md), [dataset](docs/03_dataset.md) y [diseño técnico](docs/04_diseno_tecnico.md).
2. **Evaluación de modelos:** descarga de UCI, auditoría, baseline, regresión logística, Random Forest, matrices y ROC/AUC.
3. **Validación estadística y financiera:** selección de umbral en desarrollo, A/B offline, McNemar y escenarios de costo.
4. **Integración y presentación:** reporte técnico, resultados tabulados y tablero visual en GitHub Pages.

## 6. Correspondencia con la rúbrica

| Criterio (25 puntos cada uno) | Evidencia |
|---|---|
| Definición del problema, métricas y matriz | [Reporte §§ 2, 4 y 5](docs/07_reporte_tecnico_integral.md) |
| Evaluación de modelos, ROC/AUC y umbral | [Reporte §§ 4 y 5](docs/07_reporte_tecnico_integral.md), notebooks |
| Validación cruzada, baseline, A/B y significancia | [Reporte §§ 3, 4 y 6](docs/07_reporte_tecnico_integral.md) |
| Justificación técnica, impacto y comunicación | [Reporte §§ 7–10](docs/07_reporte_tecnico_integral.md) y [tablero](https://roquegarciaespinoza-ops.github.io/Actividad6-Evaluacion-Validacion-Modelos-de-IA/) |

## 7. Limitaciones y consideraciones éticas

El dataset es histórico de Taiwán y **no representa directamente una cartera mexicana**. Los costos son supuestos docentes. Antes de una aplicación real se requieren validación temporal y externa, evaluación de sesgos y equidad, privacidad, calibración, estimación de costos reales, revisión humana y monitoreo.

**Formato de entrega:** por decisión del equipo, los entregables se presentan en GitHub (Markdown, notebooks, CSV y GitHub Pages). La consigna original también solicita ZIP, reporte PDF/Word y tablero Trello; **la aceptación de los formatos alternativos depende del docente**.

---

**Enlace único para compartir la actividad:** https://github.com/roquegarciaespinoza-ops/Actividad6-Evaluacion-Validacion-Modelos-de-IA
