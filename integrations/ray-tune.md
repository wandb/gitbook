# Ray Tune

W&B se integra con [Ray](https://github.com/ray-project/ray) al ofrecer dos integraciones livianas.

Una es WandbLogger, que automáticamente registra métricas reportadas a Tune para la API de Wandb. La otra es el decorador `@wandb_mixin`, que puede ser usado con la API Function. Automáticamente, inicializa la API de Wandb con la información de entrenamiento de Tune. Puedes usar solamente la API de Wandb como lo harías normalmente, por ejemplo usando `wandb.log()` para registrar tu proceso de entrenamiento.

## WandbLogger

```python
from ray.tune.integration.wandb import WandbLogger
```

La configuración de Wandb es realizada al pasar una clave de wandb al parámetro config de tune.run\(\) \(ver el ejemplo de abajo\).

El contenido de la entrada config de wandb es pasado a wandb.init\(\) como argumentos de palabras claves. Las excepciones son los siguientes ajustes, que son usados para configurar al WandbLogger mismo:

### Parámetros

`api_key_file (str)` – Ruta al archivo que contiene la CLAVE de la API de Wandb.

`api_key (str)` – Clave de la API de Wanndb. Alternativa al ajuste `api_key_file`.

`excludes (list)` – Lista de métricas que deberían ser excluidas del registro.

`log_config (bool)` – Booleano que indica si el parámetro config del diccionario de resultados debería ser registrado. Esto tiene sentido si los parámetros van a cambiar durante el entrenamiento, por ejemplo con PopulationBasedTraining. Por defecto es False.

### Ejemplo

```python
from ray.tune.logger import DEFAULT_LOGGERS
from ray.tune.integration.wandb import WandbLogger
tune.run(
    train_fn,
    config={
        # define search space here
        "parameter_1": tune.choice([1, 2, 3]),
        "parameter_2": tune.choice([4, 5, 6]),
        # wandb configuration
        "wandb": {
            "project": "Optimization_Project",
            "api_key_file": "/path/to/file",
            "log_config": True
        }
    },
    loggers=DEFAULT_LOGGERS + (WandbLogger, ))
```

## wandb\_mixin

```python
ray.tune.integration.wandb.wandb_mixin(func)
```

Este mixin de Ray Tune Trainable ayuda a inicializar la API de Wandb para usarla con la clase `Trainable` o con `@wandb_mixin` para la API Function.

Para su uso básico, solamente antepone el decorador `@wandb_mixin` a tu función de entrenamiento:

```python
from ray.tune.integration.wandb import wandb_mixin

@wandb_mixin
def train_fn(config):
    wandb.log()
```

La configuración de `wandb` se hace al pasar la clave de wandb al parámetro `config de tune.run()` \(ver el ejemplo de abajo\).

El contenido de la entrada config de wandb es pasado a `wandb.init()` como argumentos de palabras claves. Las excepciones son los siguientes ajustes, que son utilizados para configurar al `WandbTrainableMixin` mismo:

### Parámetros

`api_key_file (str)` –  Ruta al archivo que contiene la CLAVE de la API de Wandb.

`api_key (str)` – Clave de la API de Wanndb. Alternativa al ajuste `api_key_file`.

`group`, `rund_id` y `run_name` de Wandb son seleccionados automáticamente por Tune, pero pueden ser sobrescritos al completar los respectivos valores de configuración.

Por favor, observa aquí para ver todos los otros ajustes de configuración válidos: [https://docs.wandb.com/library/init](https://docs.wandb.com/library/init)

###  Ejemplo:

```python
from ray import tune
from ray.tune.integration.wandb import wandb_mixin

@wandb_mixin
def train_fn(config):
    for i in range(10):
        loss = self.config["a"] + self.config["b"]
        wandb.log({"loss": loss})
        tune.report(loss=loss)

tune.run(
    train_fn,
    config={
        # define search space here
        "a": tune.choice([1, 2, 3]),
        "b": tune.choice([4, 5, 6]),
        # wandb configuration
        "wandb": {
            "project": "Optimization_Project",
            "api_key_file": "/path/to/file"
        }
    })
```

## Example Code Código de Ejemplo

Hemos creado algunos ejemplos para que veas cómo funciona la integración:

* [Colab](https://colab.research.google.com/drive/1an-cJ5sRSVbzKVRub19TmmE4-8PUWyAi?usp=sharing): Una demostración simple para probar la integración
*  [Tablero de Control](https://app.wandb.ai/authors/rayTune?workspace=user-cayush): Ver el tablero de control generado a partir del ejemplo

