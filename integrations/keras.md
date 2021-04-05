---
description: How to integrate a Keras script to log metrics to W&B
---

# Keras

Utiliza el callback de Keras para guardar automáticamente todas las métricas y los valores de pérdida rastreados en `model.fit`.

{% code title="example.py" %}
```python
import wandb
from wandb.keras import WandbCallback
wandb.init(config={"hyper": "parameter"})

# Magic

model.fit(X_train, y_train,  validation_data=(X_test, y_test),
          callbacks=[WandbCallback()])
```
{% endcode %}

Prueba nuestra integración en una [notebook ](https://colab.research.google.com/github/wandb/examples/blob/master/colabs/keras/Simple_Keras_Integration.ipynb)[de colab](https://colab.research.google.com/github/wandb/examples/blob/master/colabs/keras/Simple_Keras_Integration.ipynb), complétala con el [video tutorial](https://www.youtube.com/watch?v=Bsudo7jbMow&ab_channel=Weights%26Biases), o mira nuestros [proyectos de ejemplo](https://docs.wandb.ai/examples) para obtener un ejemplo completp de un script.

#### Opciones

La clase `WandbCallback()` de Keras soporta varias opciones:

| Argumento de palabra clave | Defecto | Descripción |
| :--- | :--- | :--- |
| monitor | val\_loss | La métrica de entrenamiento utilizada para medir el desempeño para guardar el mejor modelo, es decir, var\_loss |
| mode | auto | ‘min’, ‘max’ o ‘auto’: Cómo comparar la métrica de entrenamiento especificada en `monitor` entre diferentes pasos |
| save\_weights\_only | False | solamente guarda los pesos, en lugar del modelo entero |
| save\_model | True | guarda el modelo si este es mejorado en cada paso |
| log\_weights | False | registra los valores de cada uno de los parámetros de las capas en cada época |
| log\_gradients | False | registra los gradientes de cada uno de los parámetros de las capas en cada época |
| training\_data | None | tupla \(X,y\) necesaria para calcular los gradientes |
| data\_type | None | el tipo de datos que estamos guardando, actualmente sólo es soportado “image” |
| labels | None | usado solamente si data\_type está especificado, lista de etiquetas a las que se convertirá la salida numérica si estás construyendo un clasificador. \(soporta clasificación binaria\) |
| predictions | 36 | el número de predicciones a realizar si está especificado data\_type. El máximo es 100. |
| generator | None | si se utiliza aumentación de datos y data\_type, puedes especificar un generador con el cual hacer predicciones. |

##  Preguntas Comunes

### Utiliza el multiprocesamiento de Keras con wandb

Si estás estableciendo `use_multiprocessing=True` y ves el error`Error('You must call wandb.init() before wandb.config.batch_size')` intenta esto:

1. En la clase Sequence, agrega al método init lo siguiente: `wandb.init(group='...')`
2.  En tu programa principal, asegúrate de que estés usando `if name == "main"`: y entonces pon el resto de la lógica de tu script ahí adentro.

## Ejemplos

Hemos creado algunos ejemplos para que veas cómo funciona la integración:

* [Ejemplo en Github](https://github.com/wandb/examples/blob/master/examples/keras/keras-cnn-fashion/train.py): Ejemplo de moda MNIST en un script de Python
* Ejecución en Google Colab: Un ejemplo de una notebook simple para comenzar
* [Tablero de ](https://app.wandb.ai/wandb/keras-fashion-mnist/runs/5z1d85qs)[C](https://app.wandb.ai/wandb/keras-fashion-mnist/runs/5z1d85qs)[ontrol ](https://app.wandb.ai/wandb/keras-fashion-mnist/runs/5z1d85qs)[de W](https://app.wandb.ai/wandb/keras-fashion-mnist/runs/5z1d85qs)[andb](https://app.wandb.ai/wandb/keras-fashion-mnist/runs/5z1d85qs): Ver resultados en W&B

