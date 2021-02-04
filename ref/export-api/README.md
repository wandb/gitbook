# Data Import/Export API

Exporta un dataframe para análisis personalizados, o agrega datos de forma asincrónica a una ejecución completada. Para más detalles, mirar la [Referencia de la API](https://docs.wandb.ai/ref/export-api/api).

###  Autenticación

Autentica tu máquina con tu [clave de la API](https://wandb.ai/authorize) en una de estas dos formas:

1.  Ejecuta wandb login en la línea de comandos y pega tu clave de la API.
2. Establece la variable de entorno **WANDB\_API\_KEY** con el valor de tu clave de la API.

### Exportar Datos de la Ejecución

Descargas los datos de una ejecución finalizada o de una activa. Los usos comunes incluyen descargar un dataframe para un análisis personalizado en una notebook de Jupyter, o usar lógica personalizada en un entorno automatizado.

```python
import wandb
api = wandb.Api()
run = api.run("<entity>/<project>/<run_id>")
```

Los atributos más comúnmente utilizados de un objeto run son:

| Atributo | Significado |
| :--- | :--- |
| run.config | Un diccionario para las entradas del modelo, tal como los hiperparámetros |
| run.history\(\) | Una lista de diccionarios pensada para almacenar valores que cambian mientras el modelo se está entrenando, tal como la pérdida. El comando wandb.log\(\) adjunta su salida a este objeto. |
| run.summary | Un diccionario de salidas. Pueden ser escalares, como la precisión y la pérdida, o archivos grandes. Por defecto, wandb.log\(\) establece la síntesis al valor final de la serie de tiempos registrados. También puede ser establecido de forma directa. |

También puedes modificar o actualizar los datos de las ejecuciones pasadas. Por defecto, una instancia simple de un objeto api va a almacenar todas las solicitudes de la red. Si tu caso de uso requiere información en tiempo real en un script que está corriendo, llama a api.flush\(\) para obtener valores actualizados.

### Muestreo

El método history, por defecto, muestrea las métricas con un número fijo de muestras \(el valor predeterminado es 500, puedes cambiarlo con el argumento samples\). Si quieres exportar todos los datos de una ejecución grande, puedes usar el método run.scan\_history\(\). Para más detalles mira la [Referencia de la API](https://docs.wandb.ai/ref/export-api/api).

### Consultando a Múltiples Ejecuciones

{% tabs %}
{% tab title="Estilo de MongoDB" %}
La API de W&B también provee una forma para que consultes entre las ejecuciones de un proyecto con api.runs\(\). El caso de uso más común es para exportar datos de las ejecuciones para un análisis personalizado. La interfaz para las consultas es la misma que la que [usa MongoDB](https://docs.mongodb.com/manual/reference/operator/query).

```python
runs = api.runs("username/project", {"$or": [{"config.experiment_name": "foo"}, {"config.experiment_name": "bar"}]})
print("Found %i" % len(runs))
```
{% endtab %}

{% tab title="Dataframes and CSVs" %}
Dataframes y CSVs

```python
import wandb
api = wandb.Api()

# Change oreilly-class/cifar to <entity/project-name>
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
{% endtab %}
{% endtabs %}

 Este script de ejemplo encuentra un proyecto y produce un CSV de la ejecuciones con el nombre, las configuraciones y las estadísticas de la síntesis.

Llamar a `api.runs(…)` devuelve un objeto Runs que es iterable y actúa como una lista. El objeto carga 50 ejecuciones a la vez de forma secuencial según sea necesario, puedes cambiar el número cargado por página con el argumento de palabra clave per\_page.

`api.runs(…)` también acepta un argumento de palabra clave order. El valor predeterminado de order es -created\_at, especifica +created\_at para obtener resultados en orden ascendente. También lo puedes ordenar por valores de configuración o de síntesis, es decir, summary.val\_acc o config.experiment\_name.

###  Manejo de Errores

Si ocurren errores mientras se está hablando con los servidores de W&B, se leveantará un `wandb.CommError`. La excepción original puede ser obtenida por introspección a través del atributo exc.

###  Obtén el último git commit a través de la API

. En la Interfaz de Usuario, haz click en una ejecución y entonces haz click en la pestaña Overview, en la página de la ejecución, para ver al último git commit. También está en el archivo `wandb-metadata.json`. Al utilizar la API pública, puedes obtener el git hash con run.commit.

##  Preguntas Comunes

### Exportar datos para visualizar en matplotlib o en seaborn

 Revisa los [ejemplos de nuestra API](https://docs.wandb.ai/ref/export-api/examples) para encontrar algunos patrones de exportación comunes. También puedes hacer click en el botón de descarga sobre un diagrama personalizado, o sobre la tabla expandida de las ejecuciones, para descargar un CSV desde tu navegador.

### Obtén el ID y el nombre aleatorios de la ejecución desde tu script

 Después de llamar a `wandb.init()`, puedes acceder al ID aleatorio de la ejecución o a su nombre, legible por un ser humano y también aleatorio, desde tu script, de la siguiente forma:

*  ID único de la ejecución \(hash de 8 caracteres\): `wandb.run.id`
* Nombre aleatorio de la ejecución \(legible por el ser humano\): `wandb.run.name`

 Si estás pensando acerca de las formas de establecer identificadores útiles para tus ejecuciones, aquí está lo que recomendamos:

* ID de la ejecución: Déjalo como el hash generado. Necesita ser único respecto a todas las ejecuciones en tu proyecto.
*  Nombre de la ejecución: Debería ser algo corto, legible, y preferiblemente único, para que puedas notar la diferencia entre las diferentes líneas de tu gráfico.
* Notas de la ejecución: Es un lugar magnífico para poner una descripción rápida de lo que estás haciendo en la ejecución. Puedes establecerlo con `wandb.init(notes="your notes here")`.
* Etiquetas de las ejecuciones: Haz el seguimiento dinámico de las cosas usando las etiquetas de tus ejecuciones, y utiliza los filtros de la Interfaz de Usuario para filtrar la tabla, para que sólo queden las ejecuciones que te interesan. Puedes establecer las etiquetas desde tu script y entonces editarlas en la Interfaz de Usuario, tanto en la tabla de las ejecuciones como en la pestaña overview de la página de las ejecuciones.

