# Actividad 6 — Reporte técnico integral de evaluación y validación de IA

**Caso:** predicción de incumplimiento de pago mediante clasificación binaria. **Fuente:** UCI Default of Credit Card Clients (ID 350). **Alcance:** ejercicio académico, no sistema de aprobación crediticia.

## 1. Resumen ejecutivo
Se evaluaron un baseline de clase mayoritaria, regresión logística y Random Forest. En la evaluación inicial, Random Forest obtuvo ROC-AUC **0.7656**, por encima de la meta SMART de 0.75. El ajuste de umbral, elegido **solo con predicciones fuera de fold de desarrollo**, seleccionó Random Forest con umbral **0.10**. En 6,000 casos de prueba logró recall **89.98 %** y precision **27.83 %**. Su costo de errores bajo los supuestos didácticos fue **1,593,800 unidades monetarias**. El aumento de alertas falsas impide recomendar su despliegue sin más validación.

## 2. Problema, contexto y objetivo SMART
La clase positiva (1) significa incumplimiento; la negativa (0), cumplimiento. Un falso negativo (FN) es un incumplimiento no identificado y un falso positivo (FP) es una alerta sobre un cliente cumplidor. **Objetivo SMART:** al finalizar el experimento, evaluar tres modelos con partición estratificada 80/20 y cinco folds, reportar accuracy, precision, recall, F1, matriz y ROC-AUC, buscar AUC ≥ 0.75 en el conjunto reservado y justificar el umbral con costos explícitos. **Meta alcanzada:** Random Forest AUC 0.7656. No acredita desempeño en una cartera mexicana.

## 3. Dataset y protocolo experimental
- **Datos:** 30,000 registros, 23 predictores tras excluir ID; 23,364 cumplidores (77.88 %) y 6,636 incumplidores (22.12 %).
- **Auditoría:** sin nulos; 56 patrones repetidos de predictores (no equivalen necesariamente a registros completos duplicados).
- **Partición:** 24,000 observaciones de desarrollo y 6,000 de prueba; estratificación y semilla 42.
- **Pipeline:** imputación, escalado numérico y codificación one-hot de SEX, EDUCATION y MARRIAGE. Preprocesamiento entrenado dentro de cada fold para evitar fuga.
- **Validación:** StratifiedKFold de 5 particiones; en Fase 3 se generaron probabilidades *out-of-fold* (OOF) para elegir umbral sin consultar test.

## 4. Baseline y comparación inicial (Fase 2, umbral 0.50)
| Modelo | Accuracy | Precision | Recall | F1 | ROC-AUC |
|---|---:|---:|---:|---:|---:|
| Baseline | 77.88 % | 0 % | 0 % | 0 % | 0.5000 |
| Regresión logística | 80.77 % | 68.68 % | 23.96 % | 35.53 % | 0.7076 |
| Random Forest | **81.67 %** | 65.61 % | **35.95 %** | **46.45 %** | **0.7656** |

La exactitud del baseline es engañosa: siempre predice la clase mayoritaria y no detecta incumplimientos. **Recall** mide qué proporción de incumplimientos se detecta; **precision**, qué fracción de alertas es correcta; **F1** equilibra ambas; **ROC** relaciona sensibilidad y tasa de falsos positivos para distintos umbrales; **AUC** resume discriminación, no rentabilidad ni calibración. La validación cruzada de Random Forest registró AUC media **0.7758** y desviación estándar **0.0043** en desarrollo.

## 5. Umbral seleccionado y matriz de confusión
El barrido 0.10–0.90 en pasos de 0.05 minimizó costo hipotético con predicciones OOF. Se seleccionó **Random Forest, umbral 0.10**; luego se entrenó sobre desarrollo y se evaluó una sola vez en prueba.

| Real / Predicho | Cumple (0) | Incumple (1) |
|---|---:|---:|
| Cumple (0) | TN = **1,577** | FP = **3,096** |
| Incumple (1) | FN = **133** | TP = **1,194** |

**Métricas finales:** accuracy **46.18 %**, precision **27.83 %**, recall **89.98 %**, F1 **42.51 %**, ROC-AUC **0.7656**. Con umbral 0.50 Random Forest tenía TN 4,423, FP 250, FN 850 y TP 477. Al bajar a 0.10 detecta **717** incumplimientos adicionales, pero genera **2,846** falsas alarmas adicionales. Aproximadamente 72 de cada 100 alertas no son incumplimientos. AUC no cambia porque las probabilidades/ranking son las mismas.

## 6. Comparación A/B offline y significancia
En desarrollo, con umbral fijo 0.50, se compararon sobre los mismos casos OOF: A = regresión logística y B = Random Forest. A acierta y B falla **598**; A falla y B acierta **816**; discordancias **1,414**. Prueba exacta de McNemar bilateral: **p = 7.3876 × 10⁻⁹**, alfa 0.05. Se rechaza igualdad de tasas de acierto pareadas en este contraste, favorable a B. **No** es un A/B aleatorizado, no demuestra causalidad ni diferencia estadística de AUC. El contraste se hizo a umbral 0.50, no al umbral final 0.10.

## 7. Análisis financiero hipotético
Supuestos docentes: costo FN = **5,000**, costo FP = **300** unidades monetarias. Fórmula: **costo = 5,000 × FN + 300 × FP**.

| Random Forest (prueba) | FN | FP | Costo FN | Costo FP | Costo total |
|---|---:|---:|---:|---:|---:|
| Umbral 0.50 | 850 | 250 | 4,250,000 | 75,000 | **4,325,000** |
| Umbral 0.10 | 133 | 3,096 | 665,000 | 928,800 | **1,593,800** |

Diferencia hipotética **2,731,200 (63.15 %)**. **No es ahorro real, ROI ni estimación causal**. Faltan costos de operación, capacidad de revisión, recuperaciones, distribución futura y análisis regulatorio.

### Sensibilidad (desarrollo OOF)
| Costo FP | Costo FN | Modelo elegido | Umbral |
|---:|---:|---|---:|
| 100 | 5,000 | Random Forest | 0.10 |
| 300 | 5,000 | Random Forest | 0.10 |
| 600 | 5,000 | Random Forest | 0.10 |
| 1,000 | 5,000 | Random Forest | 0.20 |
| 2,000 | 5,000 | Random Forest | 0.30 |

Al aumentar el costo de las alertas falsas, aumenta el umbral seleccionado. El criterio de negocio determina la política óptima.

## 8. Decisión técnica y riesgos
**Selección académica:** Random Forest a 0.10 minimiza el costo bajo los supuestos definidos, supera la meta AUC y detecta casi 90 % de incumplimientos. **No desplegar automáticamente**: se necesitan validación temporal/externa (los datos son históricos de Taiwán), evaluación de sesgos y equidad, calibración, privacidad, costos reales, capacidad operativa para 3,096 falsas alertas, supervisión humana y monitoreo de deriva.

## 9. Conclusiones
1. Accuracy sola oculta fallas graves ante clases desbalanceadas.
2. Random Forest supera al baseline y a regresión logística en AUC y F1 de la evaluación inicial.
3. McNemar detecta diferencia significativa en aciertos pareados a umbral 0.50, pero no reemplaza un experimento aleatorizado.
4. El umbral 0.10 incrementa recall, sacrifica precision y reduce el costo **hipotético** bajo el escenario definido.
5. El modelo queda validado para fines académicos, **no** autorizado para uso crediticio real.

## 10. Evidencia reproducible y tablero
- [Notebook Fase 2](../notebooks/actividad6_fase2.ipynb) · [Notebook Fase 3](../notebooks/actividad6_fase3.ipynb)
- [Workflow Fase 2](../.github/workflows/validar-fase2.yml) · [Workflow Fase 3](../.github/workflows/validar-fase3.yml)
- [Tablero visual de resultados](../dashboard/index.html) · [Resultados documentados Fase 3](06_resultados_fase3.md)
- CSV y gráfica de Fase 3: artefacto `fase3-evidencias` de GitHub Actions, ejecución exitosa #2.

## 11. Cobertura de rúbrica
| Criterio (25 puntos) | Evidencia |
|---|---|
| Definición, matriz y métricas | Secciones 2, 4, 5 |
| Evaluación, ROC/AUC y umbral | Secciones 4, 5; notebooks |
| Validación, baseline, significancia y A/B | Secciones 3, 4, 6 |
| Integración, justificación e impacto | Secciones 7–10 y tablero |

**Nota de formato:** el enunciado menciona PDF/Word y tablero Trello; la entrega acordada es GitHub (Markdown y HTML). Si el docente exige literalmente esos formatos, será necesaria su aprobación para el equivalente en GitHub.
