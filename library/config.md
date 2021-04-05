---
description: Objeto de tipo diccionario para guardar la configuración de tus experimentos.
---

# wandb.config

##  Resumen

[![](https://colab.research.google.com/assets/colab-badge.svg)](http://wandb.me/colab)

Establece el objeto `wandb.config` en tu script para guardar tu configuración de entrenamiento: hiperparámetros, ajustes de entrada como el nombre del conjunto de datos o el tipo del modelo, y cualquier otra variable independiente para tus experimentos. Esto es útil para analizar tus experimentos y reproducir tus trabajos en el futuro. Serás capaz de hacer agrupaciones por valores de configuración en la interfaz web, de comparar los ajustes de diferentes ejecuciones y de ver cómo éstos afectan a la salida. Notar que las métricas de salida, o las variables dependientes \(como la pérdida y la precisión\), en su lugar deberían ser guardadas con `wandb.log`.

Puedes enviarnos un diccionario anidado en la configuración, y en nuestro backend aplanaremos los nombres utilizando puntos. Te recomendamos que evites la utilización de puntos en los nombres de las variables de configuración y, en su lugar, utilices un guión o un guión bajo. Una vez que hayas creado tu diccionario de configuración wandb, si tu script accede a las claves de wandb.config, utiliza la sintaxis `[ ]`  en lugar de la sintaxis `.`.

## Ejemplo simple

```python
wandb.config.epochs = 4
wandb.config.batch_size = 32
# you can also initialize your run with a config
wandb.init(config={"epochs": 4})
```

##  Inicialización eficiente

 Puedes tratar a `wandb.config` como a un diccionario, actualizando múltiples valores a la vez.

```python
wandb.init(config={"epochs": 4, "batch_size": 32})
# or
wandb.config.update({"epochs": 4, "batch_size": 32})
```

##  Banderas argparse

 Puedes pasar el diccionario de argumentos desde argparse. Esto es conveniente para testear rápidamente diferentes valores de los hiperparámetros desde la línea de comandos.

```python
wandb.init()
wandb.config.epochs = 4

parser = argparse.ArgumentParser()
parser.add_argument('-b', '--batch-size', type=int, default=8, metavar='N',
                     help='input batch size for training (default: 8)')
args = parser.parse_args()
wandb.config.update(args) # adds all of the arguments as config variables
```

## Banderas absl

También puedes pasar banderas absl.

```python
flags.DEFINE_string(‘model’, None, ‘model to run’) # name, default, help
wandb.config.update(flags.FLAGS) # adds all absl flags to config
```

##  Configuraciones Basadas en Archivos

Puedes crear un archivo llamado config-defaults.yaml, \_\_ y automáticamente será cargado en `wandb.config`

```yaml
# sample config defaults file
epochs:
  desc: Number of epochs to train over
  value: 100
batch_size:
  desc: Size of each mini-batch
  value: 32
```

Puedes decirle a wandb que cargue diferentes archivos de configuración con el argumento de la línea de comandos `--configs special-configs.yaml`, lo cual cargará parámetros desde el archivo special-configs.yaml.

Una caso de uso de ejemplo: puedes tener un archivo YAML con algunos metadatos para la ejecución, y entonces un diccionario de hiperparámetros en tu script de Python. También puedes guardarlos a ambos en el objeto de configuración anidado:

```python
hyperparameter_defaults = dict(
    dropout = 0.5,
    batch_size = 100,
    learning_rate = 0.001,
    )

config_dictionary = dict(
    yaml=my_yaml_file,
    params=hyperparameter_defaults,
    )

wandb.init(config=config_dictionary)
```

##  Identificador del Conjunto de Datos

También puedes agregar un identificador único para tu conjunto de datos \(como un hash u otro identificador\) en la configuración de tu ejecución, al ponerlo como entrada a tu experimento usando `wandb.config`

```yaml
wandb.config.update({'dataset':'ab131'})
```

### Actualizar Archivos de Configuración

También puedes usar la API pública para actualizar tu archivo de configuración

```yaml
import wandb
api = wandb.Api()
run = api.run("username/project/run_id")
run.config["foo"] = 32
run.update()
```

###  Pares de Valores de Claves

Puedes registrar cualquier par de valores de claves en wandb.config. Ellos serán diferentes por cada tipo de modelo que estés entrenando. Es decir, `wandb.config.update({"my_param": 10, "learning_rate": 0.3, "model_architecture": "B"})`

## Banderas de TensorFlow \(obsoletas en tensorflow v2\)

Puedes pasar banderas de TensorFlow en el objeto de configuración.

```python
wandb.init()
wandb.config.epochs = 4

flags = tf.app.flags
flags.DEFINE_string('data_dir', '/tmp/data')
flags.DEFINE_integer('batch_size', 128, 'Batch size.')
wandb.config.update(flags.FLAGS)  # adds all of the tensorflow flags as config
```

