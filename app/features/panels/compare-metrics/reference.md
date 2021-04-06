# Reference

## X-Axis

![Selecting X-Axis](../../../../.gitbook/assets/image%20%2815%29.png)

Puedes establecer el Eje X de un gráfico de líneas a cualquier valor que hayas registrado con wandb.log, siempre y cuando dicho valor sea siempre registrado con un valor numérico.

## Variables del Eje Y

Puedes establecer las variables del eje y a cualquier valor que hayas registrado con wandb.log, siempre y cuando éstos sean números, arreglos de números o un histograma de números. Si registraste más de 1500 puntos para una variable, wandb muestrea sólo 1500 puntos.

{% hint style="info" %}
Puedes cambiar el color de las líneas tu eje y al cambiar el color de la ejecución, en la tabla de las ejecuciones.
{% endhint %}

## Rango X y Rango Y

Puedes cambiar los valores máximo y mínimo de X e Y para el gráfico.

El valor predeterminado del rango X, es desde el valor más pequeño de tu eje x al más grande.

El valor predeterminado del rango Y, se toma de entre el valor más pequeño de tus métricas y cero, al valor más grande de tus métricas.

## Ejecuciones/Grupos Máximos

Por defecto, vas a graficar sólo 10 ejecuciones o grupos de ejecuciones. Las ejecuciones van a ser tomadas desde la parte superior de tu tabla de ejecuciones o de tu conjunto de ejecuciones, así que si ordenas la tabla, o el conjunto de ejecuciones, puedes cambiar las ejecuciones que van a ser mostradas.

## Leyenda

  
Puedes controlar la leyenda del gráfico que vas a mostrar para cualquier ejecución, cualquier valor de configuración que hayas registrado, y los metadatos de las ejecuciones, tales como los creados en el momento o el usuario que creó la ejecución.

Ejemplo:

${config:x} va a insertar el valor de configuración de x para una ejecución o para un grupo.

Puedes establecer \[\[$x: $y\]\] para visualizar valores específicos de puntos en el eje.

## Agrupamiento

Puedes agregar todas las ejecuciones al activar el agrupamiento, o agrupar sobre una variable individual. También puedes activar el agrupamiento al agrupar dentro de la tabla, y los grupos van a ser automáticamente rellenados en el gráfico.

## Suavizado

Puedes establecer el [coeficiente de suavizado](https://docs.wandb.ai/library/technical-faq#what-formula-do-you-use-for-your-smoothing-algorithm) para que esté entre 0 y 1, donde 0 significa que no habrá suavizado, y 1 es el suavizado máximo.

## Ignora Valores Atípicos

Ignorar valores atípicos hace que el gráfico establezca el mínimo y el máximo del eje y a los percentils 5 y 95 de los datos, respectivamente, en lugar de hacer que todos los datos sean visibles.

## Expresión

Las expresiones te permiten diagramar valores derivados de las métricas, como 1-precisión. Actualmente sólo funciona si estás diagramando una métrica simple. Puedes hacer expresiones aritméticas simples, +, -, _, / y %, así también como \*_ para la potencia.

## Estilo de Gráfico

Selecciona un estilo para tu gráfico de líneas.

 **Gráfico de líneas:**

![](../../../../.gitbook/assets/image%20%285%29%20%282%29%20%283%29.png)

**Gráfico de área:**

![](../../../../.gitbook/assets/image%20%2835%29%20%281%29%20%282%29%20%283%29%20%281%29.png)

**Gráfico de área porcentual:**

![](../../../../.gitbook/assets/image%20%2869%29%20%284%29%20%286%29%20%282%29.png)

