---
description: Ejecuta barridos desde las notebooks de Jupyter
---

# Sweep from Jupyter Notebook

##  Inicializa un barrido

```python
import wandb

sweep_config = {
  "name": "My Sweep",
  "method": "grid",
  "parameters": {
        "param1": {
            "values": [1, 2, 3]
        }
    }
}

sweep_id = wandb.sweep(sweep_config)
```

{% hint style="danger" %}
**Utiliza los siguientes métodos para especificar la entidad o el proyecto para el barrido:** 

* Argumentos para wandb.sweep\(\). Por ejemplo,`wandb.sweep(sweep_config, entity="user", project="my_project")`
* [Variables de Entorno](https://docs.wandb.ai/library/environment-variables) WANDB\_ENTITY y WANDB\_PROJECT
* [Interfaz de la Línea de Comandos](https://docs.wandb.ai/library/cli) utilizando wandb init command
* [Configuración del barrido](https://docs.wandb.ai/sweeps/configuration) utilizando las claves “entity” y “project”
{% endhint %}

## Corre un agente

Cuando corres un agente desde python, el agente ejecuta una función especificada, en lugar de usar la clave program del archivo de configuración del barrido.

```python
import wandb
import time

def train():
    run = wandb.init()
    print("config:", dict(run.config))
    for epoch in range(35):
        print("running", epoch)
        wandb.log({"metric": run.config.param1, "epoch": epoch})
        time.sleep(1)

wandb.agent(sweep_id, function=train)
```

* Reseña rápida: [Ejecuta en colab](https://github.com/wandb/examples/blob/master/examples/wandb-sweeps/sweeps-python/notebook.ipynb)
*  Guía completa del uso de los barridos en un proyecto: [Ejecuta en colab](https://colab.research.google.com/drive/181GCGp36_75C2zm7WLxr9U2QjMXXoibt)

### wandb.agent\(\)

{% hint style="danger" %}
Al usar wandb.agent\(\) con los valores de entorno de la notebook de jupyter, las cosas se pueden colgar cuando se usan las GPUs.Puede haber una mala interacción entre wandb.agent\(\) y los valores de entorno de jupyter, debido a cómo los frameworks inicializan los recursos de GPU/CUDA.Una solución alternativa temporal \(hasta que podamos arreglar estas interacciones\) es evitar el uso de la interfaz de python para correr al agente. En su lugar, utiliza la interfaz de la línea de comandos estableciendo la clave program en la configuración del barrido, y ejecuta: !wandb agent SWEEP\_ID en tu notebook. 
{% endhint %}

**Argumentos:**

* **sweep\_id \(dict\):** ID del barrido generada por la interfaz de usuario, la línea de comandos o la API del barrido
* **entity \(str, optional\):** nombre de usuario o equipo donde quieres enviar las ejecuciones
* **project \(str, optional\):** proyecto donde quieres enviar las ejecuciones
* **function \(dir, optional\):** configura la función de barrido

## Corre un controlador local

Si deseas desarrollar tus propios algoritmos de búsqueda de parámetros, puedes correr tu controlador desde python.

La forma más simple de ejecutar un controlador:

```python
sweep = wandb.controller(sweep_id)
sweep.run()
```

Si deseas más control sobre el ciclo del controlador:

```python
import wandb
sweep = wandb.controller(sweep_id)
while not sweep.done():
    sweep.print_status()
    sweep.step()
    time.sleep(5)
```

O incluso más control sobre los parámetros que están siendo servidos:

```python
import wandb
sweep = wandb.controller(sweep_id)
while not sweep.done():
    params = sweep.search()
    sweep.schedule(params)
    sweep.print_status()
```

Si quieres especificar por completo a todo tu barrido desde el código, puedes hacer algo como esto:

```python
import wandb
from wandb.sweeps import GridSearch,RandomSearch,BayesianSearch

sweep = wandb.controller()
sweep.configure_search(GridSearch)
sweep.configure_program('train-dummy.py')
sweep.configure_controller(type="local")
sweep.configure_parameter('param1', value=3)
sweep.create()
sweep.run()
```

