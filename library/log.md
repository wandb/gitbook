---
description: >-
  Haz un seguimiento de las métricas, de los videos, de los diagramas
  personalizados, y más
---

# wandb.log\(\)

 Llama a `wandb.log(dict)` para loguear un diccionario de métricas u objetos personalizados para un paso. Cada vez que loguees, incrementaremos el paso por defecto, permitiéndote ver las métricas a lo largo del tiempo.

###  Ejemplo de Uso

```python
wandb.log({'accuracy': 0.9, 'epoch': 5})
```

###  ****Procesos de Trabajo Comunes

1. **Comparar la mejor precisión:** Para comprar el mejor valor de una métrica a través de las ejecuciones, establece el valor summary para esa métrica. Por defecto, summary es establecido al último valor que registraste para cada clave. Esto es útil en la tabla de la Interfaz de Usuario, en donde puedes ordenar y filtrar las ejecuciones en base a las métricas de summary – de esta forma, podrías comparar ejecuciones en una tabla o en un gráfico de barras en base a su mejorprecisión, en vez de la precisión final. Por ejemplo, podrías establecer summary así:: `wandb.run.summary["accuracy"] = best_accuracy`
2.  **Múltiples métricas en un gráfico**: Registra múltiples métricas en la misma llamada a wandb.log\(\) de esta forma: `wandb.log({'acc': 0.9, 'loss': 0.1})`, y ambas estarán disponibles para diagramar contra el
3.  **Eje x personalizado:** Agrega un eje x personalizado a la misma llamada del registro para visualizar tus métricas contra un eje diferente en el tablero de control de W&B. Por ejemplo, `wandb.log({'acc': 0.9, 'custom_step': 3})`

###  Documentación de Referencia

 Mira la documentación de referencia, generada a partir de la biblioteca `wandb` de Python.

{% page-ref page="../ref/run.md" %}

## Registrar Objetos

 Soportamos imágenes, video, audio, gráficos personalizados, y más. Registra medios avanzados para explorar tus resultados, y visualiza las comparaciones entre tus ejecuciones.

[ ](https://colab.research.google.com/drive/15MJ9nLDIXRvy_lCwAou6C2XN3nppIeEK) [Inténtalo en Colab](https://colab.research.google.com/drive/15MJ9nLDIXRvy_lCwAou6C2XN3nppIeEK)

###  Histogramas

```python
wandb.log({"gradients": wandb.Histogram(numpy_array_or_sequence)})
wandb.run.summary.update({"gradients": wandb.Histogram(np_histogram=np.histogram(data))})
```

Si es provista una secuencia como primer argumento, vamos a guardar el histograma automáticamente en un contenedor. También puedes pasar lo que es devuelto desde np.histogram al argumento con la palabra clave np\_histogram, para hacer tu propio agrupamiento. El número máximo de contenedores soportados es de 512. Puedes usar el argumento opcional de palabra clave num\_bins cuando pases una secuencia, para sobrescribir el valor por defecto de 64 contenedores.

Si los histogramas están en tu síntesis, estos aparecerán como minigráficos en las páginas de ejecución individuales. Si están en tu historial, graficamos un mapa de calor a lo largo del tiempo.

### Imágenes y Revestimientos

{% tabs %}
{% tab title="Image" %}
`wandb.log({"examples": [wandb.Image(numpy_array_or_pil, caption="Label")]})`

Si es provisto un arreglo numpy, asumimos que se trata de una escala de grises si la última dimensión es 1, RGB si es 3, y RGBA si es 4. Si el arreglo contiene floats, los convertimos a ints entre 0 y 255. Puedes especificar manualmente un [modo](https://pillow.readthedocs.io/en/3.1.x/handbook/concepts.html#concept-modes) o sólo proveer un PIL.Image. Es recomendado registrar menos de 50 imágenes por paso.
{% endtab %}

{% tab title="Máscara de Segmentación" %}
If yoSi tienes imágenes con máscaras para la segmentación semántica, puedes registrar las máscaras y activarlas o desactivarlas en la Interfaz de Usuario. Para registrar múltiples máscaras, registra un diccionario de máscaras con múltiples claves. Aquí hay un ejemplo:

* **mask\_data**: un arreglo numpy en 2D conteniendo una etiqueta de clase de integer por cada píxel.
* **class\_labels**: un diccionario mapeando los números de mask\_data a etiquetas legibles

```python
mask_data = np.array([[1, 2, 2, ... , 2, 2, 1], ...])

class_labels = {
  1: "tree",
  2: "car",
  3: "road"
}

mask_img = wandb.Image(image, masks={
  "predictions": {
    "mask_data": mask_data,
    "class_labels": class_labels
  },
  "groud_truth": {
    ...
  },
  ...
})
```

[See a live example →](https://app.wandb.ai/stacey/deep-drive/reports/Image-Masks-for-Semantic-Segmentation--Vmlldzo4MTUwMw)

[Sample code →](https://colab.research.google.com/drive/1SOVl3EvW82Q4QKJXX6JtHye4wFix_P4J)
{% endtab %}

{% tab title="Bounding Box" %}
Registra las cajas delimitadoras con imágenes, y utiliza filtros y conmutadores para visualizar dinámicamente diferentes conjuntos de cajas en la Interfaz de Usuario.

```python
class_id_to_label = {
    1: "car",
    2: "road",
    3: "building",
    ....
}

img = wandb.Image(image, boxes={
    "predictions": {
        "box_data": [{
            "position": {
                "minX": 0.1,
                "maxX": 0.2,
                "minY": 0.3,
                "maxY": 0.4,
            },
            "class_id" : 2,
            "box_caption": "minMax(pixel)",
            "scores" : {
                "acc": 0.1,
                "loss": 1.2
            },
        }, 
        # Log as many boxes as needed
        ...
        ],
        "class_labels": class_id_to_label
    },
    "ground_truth": {
    # Log each group of boxes with a unique key name
    ...
    }
})

wandb.log({"driving_scene": img})
```

Parámetros Opcionales

Un argumento opcional que mapea tus class\_ids a valores string. Por defecto, vamos a generar `class_labels` como `class0`, `class1`, etc.

 Cajas – Cada caja pasada en box\_data puede ser definida con diferentes sistemas de coordenadas.

`position`

*  Opción 1: `{minX, maxX, minY, maxY}` Provee un conjunto de coordenadas que define los límites superiores e inferiores de cada dimensión de la caja.
*   Opción 2: `{middle, width, height}` Provee un conjunto de coordenadas, especificando las coordenadas middle como `[x, y]`, y width y height como escalares

`domain` Cambia el dominio de los valores de tu posición, basándose en la representación de tus datos

* `percentage` \(por defecto\) Un valor relativo que representa el porcentaje de la imagen como una distancia
* `pixel`Un valor absoluto en píxeles

 [Mira un ejemplo en ](https://app.wandb.ai/stacey/yolo-drive/reports/Bounding-Boxes-for-Object-Detection--Vmlldzo4Nzg4MQ)[tiempo real](https://app.wandb.ai/stacey/yolo-drive/reports/Bounding-Boxes-for-Object-Detection--Vmlldzo4Nzg4MQ)[ →](https://app.wandb.ai/stacey/yolo-drive/reports/Bounding-Boxes-for-Object-Detection--Vmlldzo4Nzg4MQ)

![](../.gitbook/assets/bb-docs.jpeg)
{% endtab %}
{% endtabs %}

###  Medios

{% tabs %}
{% tab title="Audio" %}
```python
wandb.log({"examples": [wandb.Audio(numpy_array, caption="Nice", sample_rate=32)]})
```

El número máximo de clips de audio que pueden registrarse por cada paso es de 100
{% endtab %}

{% tab title="Video" %}
```python
wandb.log({"video": wandb.Video(numpy_array_or_path_to_video, fps=4, format="gif")})
```

Si es provisto un arreglo numpy, asumimos que las dimensiones son: tiempo, canales, ancho, alto. Por defecto, creamos una imagen gif de 4 fps \(son requeridos ffmpeg y la biblioteca de python moviepy cuando se pasan objetos numpy\). Los formatos soportados son “gif”, “mp4”, “webm”, y “ogg”. Si pasas un string a `wandb.Video`, vamos a confirmar que el archivo existe y que se trata de un formato soportado, antes de subirlo a wandb. Al pasar un objeto BytesIO, se creará un archivo temporal con un formato especificado como la extensión.

En la pagina de ejecuciones de W&B, verás tus videos en la sección Medios.
{% endtab %}

{% tab title="" %}
Utiliza wandb.Table\(\) para registrar texto en las tablas que se muestran en la Interfaz de Usuario. Por defecto, las cabeceras de las columnas son `["Input", "Output", "Expected"]`. El número máximo de filas es de 10.000.

```python
# Method 1
data = [["I love my phone", "1", "1"],["My phone sucks", "0", "-1"]]
wandb.log({"examples": wandb.Table(data=data, columns=["Text", "Predicted Label", "True Label"])})

# Method 2
table = wandb.Table(columns=["Text", "Predicted Label", "True Label"])
table.add_data("I love my phone", "1", "1")
table.add_data("My phone sucks", "0", "-1")
wandb.log({"examples": table})
```

 También puedes pasar un objeto `DataFrame` de pandas.

```python
table = wandb.Table(dataframe=my_dataframe)
```
{% endtab %}

{% tab title="HTML" %}
```python
wandb.log({"custom_file": wandb.Html(open("some.html"))})
wandb.log({"custom_string": wandb.Html('<a href="https://mysite">Link</a>')})
```

El html personalizado puede ser registrado en cualquier clave, esto expone un panel HTML en la página de ejecución. Por defecto, inyectamos estilos por defecto, y puedes deshabilitarlos al pasar `inject=False`.

```python
wandb.log({"custom_file": wandb.Html(open("some.html"), inject=False)})
```
{% endtab %}

{% tab title="Molécula" %}
```python
wandb.log({"protein": wandb.Molecule(open("6lu7.pdb"))}
```

 Registra datos moleculares en cualquiera de los 10 tipos de archivos

`'pdb', 'pqr', 'mmcif', 'mcif', 'cif', 'sdf', 'sd', 'gro', 'mol2', 'mmtf'`

Cuando tu ejecución finalice, serás capaz de interactuar con visualizaciones en 3D de tus moléculas desde la Interfaz de Usuario.

[Ver un ejemplo en tiempo real →](https://app.wandb.ai/nbaryd/Corona-Virus/reports/Visualizing-Molecular-Structure-with-Weights-%26-Biases--Vmlldzo2ODA0Mw)

![](../.gitbook/assets/docs-molecule.png)
{% endtab %}
{% endtabs %}

### Gráficos Personalizados

Estos preajustes tienen métodos `wandb.plot` incrustados que hacen que registrar gráficos directamente desde tu script sea rápido, y así puedes ver las visualizaciones exactas que estás buscando en la Interfaz de Usuario.

{% tabs %}
{% tab title="Línea" %}
`wandb.plot.line()`

Registra un diagrama de líneas personalizado – una lista de puntos \(x,y\) conectados y ordenados sobre ejes x e y arbitrarios.

```python
data = [[x, y] for (x, y) in zip(x_values, y_values)]
table = wandb.Table(data=data, columns = ["x", "y"])
wandb.log({"my_custom_plot_id" : wandb.plot.line(table, "x", "y", title="Custom Y vs X Line Plot")})
```

Puedes usar esto para registrar curvas sobre dos dimensiones cualquiera. Notar que si estás trazando dos listas de valores entre sí, el número de los valores en las listas debe corresponderse de forma exacta \(es decir, cada punto debe tener un x y un y\).

![](../.gitbook/assets/line-plot.png)

 [Ver en la aplicación →](https://wandb.ai/wandb/plots/reports/Custom-Line-Plots--VmlldzoyNjk5NTA)

 [Ejecutar](https://tiny.cc/custom-charts)[ el código →](https://tiny.cc/custom-charts)
{% endtab %}

{% tab title="Scatter" %}
`wandb.plot.scatter()`

Log a custom scatter plot—a list of points \(x, y\) on a pair of arbitrary axes x and y.

```python
data = [[x, y] for (x, y) in zip(class_x_prediction_scores, class_y_prediction_scores)]
table = wandb.Table(data=data, columns = ["class_x", "class_y"])
wandb.log({"my_custom_id" : wandb.plot.scatter(table, "class_x", "class_y")})
```

You can use this to log scatter points on any two dimensions. Note that if you're plotting two lists of values against each other, the number of values in the lists must match exactly \(i.e. each point must have an x and a y\).

![](../.gitbook/assets/demo-scatter-plot.png)

[See in the app →](https://wandb.ai/wandb/plots/reports/Custom-Scatter-Plots--VmlldzoyNjk5NDQ)

[Run the code →](https://tiny.cc/custom-charts)
{% endtab %}

{% tab title="Bar" %}
`wandb.plot.bar()`

Log a custom bar chart—a list of labeled values as bars—natively in a few lines:

```python
data = [[label, val] for (label, val) in zip(labels, values)]
table = wandb.Table(data=data, columns = ["label", "value"])
wandb.log({"my_bar_chart_id" : wandb.plot.bar(table, "label", "value", title="Custom Bar Chart")
```

You can use this to log arbitrary bar charts. Note that the number of labels and values in the lists must match exactly \(i.e. each data point must have both\).

![](../.gitbook/assets/image%20%2896%29.png)

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

![](../.gitbook/assets/demo-custom-chart-histogram.png)

[See in the app →](https://wandb.ai/wandb/plots/reports/Custom-Histograms--VmlldzoyNzE0NzM)

[Run the code →](https://tiny.cc/custom-charts)
{% endtab %}

{% tab title="PR" %}
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

![](../.gitbook/assets/demo-precision-recall.png)

[See in the app →](https://wandb.ai/wandb/plots/reports/Plot-Precision-Recall-Curves--VmlldzoyNjk1ODY)

[Run the code →](https://colab.research.google.com/drive/1mS8ogA3LcZWOXchfJoMrboW3opY1A8BY?usp=sharing)
{% endtab %}

{% tab title="ROC" %}
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

![](../.gitbook/assets/demo-custom-chart-roc-curve.png)

[See in the app →](https://wandb.ai/wandb/plots/reports/Plot-ROC-Curves--VmlldzoyNjk3MDE)

[Run the code →](https://colab.research.google.com/drive/1_RMppCqsA8XInV_jhJz32NCZG6Z5t1RO?usp=sharing)
{% endtab %}

{% tab title="Confusion Matrix" %}
`wandb.plot.confusion_matrix()`

Log a multi-class [confusion matrix](https://scikit-learn.org/stable/auto_examples/model_selection/plot_confusion_matrix.html) in one line:

```text
wandb.log({"conf_mat" : wandb.plot.confusion_matrix(
                        predictions, ground_truth, class_names})
```

You can log this wherever your code has access to:

* a model's predicted scores on a set of examples \(`predictions`\)
* the corresponding ground truth labels for those examples \(`ground_truth`\)
* a full list of the labels/class names as strings \(`class_names`, e.g. `class_names=["cat", "dog", "bird"]` if index 0 means cat, 1 = dog, 2 = bird, etc\)

![](../.gitbook/assets/image%20%281%29%20%281%29%20%281%29%20%281%29%20%281%29%20%281%29%20%281%29%20%281%29%20%281%29.png)

​[See in the app →](https://wandb.ai/wandb/plots/reports/Confusion-Matrix--VmlldzozMDg1NTM)​

​[Run the code →](https://colab.research.google.com/drive/1OlTbdxghWdmyw7QPtSiWFgLdtTi03O8f?usp=sharing)
{% endtab %}
{% endtabs %}





### Dispersión

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

 [Ejecutar el código →](https://tiny.cc/custom-charts)

### Matplotlib

```python
import matplotlib.pyplot as plt
plt.plot([1, 2, 3, 4])
plt.ylabel('some interesting numbers')
wandb.log({"chart": plt})
```

Puedes pasar un objeto figura o un pyplot de `matplotlib` a `wandb.log()`. Por defecto, convertiremos el diagrama en un diagrama [Plotly](https://plot.ly/). Si quieres registrar explícitamente el diagrama como una imagen, puedes pasarlo a `wandb.Image`. También aceptamos el registro directo de gráficos Plotly.

### Visualizaciones en 3D

{% tabs %}
{% tab title="Objeto 3D" %}
 Registra archivos en formatos `obj`, `gltf` o `glb`, y los representaremos en la Interfaz de Usuario cuando tu ejecución finalice.

```python
wandb.log({"generated_samples":
           [wandb.Object3D(open("sample.obj")),
            wandb.Object3D(open("sample.gltf")),
            wandb.Object3D(open("sample.glb"))]})
```

![Datos reales y predicci&#xF3;n de una nube de puntos de audiculares](../.gitbook/assets/ground-truth-prediction-of-3d-point-clouds.png)

[See a live example →](https://app.wandb.ai/nbaryd/SparseConvNet-examples_3d_segmentation/reports/Point-Clouds--Vmlldzo4ODcyMA)
{% endtab %}

{% tab title="Nubes de Puntos" %}
 Registra nubes de puntos en 3D y escenas Lidar con cajas delimitadoras. Pasa un arreglo numpy conteniendo las coordenadas y los colores para los puntos que se van a representar.

```python
point_cloud = np.array([[0, 0, 0, COLOR...], ...])

wandb.log({"point_cloud": wandb.Object3D(point_cloud)})
```

Son soportadas tres formas diferentes de arreglos numpy para los esquemas de colores flexibles.

* `[[x, y, z], ...]` `nx3`
* `[[x, y, z, c], ...]` `nx4` `| c is a category` c es una categoría en el rango `[1, 14]` \(Útil para la segmentación\)
* `[[x, y, z, r, g, b], ...]` `nx6 | r,g,b` son valores en el rango`[0,255]`para los canales de colores rojo, verde y azul.

 Aquí hay un ejemplo del código de registro de abajo:

* `points` es un arreglo numpy con el mismo formato que el renderizador de la nube de puntos simple mostrado anteriormente.
* `boxes` es un arreglo numpy de diccionarios python con tres atributos:
  * `corners`- una lista de ocho esquinas
  * `label`- un string simbolizando la etiqueta a ser representada en la caja \(Opcional\)
  * `color`- alores rbg representando el color de la caja
* `type` es un string simbolizando el tipo de escena a representar. Actualmente, el único valor soportado es `lidar/beta`

```python
# Log points and boxes in W&B
wandb.log(
        {
            "point_scene": wandb.Object3D(
                {
                    "type": "lidar/beta",
                    "points": np.array(
                        [
                            [0.4, 1, 1.3], 
                            [1, 1, 1], 
                            [1.2, 1, 1.2]
                        ]
                    ),
                    "boxes": np.array(
                        [
                            {
                                "corners": [
                                    [0,0,0],
                                    [0,1,0],
                                    [0,0,1],
                                    [1,0,0],
                                    [1,1,0],
                                    [0,1,1],
                                    [1,0,1],
                                    [1,1,1]
                                ],
                                "label": "Box",
                                "color": [123,321,111],
                            },
                            {
                                "corners": [
                                    [0,0,0],
                                    [0,2,0],
                                    [0,0,2],
                                    [2,0,0],
                                    [2,2,0],
                                    [0,2,2],
                                    [2,0,2],
                                    [2,2,2]
                                ],
                                "label": "Box-2",
                                "color": [111,321,0],
                            }
                        ]
                    ),
                    "vectors": [
                        {"start": [0,0,0], "end": [0.1,0.2,0.5]}
                    ]
                }
            )
        }
    )
```
{% endtab %}
{% endtabs %}

##  Registro Incremental

 Si deseas trazar tus métricas contra diferentes ejes x, puedes registrar el paso como una métrica, como `wandb.log({'loss': 0.1, 'epoch': 1, 'batch': 3})`. En la Interfaz de Usuario, puedes intercambiar entre los ejes x en los ajustes del gráfico.

Si deseas registrar un paso simple del historial desde muchos lugares diferentes en tu código, puedes pasar un índice del paso a `wandb.log()` como a continuación:

```python
wandb.log({'loss': 0.2}, step=step)
```

 Siempre y cuando sigas pasando el mismo valor para `step`, W&B recogerá las claves y los valores de cada llamada en un diccionario unificado. Tan pronto como llames a `wandb.log()` con un valor step diferente al previo, W&B escribirá todas las claves y los valores recogidos al historial, y volverá a empezar la recolección otra vez. Ten en cuenta que esto significa que sólo deberías usar esto con valores consecutivos para `step`: 0, 1, 2… Esta característica no te permite escribir a absolutamente ningún paso del historial que quieras, sólo al “actual” y al “próximo”.

También puedes establecer commit=False en `wandb.log` para acumular métricas, sólo asegúrate de llamar a `wandb.log` sin la bandera commit para persistir las métricas.

```python
wandb.log({'loss': 0.2}, commit=False)
# Somewhere else when I'm ready to report this step:
wandb.log({'accuracy': 0.8})
```

##  Métricas de Síntesis

Las síntesis estadísticas son utilizadas para hacer un seguimiento de las métricas simples por modelo. Si se modifica una métrica de la síntesis, sólo se guarda el estado actualizado. Nosotros establecemos automáticamente la síntesis a la última fila del historial agregada, a menos que lo modifiques manualmente. Si cambias una métrica de la síntesis, sólo persistimos el último valor a la que ésta fue establecida.

```python
wandb.init(config=args)

best_accuracy = 0
for epoch in range(1, args.epochs + 1):
  test_loss, test_accuracy = test()
  if (test_accuracy > best_accuracy):
    wandb.run.summary["best_accuracy"] = test_accuracy
    best_accuracy = test_accuracy
```

Puedes querer almacenar métricas de evaluación en una síntesis de ejecuciones después de que el entrenamiento se haya completado. La síntesis puede manejar arreglos numpy, tensores pytorch o tensores tensorflow. Cuando un valor es de uno de estos tipos, persistimos el tensor entero en un archivo binario y almacenamos las métricas de alto nivel en un objeto síntesis, tales como mínimo, media, varianza, percentil 95, etc.

```python
api = wandb.Api()
run = api.run("username/project/run_id")
run.summary["tensor"] = np.random.random(1000)
run.summary.update()
```

### Accediendo a los registros de forma directa

El objeto history es utilizado para hacer un seguimiento de las métricas registradas por wandb.log. Puedes acceder a los diccionarios de métricas mutables a través de `run.history.row`. La fila será guardada y una nueva será creada cuando `run.history.add` o `wandb.log` sean llamados.

####  Ejemplo de Tensorflow

```python
wandb.init(config=flags.FLAGS)

# Start tensorflow training
with tf.Session() as sess:
  sess.run(init)

  for step in range(1, run.config.num_steps+1):
      batch_x, batch_y = mnist.train.next_batch(run.config.batch_size)
      # Run optimization op (backprop)
      sess.run(train_op, feed_dict={X: batch_x, Y: batch_y})
      # Calculate batch loss and accuracy
      loss, acc = sess.run([loss_op, accuracy], feed_dict={X: batch_x, Y: batch_y})

      wandb.log({'acc': acc, 'loss':loss}) # log accuracy and loss
```

####  Ejemplo de Pytorch

```python
# Start pytorch training
wandb.init(config=args)

for epoch in range(1, args.epochs + 1):
  train_loss = train(epoch)
  test_loss, test_accuracy = test()

  torch.save(model.state_dict(), 'model')

  wandb.log({"loss": train_loss, "val_loss": test_loss})
```

## Preguntas Comunes

###  ****Comparar imágenes a partir de diferentes épocas

 Cada vez que registras imágenes desde un paso, las guardamos para mostrarlas en la Interfaz de Usuario. Pincha el panel de imágenes, y utiliza el deslizador de pasos para ver las imágenes de los diferentes pasos. Esto facilita la comparación de cómo cambia la salida de un modelo sobre el entrenamiento.

```python
wandb.log({'epoch': epoch, 'val_acc': 0.94})
```

###  Registro en lote

Si quisieras registrar ciertas métricas en cada lote y estandarizar diagramas, puedes registrar los valores del eje x que quieras trazar con tus métricas. Entonces, en los diagramas personalizados, haz click en editar y selecciona el eje x personalizado.

```python
wandb.log({'batch': 1, 'loss': 0.3})
```

###  ****Registrar un PNG

 Por defecto, wandb.Image convierte arreglos numpy, o instancias de PILImage, a PNG.

```python
wandb.log({"example": wandb.Image(...)})
# Or multiple images
wandb.log({"example": [wandb.Image(...) for img in images]})
```

### Registrar un JPEG

Para guardar un JPEG puedes pasar una ruta a un archivo

```python
im = PIL.fromarray(...)
rgb_im = im.convert('RGB')
rgb_im.save('myimage.jpg')
wandb.log({"example": wandb.Image("myimage.jpg")})
```

### Registrar un Video

```python
wandb.log({"example": wandb.Video("myvideo.mp4")})
```

 Ahora puedes ver videos en el navegador de medios. Ve al espacio de trabajo de tu proyecto, ejecuta el espacio de trabajo, o reporta y haz click en “Agregar visualización” a un panel de medios avanzados.

### Eje x personalizado

Por defecto, incrementamos el paso global cada vez que llamas a wandb.log. Si quieres, puedes registrar tu propio paso incrementándolo monótonamente y entonces seleccionarlo como un eje x personalizado en tus gráficos.

Por ejemplo, si tienes pasos de entrenamiento y de validación que te gustaría alinear, pásanos tu propio contador de pasos: `wandb.log({“acc”:1, “global_step”:1})`. Entonces, en los gráficos, elige “global\_step” como el eje x.

`wandb.log({“acc”:1,”batch”:10}, step=epoch)` te permitiría elegir “lote” como un eje x, en adición al eje de los pasos por defecto.

### Navegar y acercar las nubes de puntos

Puedes tomar el control y usar el mouse para moverte mientras estás dentro del espacio

### No se muestra nada en los gráficos

 Si estás viendo “Aún no se han registrado datos para la visualización”, significa que aún no hemos obtenido la primera llamada a wandb.log desde tu script. Esto podría deberse a que tus ejecuciones toman mucho tiempo para terminar un paso. Si estás haciendo el registro al final de cada época, podrías registrar unas cuantas veces por época para ver los datos fluyendo más rápidamente.

### Duplicar los nombres de las métricas

Si estás registrando diferentes tipos bajo la misma clave, tenemos que dividirlos en la base de datos. Esto significa que verás múltiples entradas con el mismo nombre de la métrica en una lista desplegable en la Interfaz de Usuario. Los tipos por los que agrupamos son number, `string`, `bool`, `otros` \(en su mayoría arreglos\), y cualquier tipo wandb \(`histogramas`, `imágenes`, etc.\). Por favor, envía sólo un tipo a cada clave para evitar este comportamiento.

### Desempeño y límites

 **Muestreo**

Cuantos más puntos nos envíes, más tiempo llevará cargar tus gráficos en la Interfaz de Usuario. Si tienes más de 1000 puntos en una línea, tomamos muestras por debajo de los 1000 puntos en el backend antes de enviar los datos a tu navegador. Este muestreo es no determinístico, así que si refrescas la página verás un conjunto diferente de puntos muestreados.

Si quisieras todos los datos originales, puedes usar nuestra [API de datos](https://docs.wandb.com/library/api) para tomar los datos que no están muestreados.

 **Guías**

Te recomendamos que intentes registrar menos de 10.000 puntos por métrica. Si tienes más de 500 columnas de métricas de configuración y de síntesis, sólo mostraremos 500 en la tabla. Si registras más de un millón de puntos en una línea, nos tomará un rato cargar la página.

Almacenamos métricas de forma tal que éstas no son sensibles a las mayúsculas y a las minúsculas, así que asegúrate de no tener dos métricas con el mismo nombre como “My-Metric” y “my-metric”.

### Controlar la subida de las imágenes

“Quiero integrar W&B en mi proyecto, pero no quiero subir ninguna imagen”

Nuestra integración no carga imágenes de forma automática – tú especificas cualquier archivo que te gustaría cargar de forma explícita. Aquí hay un ejemplo simple que hice para PyTorch, en donde explícitamente registro imágenes: [http://bit.ly/pytorch-mnist-colab](http://bit.ly/pytorch-mnist-colab)

```python
wandb.log({
        "Examples": example_images,
        "Test Accuracy": 100. * correct / len(test_loader.dataset),
        "Test Loss": test_loss})
```

