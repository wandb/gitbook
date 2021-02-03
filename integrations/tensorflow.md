---
description: How to integrate a TensorFlow script to log metrics to W&B
---

# TensorFlow

Si ya estás usando TensorBoard, es fácil integrarlo con wandb.

```python
import tensorflow as tf
import wandb
wandb.init(config=tf.flags.FLAGS, sync_tensorboard=True)
```

Mira nuestros [proyectos de ejemplo](https://docs.wandb.ai/examples) para obtener un ejemplo de un script completo.

##  Métricas personalizadas

Si necesitas registrar métricas personalizadas adicionales, que no están siendo registradas a TensorBoard, puedes llamar a `wandb.log` en tu código con el mismo argumento step que está usando TensorFlow: es decir, `wandb.log({"custom": 0.8}, step=global_step)`

## Enganche a TensoFlow

 Si quieres más control sobre lo que es registrado, wandb también provee un enganche para los estimadores de TensorFlow. Esto registrará todos los valores de `tf.summary` en el gráfico.

```python
import tensorflow as tf
import wandb

wandb.init(config=tf.FLAGS)

estimator.train(hooks=[wandb.tensorflow.WandbHook(steps_per_log=1000)])
```

##  Registro Manual

The simplest way to log metrics in TensorFlow is by logging `tf.summary` with the TensorFlow logger:

```python
import wandb

with tf.Session() as sess:
    # ...
    wandb.tensorflow.log(tf.summary.merge_all())
```

Con TensorFlow 2, la forma recomendada de entrenar un modelo con un ciclo personalizado es utilizando `tf.GradientTape`. Puedes leer más acerca de esto [aquí](https://www.tensorflow.org/tutorials/customization/custom_training_walkthrough). Si deseas incorporar wandb a las métricas del registro en tus ciclos de entrenamiento personalizados de TensorFlow, puedes seguir este extracto de código

```python
    with tf.GradientTape() as tape:
        # Get the probabilities
        predictions = model(features)
        # Calculate the loss
        loss = loss_func(labels, predictions)

    # Log your metrics
    wandb.log("loss": loss.numpy())
    # Get the gradients
    gradients = tape.gradient(loss, model.trainable_variables)
    # Update the weights
    optimizer.apply_gradients(zip(gradients, model.trainable_variables))
```

Hay disponible un ejemplo completo [aquí](https://www.wandb.com/articles/wandb-customizing-training-loops-in-tensorflow-2).

##  ¿Qué tiene de diferente W&B respecto a TensorBoard?

Nos inspiramos en mejorar las herramientas de seguimiento de experimentos para todo el mundo. Cuando los cofundadores comenzaron a trabajar en W&B, se inspiraron en construir una herramienta para los usuarios de OpenAI insatisfechos con TensorBoard. Aquí hay algunas cosas en las que nos enfocamos para mejorar:

1. **Reproducir modelos**: Weights & Biases es bueno para la experimentación, la exploración, y la reproducción de los modelos en el futuro. No solo que capturamos las métricas, sino también los hiperparámetros y la versión del código, y podemos guardar los puntos de control de tu modelo para que tu proyecto sea reproducible.
2. **Organización automática**: Si le pasas un proyecto a un colaborador o te tomas unas vacaciones, W&B te facilita ver todos los modelos que hayas probado, así no pierdes horas volviendo a correr los viejos experimentos.
3. **Integración rápida y flexible**: Agrega W&B a tu proyecto en 5 minutos. Instala nuestro paquete Python, libre y de código abierto, y agrega un par de líneas a tu código, y cada vez que corras tu modelo vas a registrar las métricas y los registros.
4. **Tablero de control persistente y centralizado:** Dondequiera que entrenes tus modelos, ya sea en tu máquina local, tu cluster del laboratorio, o en instancias de spot en la nube, te ofrecemos el mismo tablero de control centralizado. No necesitas perder el tiempo copiando y organizando archivos de TensorFlow desde diferentes máquinas.
5. **Tabla poderosa:** Busca, filtra, ordena y agrupa resultados de diferentes modelos. Es fácil revisar miles de versiones de los modelos y encontrar al más adecuado para diferentes tareas. TensorBoard no funciona adecuadamente en proyectos grandes.
6. **Herramientas para la colaboración:** Utiliza W&B para organizar proyectos de aprendizaje de máquinas complejos. Es fácil compartir un enlace a W&B, y puedes utilizar equipos privados y hacer que todos envíen resultados a un proyecto compartido. También soportamos la colaboración a través de los repositorios – agrega visualizaciones interactivas y describe tu trabajo en markdown. Esta es una gran forma de mantener un trabajo registrado, de compartir resultados con tu supervisor, o de presentar resultados a tu laboratorio.

 Empieza con una [cuenta personal gratuita](http://app.wandb.ai/) →

## Ejemplo

Hemos creado algunos ejemplos para que veas cómo funciona la integración:

*  [Ejemplo en Github](https://github.com/wandb/examples/blob/master/examples/tensorflow/tf-estimator-mnist/mnist.py): Ejemplo de MNIST Usando Estimadores de TensorFlow
*  [Ejemplo en Github](https://github.com/wandb/examples/blob/master/examples/tensorflow/tf-cnn-fashion/train.py): Ejemplo de Moda MNIST Usando TensorFlow Puro
* [Tablero de ](https://app.wandb.ai/l2k2/examples-tf-estimator-mnist/runs/p0ifowcb)[C](https://app.wandb.ai/l2k2/examples-tf-estimator-mnist/runs/p0ifowcb)[ontrol de wandb](https://app.wandb.ai/l2k2/examples-tf-estimator-mnist/runs/p0ifowcb): Ver resultados en W&B
* Personalizando Ciclos de Entrenamiento en TensorFlow 2- [Artículo](https://www.wandb.com/articles/wandb-customizing-training-loops-in-tensorflow-2) \| [Notebook ](https://colab.research.google.com/drive/1JCpAbjkCFhYMT7LCQ399y35TS3jlMpvM)[de Colab](https://colab.research.google.com/drive/1JCpAbjkCFhYMT7LCQ399y35TS3jlMpvM) \| [Tablero de Control](https://app.wandb.ai/sayakpaul/custom_training_loops_tf)

