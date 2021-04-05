# Visualize Sweep Results

## Gráfico de coordenadas paralelas

![](https://paper-attachments.dropbox.com/s_194708415DEC35F74A7691FF6810D3B14703D1EFE1672ED29000BA98171242A5_1578695138341_image.png)

 Los gráficos de coordenadas paralelas mapean los valores de los hiperparámetros a las métricas del modelo. Son útiles para pulir sobre las combinaciones de los hiperparámetros \_\*\*\_ que condujeron al mejor desempeño del modelo.

### Gráfico de la Importancia de los Hipreparámetros

![](https://paper-attachments.dropbox.com/s_194708415DEC35F74A7691FF6810D3B14703D1EFE1672ED29000BA98171242A5_1578695757573_image.png)

El gráfico de la importancia de los hiperparámetros surge a partir de los parámetros que fueron los mejores indicadores, y está altamente correlacionado con los valores deseables para tus métricas.

**Correlación** es una correlación lineal entre el hiperparámetro y la métrica elegida \(en este caso val\_loss\). Así que una correlación alta significa que cuando el hiperparámetro tiene un valor más alto, la métrica también tiene valores más altos, y viceversa. La correlación es una métrica magnífica, a la cual hay que prestarle atención, pero no puede capturar interacciones de segundo orden entre las entradas, y puede volverse complicado comparar entradas con rangos extremadamente diferentes.

Consecuentemente, también calculamos una métrica de **importancia**, en donde entrenamos un bosque aleatorio con los hiperparámetros como entradas y la métrica como la salida objetivo, y reportamos los valores de importancia de la característica para el bosque aleatorio.

