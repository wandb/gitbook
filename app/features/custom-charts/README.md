---
description: Visualizaciones personalizadas y paneles personalizados utilizando consultas
---

# Custom Charts

Utiliza los Gráficos Personalizados para crear gráficos que no son posible de momento, desde la interfaz de usuario por defecto. Registra tablas de datos arbitrarias y visualízalas exactamente como lo desees. Controla los detalles de las fuentes, los colores y los mensajes emergentes con el poder de [Vega](https://vega.github.io/vega/).

* **Lo que es posible**: Lee el [anuncio del lanzamiento →](https://wandb.ai/wandb/posts/reports/Announcing-the-W-B-Machine-Learning-Visualization-IDE--VmlldzoyNjk3Nzg)
* **Código**: Prueba un ejemplo en tiempo real en una [notebook ](https://tiny.cc/custom-charts)[alojada](https://tiny.cc/custom-charts)[ →](https://tiny.cc/custom-charts)
* **Mira un** [video rápido guía →](https://www.youtube.com/watch?v=3-N9OV6bkSM)
* **Ejemplo:** [Notebook de demostración](https://colab.research.google.com/drive/1g-gNGokPWM2Qbc8p1Gofud0_5AoZdoSD?usp=sharing) simple con Keras y Sklearn →

 Comunícate con Carey \([c@wandb.com](mailto:c@wandb.com)\) para hacerle preguntas o sugerencias

![Supported charts from vega.github.io/vega](../../../.gitbook/assets/screen-shot-2020-09-09-at-2.18.17-pm.png)

###  Cómo funciona

1. **Registra datos:** Desde tu script, registra datos de [configuración](https://docs.wandb.ai/library/config) y de síntesis, como lo harías normalmente cuando ejecutas con W&B. Para visualizar una lista de múltiples valores registrados en un momento específico, utiliza un `wandb.Table` persoanlizado.
2. **Personaliza el gráfico:** Toma cualquiera de estos datos registrados con una consulta de [GraphQL](https://graphql.org/). Visualiza los datos de tu consulta con [Vega](https://vega.github.io/vega/), una poderosa gramática de visualización.
3. **Registra el gráfico:** Llama a tus propios preajustes desde tu script con wandb.plot\_table\(\) o utiliza uno de nuestros preajustes incorporados.

{% page-ref page="walkthrough.md" %}

![](../../../.gitbook/assets/pr-roc.png)

## Registra gráficos desde un script

### Preajustes incorporados

Estos preajustes tienen métodos `wandb.plot` incorporados que aceleran el registro de gráficos directamente desde tu script, y hacen que veas las visualizaciones exactas que estás buscando en la interfaz de usuario.

{% tabs %}
{% tab title="Gráfico de líneas" %}
`wandb.plot.line()`

**Registra un diagrama de líneas personalizado – una lista de puntos \(x,y\) conectados y ordenados sobre ejes x e y arbitrarios.**

```python
data = [[x, y] for (x, y) in zip(x_values, y_values)]
table = wandb.Table(data=data, columns = ["x", "y"])
wandb.log({"my_custom_plot_id" : wandb.plot.line(table, "x", "y", title="Custom Y vs X Line Plot")})
```

**Puedes usar esto para registrar curvas sobre dos dimensiones cualquiera. Notar que si estás trazando dos listas de valores entre sí, el número de los valores en las listas debe coincidir de forma exacta \(es decir, cada punto debe tener un x y un y\).**

![](../../../.gitbook/assets/line-plot.png)

 [Ver en la aplicación →](https://wandb.ai/wandb/plots/reports/Custom-Line-Plots--VmlldzoyNjk5NTA)

[Ejecutar](https://tiny.cc/custom-charts)[ el código →](https://tiny.cc/custom-charts)
{% endtab %}

{% tab title="Scatter plot" %}
`wandb.plot.scatter()`

Log a custom scatter plot—a list of points \(x, y\) on a pair of arbitrary axes x and y.

```python
data = [[x, y] for (x, y) in zip(class_x_prediction_scores, class_y_prediction_scores)]
table = wandb.Table(data=data, columns = ["class_x", "class_y"])
wandb.log({"my_custom_id" : wandb.plot.scatter(table, "class_x", "class_y")})
```

You can use this to log scatter points on any two dimensions. Note that if you're plotting two lists of values against each other, the number of values in the lists must match exactly \(i.e. each point must have an x and a y\).

![](../../../.gitbook/assets/demo-scatter-plot.png)

[See in the app →](https://wandb.ai/wandb/plots/reports/Custom-Scatter-Plots--VmlldzoyNjk5NDQ)

[Run the code →](https://tiny.cc/custom-charts)
{% endtab %}

{% tab title="Bar chart" %}
`wandb.plot.bar()`

Log a custom bar chart—a list of labeled values as bars—natively in a few lines:

```python
data = [[label, val] for (label, val) in zip(labels, values)]
table = wandb.Table(data=data, columns = ["label", "value"])
wandb.log({"my_bar_chart_id" : wandb.plot.bar(table, "label", "value", title="Custom Bar Chart")
```

You can use this to log arbitrary bar charts. Note that the number of labels and values in the lists must match exactly \(i.e. each data point must have both\).

![](../../../.gitbook/assets/image%20%2896%29.png)

[See in the app →](https://wandb.ai/wandb/plots/reports/Custom-Bar-Charts--VmlldzoyNzExNzk)

[Run the code →](https://tiny.cc/custom-charts)
{% endtab %}

{% tab title="Histogram" %}
`wandb.plot.histogram()`

Log a custom histogram—sort list of values into bins by count/frequency of occurrence—natively in a few lines. Let's say I have a list of prediction confidence scores \(`scores`\) and want to visualize their distribution:

```python
data = [[s] for s in scores]
table = wandb.Table(data=data, columns=["scores"])
wandb.log({'my_histogram': wandb.plot.histogram(table, "scores", title=None)})
```

You can use this to log arbitrary histograms. Note that `data` is a list of lists, intended to support a 2D array of rows and columns.

![](../../../.gitbook/assets/demo-custom-chart-histogram.png)

[See in the app →](https://wandb.ai/wandb/plots/reports/Custom-Histograms--VmlldzoyNzE0NzM)

[Run the code →](https://tiny.cc/custom-charts)
{% endtab %}

{% tab title="PR curve" %}
`wandb.plot.pr_curve()`

Log a [Precision-Recall curve](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_recall_curve.html#sklearn.metrics.precision_recall_curve) in one line:

```python
wandb.log({"pr" : wandb.plot.pr_curve(ground_truth, predictions,
                     labels=None, classes_to_plot=None)})
```

You can log this whenever your code has access to:

* a model's predicted scores \(`predictions`\) on a set of examples
* the corresponding ground truth labels \(`ground_truth`\) for those examples
* \(optionally\) a list of the labels/class names \(`labels=["cat", "dog", "bird"...]` if label index 0 means cat, 1 = dog, 2 = bird, etc.\)
* \(optionally\) a subset \(still in list format\) of the labels to visualize in the plot

![](../../../.gitbook/assets/demo-precision-recall.png)

[See in the app →](https://wandb.ai/wandb/plots/reports/Plot-Precision-Recall-Curves--VmlldzoyNjk1ODY)

[Run the code →](https://colab.research.google.com/drive/1mS8ogA3LcZWOXchfJoMrboW3opY1A8BY?usp=sharing)
{% endtab %}

{% tab title="ROC curve" %}
`wandb.plot.roc_curve()`

Log an [ROC curve](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_curve.html#sklearn.metrics.roc_curve) in one line:

```text
wandb.log({"roc" : wandb.plot.roc_curve( ground_truth, predictions, \
                        labels=None, classes_to_plot=None)})
```

You can log this whenever your code has access to:

* a model's predicted scores \(`predictions`\) on a set of examples
* the corresponding ground truth labels \(`ground_truth`\) for those examples
* \(optionally\) a list of the labels/ class names \(`labels=["cat", "dog", "bird"...]` if label index 0 means cat, 1 = dog, 2 = bird, etc.\)
* \(optionally\) a subset \(still in list format\) of these labels to visualize on the plot

![](../../../.gitbook/assets/demo-custom-chart-roc-curve.png)

[See in the app →](https://wandb.ai/wandb/plots/reports/Plot-ROC-Curves--VmlldzoyNjk3MDE)

[Run the code →](https://colab.research.google.com/drive/1_RMppCqsA8XInV_jhJz32NCZG6Z5t1RO?usp=sharing)
{% endtab %}
{% endtabs %}

### Gráfico de Dispersión

Registra un diagrama de dispersión personalizado – una lista de puntos \(x, y\) sobre un par de ejes arbitrarios x e y.

```python
# Create a table with the columns to plot
table = wandb.Table(data=data, columns=["step", "height"])

# Map from the table's columns to the chart's fields
fields = {"x": "step",
          "value": "height"}

# Use the table to populate the new custom chart preset
# To use your own saved chart preset, change the vega_spec_name
my_custom_chart = wandb.plot_table(vega_spec_name="carey/new_chart",
              data_table=table,
              fields=fields,
              )
```

[Run the code →](https://tiny.cc/custom-charts)

![](../../../.gitbook/assets/image%20%2897%29.png)

## Registra datos

Aquí están los tipos de datos que puedes registrar desde tu script y usarlos en un gráfico personalizado:

* Configuración: Ajustes iniciales de tu experimento \(tus variables independientes\). 
* Esto incluye cualquier campo nombrado que hayas registrado como una clave en wandb.config al comienzo de tu entrenamiento \(por ejemplo, wandb.config.learning\_rate = 0.0001\). 
* Síntesis: Valores simples registrados durante el entrenamiento \(tus resultados o las variables dependientes\), por ejemplo wandb.log\({"val\_acc" : 0.8}\). 
* Si escribes esta clave múltiples veces durante el entrenamiento, a través de wandb.log\(\), la síntesis es establecida como el valor final de dicha clave..
*  Historial: La serie de tiempos completa de un escalar registrado está disponible para ser consultado a través del campo history.. 
* summaryTable: Si necesitas registrar una lista de múltiples valores, utiliza wandb.Table\(\) para guardar los datos, entonces consúltala en tu panel personalizado.

### Cómo registrar una tabla personalizada

 Utiliza `wandb.Table()` para registrar tus datos como un arreglo en 2D. En general, cada fila de la tabla representa un punto de datos, y cada columna denota los campos/dimensiones relevantes para cada punto de datos que te gustaría trazar. Mientras configuras un panel personalizado, la tabla completa va a ser accesible a través de la clave nombrada pasada a `wandb.log()` \("custom\_data\_table" abajo\), y los campos individuales van a ser accesibles a través de los nombres de las columnas \(“x”, “y” y “z”\). Puedes registrar las tablas en múltiples pasos de tiempo a través de todo tu experimento. El tamaño máximo de cada tabla es de 10.000 filas.

 [Pruébalo en un Colab de Google →](https://tiny.cc/custom-charts)

```python
# Logging a custom table of data
my_custom_data = [[x1, y1, z1], [x2, y2, z2]]
wandb.log({“custom_data_table”: wandb.Table(data=my_custom_data,
                                columns = ["x", "y", "z"])})
```

## Personalizar al Gráfico

Para empezar, agrega un nuevo gráfico personalizado, entonces edita la consulta para seleccionar los datos de tus ejecuciones visibles. La consulta utiliza [GraphQL](https://graphql.org/) para buscar datos de los campos config, summary y history en tus ejecuciones.

![Add a new custom chart, then edit the query](../../../.gitbook/assets/2020-08-28-06.42.40.gif)

### Visualizaciones personalizadas

Selecciona un Gráfico en la esquina superior derecha para comenzar con un preajuste predeterminado. A continuación, selecciona **Campos del gráfico** para mapear los datos que estás tomando de la consulta a los campos correspondientes de tu gráfico. Aquí hay un ejemplo de la selección de una métrica para obtenerla desde la consulta, y entonces mapear eso en los campos de un gráfico de barras por debajo.

![Creating a custom bar chart showing accuracy across runs in a project](../../../.gitbook/assets/demo-make-a-custom-chart-bar-chart.gif)

###  Cómo editar Vega

Haz click en Editar, en la parte superior del panel, para ir al modo edición de [Vega](https://vega.github.io/vega/). Aquí puedes definir una [especificación de Vega](https://vega.github.io/vega/docs/specification/) que crea un gráfico interactivo en la interfaz de usuario. Puedes cambiar cualquier aspecto del gráfico, desde el estilo visual \(por ejemplo, cambiar el título, seleccionar un color de esquema diferente, mostrar las curvas como una serie de puntos en lugar de como líneas conectadas\) a los datos mismos \(utiliza una transformación de Vega para contener un arreglo de valores en un histograma, etc.\). La visualización previa del panel se va a actualizar interactivamente, así puedes ver el efecto de tus cambios a medida que edites la especificación de Vega o la consulta. [La documentación y los tutoriales de Vega](https://vega.github.io/vega/) son una fuente de inspiración excelente.

**Referencias de los campos**

Para llevar datos a tu gráfico de W&B, agrega plantillas de texto de la forma "${field:}" en cualquier lugar de tu especificación de Vega. Esto va a crear una lista desplegable en el área de Campos de los Gráficos, a mano derecha, que los usuarios pueden usar para seleccionar una columna con los resultados de un consulta para mapear a Vega.

Para establecer un valor por defecto para un campo, utiliza la sintaxis:`"${field:<field-name>:<placeholder text>}"`

### Guarda preajustes de los gráficos

Aplica cualquier cambio a un panel de visualización específico con el botón en la parte inferior del modal. Alternativamente, puedes guardar la especificación de Vega para usarla en cualquier lugar de tu proyecto. Para guardar la definición reutilizable del gráfico, haz click en **Guardar como** en la parte superior del editor de Vega y dale un nombre a tu preajuste.

## Artículos y guías

1. [La IDE de Visualización de](https://wandb.ai/wandb/posts/reports/The-W-B-Machine-Learning-Visualization-IDE--VmlldzoyNjk3Nzg)[l](https://wandb.ai/wandb/posts/reports/The-W-B-Machine-Learning-Visualization-IDE--VmlldzoyNjk3Nzg)[ Aprendizaje de Máquinas de W&B](https://wandb.ai/wandb/posts/reports/The-W-B-Machine-Learning-Visualization-IDE--VmlldzoyNjk3Nzg)
2.  [Visualizando Modelos NLP Basados en la Atención](https://wandb.ai/kylegoyette/gradientsandtranslation2/reports/Visualizing-NLP-Attention-Based-Models-Using-Custom-Charts--VmlldzoyNjg2MjM)
3.  [Visualizando el Efecto de la Atención ](https://wandb.ai/kylegoyette/gradientsandtranslation/reports/Visualizing-The-Effect-of-Attention-on-Gradient-Flow-Using-Custom-Charts--VmlldzoyNjg1NDg)[sobre la ](https://wandb.ai/kylegoyette/gradientsandtranslation/reports/Visualizing-The-Effect-of-Attention-on-Gradient-Flow-Using-Custom-Charts--VmlldzoyNjg1NDg)[Curva de la Máxima Pendiente](https://wandb.ai/kylegoyette/gradientsandtranslation/reports/Visualizing-The-Effect-of-Attention-on-Gradient-Flow-Using-Custom-Charts--VmlldzoyNjg1NDg)
4.  [Registrando Curvas Arbitrarias](https://wandb.ai/stacey/deep-drive/reports/Logging-arbitrary-curves--VmlldzoyMzczMjM)

## Preguntas realizadas frecuentemente

### Próximamente

* **Sondeo:** Refresco automático de los datos en el gráfico
* **Muestreo:** Ajuste dinámico del número total de puntos cargados en el panel para mejorar la eficiencia

### Problemas

* ¿No estás viendo los datos que estás esperando de la consulta mientras estás editando tu gráfico? Podría ser porque la columna que estás buscando no está registrada en las ejecuciones que has seleccionado. Guarda tu gráfico y vuelve a la tabla de las ejecuciones, y selecciona a las ejecuciones que te gustaría visualizar con el ícono del ojo.

### Casos de uso comunes

* Personaliza los gráficos de barras con barras de error
* Muestra las métricas de validación del modelo que requieren coordenadas x-y personalizadas \(como las curvas de precisión y exhaustividad\)
* Distribuciones de los datos de revestimiento a partir de dos modelos / experimentos diferentes como histogramas
* Muestra cambios en una métrica a través de las panorámicas, en múltiples puntos, durante el entrenamiento
* Crea una visualización única que aún no esté disponible en W&B \(y, con suerte, compártela con el mundo\)

