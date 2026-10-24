# 03. Dataset y gobierno de datos

## Fuente
[UCI Machine Learning Repository — Default of Credit Card Clients](https://archive.ics.uci.edu/dataset/350/default+of+credit+card+clients).

## Características publicadas
- 30,000 observaciones.
- 23 variables predictoras, además de la variable objetivo binaria.
- Información de límite de crédito, historial de pagos, facturación, pagos y atributos demográficos codificados.
- Objetivo: incumplimiento en el siguiente mes (1 = sí; 0 = no).

## Preparación prevista
1. Descargar desde la fuente original y registrar URL, fecha y versión disponible.
2. Verificar dimensiones, duplicados, valores faltantes, clases y tipos.
3. Excluir el identificador ID de las variables predictoras.
4. Examinar codificaciones inusuales de variables categóricas y documentar tratamiento.
5. Separar entrenamiento y prueba con estratificación y semilla fija antes de seleccionar modelos o umbrales.
6. Encapsular imputación, escalamiento y codificación, según el modelo, dentro de pipelines entrenados exclusivamente con cada fold.

## Riesgos y limitaciones
Los datos son históricos y corresponden a Taiwán; su generalización a México o a otra época no está demostrada. Los atributos demográficos pueden introducir disparidades: se evaluarán posibles sesgos sin atribuir causalidad. No se publicarán datos confidenciales.

## Reproducibilidad
Se documentarán dependencias, semilla, particiones, transformaciones y resultados generados por código. No se incluirán archivos binarios grandes sin necesidad.
