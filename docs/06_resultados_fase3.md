# Resultados verificados de Fase 3

Fuente: artefacto `fase3-evidencias.zip` de GitHub Actions, ejecución exitosa #2. Valores tomados de los CSV ejecutados, no simulados.

## Selección en desarrollo
- Modelo: **Random Forest**.
- Umbral: **0.10**.
- Criterio: minimizar costo hipotético FN × 5,000 + FP × 300 con predicciones out-of-fold de desarrollo.
- Costo en desarrollo: **6,156,600 unidades monetarias** (24,000 observaciones).

## Evaluación final en prueba reservada (6,000 observaciones)
| Métrica | Resultado |
|---|---:|
| TN | 1,577 |
| FP | 3,096 |
| FN | 133 |
| TP | 1,194 |
| Accuracy | 0.461833 |
| Precision | 0.278322 |
| Recall | 0.899774 |
| F1 | 0.425138 |
| ROC-AUC | 0.765564 |
| Costo hipotético | 1,593,800 |

El costo hipotético en prueba es 133 × 5,000 + 3,096 × 300 = **1,593,800** unidades monetarias. No son pérdidas reales.

## Comparación con Fase 2 (Random Forest, umbral 0.50)
| Métrica | Fase 2, umbral 0.50 | Fase 3, umbral 0.10 |
|---|---:|---:|
| TP | 477 | 1,194 |
| FN | 850 | 133 |
| FP | 250 | 3,096 |
| TN | 4,423 | 1,577 |
| Recall | 35.95% | 89.98% |
| Precision | 65.61% | 27.83% |

Bajo los mismos costos hipotéticos, Fase 2 tendría costo 850×5,000 + 250×300 = **4,325,000**; el umbral seleccionado reduce ese costo hipotético a **1,593,800**, una diferencia de **2,731,200** (63.15%). Esta comparación es ilustrativa y no una estimación de ahorro real.

## Prueba A/B offline (McNemar exacto, umbral 0.50, desarrollo OOF)
- A: Regresión logística; B: Random Forest.
- A acierta / B falla: 598.
- A falla / B acierta: 816.
- Discordancias: 1,414.
- Valor p bilateral: **7.387604 × 10⁻⁹**; alfa = 0.05.
- Conclusión: evidencia estadística de distinta tasa de acierto pareada, favorable a B. No demuestra causalidad ni una diferencia estadística en ROC-AUC.

## Sensibilidad al costo de falsos positivos (desarrollo)
| Costo FP | Costo FN | Modelo elegido | Umbral |
|---:|---:|---|---:|
| 100 | 5,000 | Random Forest | 0.10 |
| 300 | 5,000 | Random Forest | 0.10 |
| 600 | 5,000 | Random Forest | 0.10 |
| 1,000 | 5,000 | Random Forest | 0.20 |
| 2,000 | 5,000 | Random Forest | 0.30 |

## Limitaciones
El costo es hipotético; el dataset es histórico de Taiwán y no se puede trasladar sin validación a una cartera mexicana. La caída de precisión y accuracy al priorizar recall implica una carga operativa elevada. La elección del umbral depende de los costos asumidos y no implica aprobación para producción.
