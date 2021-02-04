# Config

## wandb.sdk.wandb\_config

 [\[fuente\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_config.py#L3)

Config es un objeto de tipo diccionario que es útil para hacer el seguimiento de las entradas a tu script, como los hiperparámetros. Te sugerimos que lo establezcas una vez al comenzar tu trabajo, cuando inicialices la ejecución, de esta forma: `wandb.init(config={"key": "value"})`. Por ejemplo, si estás entrenado un modelo de ML, podrías hacer el seguimiento de learning\_rate y batch\_size en config.

### Objetos Config

```python
class Config(object)
```

 [\[fuente\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_config.py#L32)

Utiliza el objeto config para guardar los hiperparámetros de tu ejecución. Cuando llames a wandb.init\(\) para empezar una nueva ejecución rastreada, se guarda un objeto run. Te recomendamos que guardes al objeto config al mismo tiempo que lo haces con el objeto run, de esta forma: wandb.init\(config=my\_config\_dict\).

Puedes crear un archivo llamado config-defaults.yaml, y wandb lo cargará automáticamente a tu configuración en wandb.config. Alternativamente, puedes usar un archivo YAML con un nombre personalizado y pasar el nombre del archivo: `wandb.init(config="my_config_file.yaml")`. Mira [https://docs.wandb.com/library/config\#file-based-configs](https://docs.wandb.com/library/config#file-based-configs).

 **Ejemplos:**

Uso básico

```text
wandb.config.epochs = 4
wandb.init()
for x in range(wandb.config.epochs):
# train
```

Usando wandb.init para establecer la configuración

```text
- `wandb.init(config={"epochs"` - 4, "batch_size": 32})
for x in range(wandb.config.epochs):
# train
```

Configuraciones anidadas

```text
wandb.config['train']['epochs] = 4
wandb.init()
for x in range(wandb.config['train']['epochs']):
# train
```

Usando banderas absl

```text
flags.DEFINE_string(‘model’, None, ‘model to run’) # name, default, help
wandb.config.update(flags.FLAGS) # adds all absl flags to config
```

Banderas argparse

```text
wandb.init()
wandb.config.epochs = 4

parser = argparse.ArgumentParser()
parser.add_argument('-b', '--batch-size', type=int, default=8, metavar='N',
help='input batch size for training (default: 8)')
args = parser.parse_args()
wandb.config.update(args)
```

Using TensorFlow flags \(deprecated in tensorflow v2\)

```text
flags = tf.app.flags
flags.DEFINE_string('data_dir', '/tmp/data')
flags.DEFINE_integer('batch_size', 128, 'Batch size.')
wandb.config.update(flags.FLAGS)  # adds all of the tensorflow flags to config
```

**persist**

```python
 | persist()
```

 [\[fuente\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_config.py#L162)

Llama al callback si éste está establecido.

