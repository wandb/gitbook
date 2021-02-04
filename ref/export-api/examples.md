---
description: >-
  Aquí hay algunos casos de uso comunes para bajar datos desde W&B utilizando
  nuestra API de Pyhton.
---

# Data Export API Examples

### Encuentra la ruta de la ejecución

Para usar la API pública, a menudo necesitarás la Ruta de la Ejecución, que es "//". En la aplicación, abre una ejecución y haz click en la pestaña Overview para ver la ruta de la ejecución, para cualquier ejecución.

###  Lee las métricas de una ejecución

Este ejemplo imprime la marca horaria y la precisión guardadas con `wandb.log({"accuracy": acc})` para una ejecución guardada en //.

```python
import wandb
api = wandb.Api()

run = api.run("<entity>/<project>/<run_id>")
if run.state == "finished":
   for i, row in run.history().iterrows():
      print(row["_timestamp"], row["accuracy"])
```

### Compara a dos ejecuciones

Esto sacará los parámetros de configuración que sean diferentes entre la run1 y la run2.

```python
import wandb
api = wandb.Api()

# replace with your <entity_name>/<project_name>/<run_id>
run1 = api.run("<entity>/<project>/<run_id>")
run2 = api.run("<entity>/<project>/<run_id>")

import pandas as pd
df = pd.DataFrame([run1.config, run2.config]).transpose()

df.columns = [run1.name, run2.name]
print(df[df[run1.name] != df[run2.name]])
```

 Salidas:

```text
              c_10_sgd_0.025_0.01_long_switch base_adam_4_conv_2fc
batch_size                                 32                   16
n_conv_layers                               5                    4
optimizer                             rmsprop                 adam
```

### Actualiza las métricas para una ejecución \(después de que la misma haya finalizado\)

Este ejemplo establece la precisión de una ejecución previa a 0.9. También modifica el histograma de las precisiones de una ejecución previa para que sea el histograma de numpy\_array

```python
import wandb
api = wandb.Api()

run = api.run("<entity>/<project>/<run_id>")
run.summary["accuracy"] = 0.9
run.summary["accuracy_histogram"] = wandb.Histogram(numpy_array)
run.summary.update()
```

###  Actualiza la configuración en una ejecución

Este ejemplo actualiza uno de los ajustes de la configuración

```python
import wandb
api = wandb.Api()
run = api.run("<entity>/<project>/<run_id>")
run.config["key"] = 10
run.update()
```

### Exporta las métricas desde una ejecución simple a un archivo CSV

Este script encuentra todas las métricas guardadas para una ejecución simple, y las guarda a un CSV.

```python
import wandb
api = wandb.Api()

# run is specified by <entity>/<project>/<run id>
run = api.run("<entity>/<project>/<run_id>")

# save the metrics for the run to a csv file
metrics_dataframe = run.history()
metrics_dataframe.to_csv("metrics.csv")
```

### Exporta las métricas desde una ejecución simple grande sin muestreos

El método history, por defecto, muestrea las métricas a un número fijo de muestras \(el valor predeterminado es 500, pero puedes cambiarlo con el argumento samples\). Si deseas exportar todos los datos que hay de una ejecución grande, puedes usar el método run.scan\_history\(\). Este script carga todas las métricas de la pérdida en una variable llamada losses para una ejecución más grande.

```python
import wandb
api = wandb.Api()

run = api.run("<entity>/<project>/<run_id>")
history = run.scan_history()
losses = [row["Loss"] for row in history]
```

### Exporta las métricas de todas las ejecuciones correspondientes a un proyecto en un archivo CSV

Este script encuentra un proyecto y produce un CSV de sus ejecuciones con nombre, configuraciones y estadísticas de la síntesis.

```python
import wandb
api = wandb.Api()

runs = api.runs("<entity>/<project>")
summary_list = [] 
config_list = [] 
name_list = [] 
for run in runs: 
    # run.summary are the output key/values like accuracy.  We call ._json_dict to omit large files 
    summary_list.append(run.summary._json_dict) 

    # run.config is the input metrics.  We remove special values that start with _.
    config_list.append({k:v for k,v in run.config.items() if not k.startswith('_')}) 

    # run.name is the name of the run.
    name_list.append(run.name)       

import pandas as pd 
summary_df = pd.DataFrame.from_records(summary_list) 
config_df = pd.DataFrame.from_records(config_list) 
name_df = pd.DataFrame({'name': name_list}) 
all_df = pd.concat([name_df, config_df,summary_df], axis=1)

all_df.to_csv("project.csv")
```

### Descarga un archivo desde una ejecución

Encuentra al archivo “model-best.h5”, que está asociado con el ID de la ejecución uxte44z7 en el proyecto cifar, y lo guarda localmente.

```python
import wandb
api = wandb.Api()
run = api.run("<entity>/<project>/<run_id>")
run.file("model-best.h5").download()
```

### Descarga todos los archivos de una ejecución

Encuentra todos los archivos asociados con el ID de la ejecución uxte44z7 y los guarda localmente. \(Nota: también puedes conseguir esto al ejecutar wandb restore  desde la línea de comandos\).

```python
import wandb
api = wandb.Api()
run = api.run("<entity>/<project>/<run_id>")
for file in run.files():
    file.download()
```

### Descarga el archivo del mejor modelo

```python
import wandb
api = wandb.Api()
sweep = api.sweep("<entity>/<project>/<sweep_id>")
runs = sorted(sweep.runs, key=lambda run: run.summary.get("val_acc", 0), reverse=True)
val_acc = runs[0].summary.get("val_acc", 0)
print(f"Best run {runs[0].name} with {val_acc}% validation accuracy")
runs[0].file("model-best.h5").download(replace=True)
print("Best model saved to model-best.h5")
```

###   Descarga los datos de las métricas del sistema

```python
import wandb
api = wandb.Api()
sweep = api.sweep("<entity>/<project>/<sweep_id>")
print(sweep.runs)
```

### Download system metrics data

Esto te da un dataframe con todas las métricas de tu sistema para una ejecución.

```python
import wandb
api = wandb.Api()
run = api.run("<entity>/<project>/<run_id>")
system_metrics = run.history(stream = 'events')
```

### Actualiza las métricas de la síntesis

Puedes pasar tu diccionario para actualizar las métricas de la síntesis.

```python
summary.update({“key”: val})
```

###  Obtén el comando que corrió la ejecución

Cada ejecución captura al comando que la lanzó en la página del resumen de la ejecución. Para bajar el comando desde la API, puedes ejecutar:

```python
api = wandb.Api()
run = api.run("username/project/run_id")
meta = json.load(run.file("wandb-metadata.json").download())
program = ["python"] + [meta["program"]] + meta["args"]
```

###  Obtén los datos paginados del historial

 Si las métricas se están trayendo muy lentamente desde nuestro backend, o si las solicitudes a la API están excediendo el tiempo permitido, puedes intentar reducir el tamaño de la página en `scan_history`, de esta forma las solicitudes individuales no van a expirar. El tamaño por defecto de la página es 1000, así que puedes experimentar con diferentes tamaños para ver que es lo que funciona mejor:

```python
api = wandb.Api()
run = api.run("username/project/run_id")
run.scan_history(keys=sorted(cols), page_size=100)
```



