1. Descripción de la Técnica Utilizada y Justificación
El método de aprendizaje no supervisado aplicado es Detección de Anomalías (Outlier Detection) utilizando el algoritmo Isolation Forest (Bosque de Aislamiento).

Justificación de la Elección:
Isolation Forest es una técnica robusta y eficiente para problemas de scoring crediticio, ya que se basa en aislar las observaciones. Las anomalías (clientes atípicos) se definen por ser escasas y diferentes, por lo que requieren menos particiones en un árbol aleatorio para ser separadas del resto de los datos.

Este método se justifica porque buscamos identificar clientes cuyas características (ingresos, monto de crédito, scores externos) son extremadamente inusuales en comparación con la población general. Si estos clientes atípicos presentan una tasa de morosidad significativamente diferente, la información puede complementar o robustecer el modelo supervisado principal, tal como se requiere en el objetivo de la EA3.

El análisis se realizó exclusivamente sobre el conjunto de entrenamiento (application_.parquet), evitando cualquier data leakage.

2. Instrucciones de Ejecución Claras del Código
El código está contenido en el archivo EA3_Unsupervised_Diaz_Barrios (1).ipynb.

Para ejecutar y replicar el análisis:

Clonar el Repositorio: Clona o descarga el repositorio de GitHub que contiene este archivo (README_UNSUPERVISED.md) y el notebook (.ipynb).

Abrir en Colab: Abre el archivo EA3_Unsupervised_Diaz_Barrios (1).ipynb directamente en Google Colab.

Subir Datos: Sube el archivo de datos application_.parquet al entorno de la sesión de Colab (utilizando el ícono de carpeta en el panel izquierdo).

Ejecutar: Ejecuta todas las celdas del notebook secuencialmente. El proceso cargará los datos, aplicará el preprocesamiento (imputación y escalado) y entrenará el modelo Isolation Forest para generar las predicciones de anomalía y el score de riesgo asociado.

3. Análisis e Interpretación de ResultadosEl modelo Isolation Forest se entrenó con una tasa de contaminación del 1%, identificando aproximadamente a 2,900 clientes como anomalías.La evaluación se realizó comparando las tasas de morosidad (TARGET=1) entre la población normal y la anómala:SegmentoClasificación (IF_Anomaly)Tasa de Morosidad (TARGET=1)Población Normal18.07%Anomalías (Outliers)-18.55%Interpretación Clave:El segmento de Anomalías presenta una tasa de morosidad del 8.55%, lo cual es ligeramente superior a la tasa promedio de la Población Normal ($8.07\%$).Esta diferencia, aunque pequeña, es técnicamente significativa: la atipicidad estadística (ser un outlier en el espacio multidimensional de las variables crediticias) se correlaciona con un riesgo de incumplimiento marginalmente mayor.La visualización confirma que las anomalías suelen ser clientes en los extremos de la distribución (ej., ingresos extremadamente altos con montos de crédito inusuales, o combinaciones de scores externos muy dispersas).

4.  Discusión sobre si el Método puede Incorporarse al Proyecto Final
Sí, el método aplicado debe incorporarse al proyecto final.

La técnica de Detección de Anomalías aporta un valor estratégico, cumpliendo con el objetivo de robustecer el modelo supervisado:

Aporte del Score como Feature: El puntaje de anomalía (IF_Anomaly_Score) puede ser agregado como una nueva característica numérica al conjunto de datos utilizado por el modelo de scoring principal (ej., Gradient Boosting o Regresión Logística). Al entrenarse, el modelo supervisado aprenderá a asignar un mayor peso de riesgo a los clientes con un score de anomalía bajo (más atípicos).

Mejora de Robustez: Esto mejora la robustez del modelo, ya que le permite hacer mejores predicciones en las colas de la distribución, donde la densidad de datos es baja y la incertidumbre es alta.

Segmentación de Riesgo Atípico: La clasificación de anomalía (IF_Anomaly = -1) puede ser utilizada como una bandera de riesgo especial. Para estos clientes, la institución financiera podría implementar un protocolo de revisión manual por un analista o aplicar políticas de crédito más conservadoras, independientemente de la puntuación de riesgo que arroje el modelo supervisado.
