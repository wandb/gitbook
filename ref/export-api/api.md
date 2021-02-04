---
description: wandb.apis.public
---

# Public API Reference

## wandb.apis.public

 [\[fuente\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1)

###  Objetos Api

```python
class Api(object)
```

 [\[fuente\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L181)

 Usados para consultar al servidor de wandb.

 **Ejemplos:**

 La forma más común de inicializar

```text
wandb.Api()
```

 **Argumentos:**

* `overrides` _dict_ -Puedes establecer `base_url` si estás usando un servidor de wandb distinto a [https://api.wandb.ai](https://api.wandb.ai/).

  También puedes establecer los valores predeterminados `para entity`, `project` y run.

**flush**

```python
 | flush()
```

 [\[fuen](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L276)[te](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L276)[\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L276)

Este objeto de la api mantiene un caché local de las ejecuciones, así que si el estado de la ejecución puede cambiar mientras se ejecuta tu script, debes limpiar el caché con `api.flush()` para obtener los últimos valores asociados con la ejecución.

**projects**

```python
 | projects(entity=None, per_page=200)
```

 [\[fuente\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L338)

Obtiene los proyectos para una entidad dada.

**Argumentos:**

* `entity str` – Nombre de la entidad solicitada. Si es None, va a tomar como opción alternativa a la entidad por defecto pasada a Api. Si no hay ninguna entidad por defecto, levantará un ValueError.
* `per_page int` – Establece el tamaño de la página para la paginación de consultas. None va a usar el valor por defecto. Por lo general, no hay razón para cambiarlo.

 **Devuelve:**

Un objeto `Projects` que es una colección iterable de objetos `Project`.

**reports**

```python
 | reports(path="", name=None, per_page=50)
```

 [\[fuente\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L360)

Obtiene reportes para una ruta dada de un proyecto.

AVISO: Esta api está en estado beta y probablemente cambie en una entrega futura.

**Argumentos:**

* `path str` – ruta al proyecto en el que reside el reporte, debería estar en la forma: “entity/project”.
* `name str` – nombre opcional del reporte solicitado.
* `per_page int` – Establece el tamaño de la página para la paginación de las consultas. None va a usar el valor por defecto. Por lo general, no hay razón para cambiarlo.

Devuelve:

Un objeto Reports que es una colección iterable de objetos `BetaReport`.

**runs**

```python
 | runs(path="", filters={}, order="-created_at", per_page=50)
```

 [\[fuente\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L393)

Return a set of runs from a project that match the filters provided. You can filter by `config.*`, `summary.*`, `state`, `entity`, `createdAt`, etc. Devuelve un conjunto de ejecuciones desde un proyecto que se corresponde con los filtros provistos. Puedes filtrar por `config.`_, `summary.`_,  `state`, `entity`, createdAt, etc.

**Ejemplos:**

Encuentra las ejecuciones en my\_project config.experiment\_name que hayan sido establecidas a “foo”

```text
api.runs(path="my_entity/my_project", {"config.experiment_name": "foo"})
```

Encuentra las ejecuciones en my\_project config.experiment\_name que hayan sido establecidas a “foo” o a “bar”

```text
api.runs(path="my_entity/my_project",
- `{"$or"` - [{"config.experiment_name": "foo"}, {"config.experiment_name": "bar"}]})
```

Encuentra las ejecuciones en my\_project ordenadas de forma ascendente por pérdida

```text
api.runs(path="my_entity/my_project", {"order": "+summary_metrics.loss"})
```

Argumentos:

* path str – Ruta al proyecto, debería estar en la forma: “entity/project”.
* filters diccionario – Consulta por ejecuciones específicas, utilizando el lenguaje de consultas de MongoDB.

Puedes filtrar por las propiedades de la ejecución, tales como config.key, summary\_metrics.key, state, entity, createdAt, etc. Por ejemplo:{"config.experiment\_name": "foo"} encontraría ejecuciones con una entrada de configuración con un nombre de experimento establecido a “foo”.

Puedes componer operaciones para hacer consultas más complicadas, mira la Referencia para el lenguaje en https://docs.mongodb.com/manual/reference/operator/query

* order str – order puede ser created\_at, heartbeat\_at, config.\*.value, or summary\_metrics.\*.

Si antepones a order con un +, el orden es ascendente.

Si antepones a order con un -, el orden es de descendente.

El valor predeterminado de order es run.created\_at, desde el más nuevo al más viejo.

**Devuelve:**

Un objeto `Runs`, que es una colección iterable de objetos `Run`.

**run**

```python
 | @normalize_exceptions
 | run(path="")
```

[\[fuente\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L445)

Devuelve una ejecución simple al analizar la ruta en la forma de entity/project/run\_id..

**Argumentos:**

* path str – ruta a la ejecución en la forma de entity/project/run\_id.

Si está establecido api.entity, puede estar en la forma project/run\_id, y si api.project también está establecido puede estar en la forma run\_id.   

**Returns**:

A `Run` object.

**sweep**

```python
 | @normalize_exceptions
 | sweep(path="")
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L462)

Devuelve un barrido al analizar la ruta en la forma de entity/project/sweep\_id.

**Argumentos:**

* path str – ruta a la ejecución en la forma de entity/project/run\_id.

Si está establecido api.entity, puede estar en la forma project/run\_id, y si api.project también está establecido puede estar en la forma run\_id.   

**Devuelve:**

 Un objeto `Sweep`.

**artifact**

```python
 | @normalize_exceptions
 | artifact(name, type=None)
```

 [\[fuente\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L496)

Devuelve un artefacto simple al analizar la ruta en la forma de entity/project/run\_id.

**Argumentos:**

* `name` _str_ - Un nombre de un artefacto. Puede estar prefijado con entity/project. Los nombres válidos pueden estar en las siguientes formas:

  name:version

  name:alias

  digest

* `type` _str, optional_ - El tipo del artefacto que hay que buscar.

 **Devuelve:**

Un objeto `Artifact`.

###  Objetos Projects

```python
class Projects(Paginator)
```

 [\[fuente\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L616)

Una colección iterable de objetos `Project`

###  Objetos Project

```python
class Project(Attrs)
```

[\[fuente\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L678)

Un proyecto es un namespace para las ejecuciones.

### Objetos Runs

```python
class Runs(Paginator)
```

[\[fuente\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L699)

Una colección iterable de ejecuciones asociadas con un proyecto y con un filtro opcional. Por lo general, es usado indirectamente a través del método .runs de Api.

### Objetos Run

```python
class Run(Attrs)
```

[\[fuente\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L804)

 Una ejecución simple asociada con una entidad y un proyecto.

**Atributos:**

* tags \[str\] – una lista de etiquetas asociada con la ejecución
* url str – la url de esta ejecución
*  id str – identificador único para la ejecución \(el valor predeterminado tiene ocho caracteres\)
* name str – el nombre de la ejecución
* state str – uno de los siguientes: running, finished, crashed, aborted
* config diccionario – un diccionario de hiperparámetros asociados con la ejecución
*  created\_at str – marca horaria ISO de cuando la ejecución fue iniciada
*  system\_metrics diccionario – las últimas métricas del sistema registradas para la ejecución
* summary diccionario – una propiedad de tipo diccionario mutable que mantiene la síntesis actual. Al llamar a la actualización persistirá cualquier cambio.
* project str – el proyecto asociado con la ejecución
* entity str – el nombre de la entidad asociado con la ejecución
* user str – el nombre del usuario que creó la ejecución
* path str – identificador único en la forma \[entity\]/\[project\]/\[run\_id\]
* notes str – notas acerca de la ejecución
* read\_only booleano – Si la ejecución es editable
*  history\_keys str – Claves de las métricas del historial que han sido registradas con wandb.log\({key: value}\)

**\_\_init\_\_**

```python
 | __init__(client, entity, project, run_id, attrs={})
```

 [\[fuente\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L829)

Las ejecuciones siempre son inicializadas al llamar a api.runs\(\), donde api es una instancia de wandb.Api.

**create**

```python
 | @classmethod
 | create(cls, api, run_id=None, project=None, entity=None)
```

 [\[fuente\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L829)

Las ejecuciones siempre son inicializadas al llamar a api.runs\(\), donde api es una instancia de wandb.Api.

**update**

```python
 | @normalize_exceptions
 | update()
```

 [\[fuente\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L887)

Crea una ejecución para un proyecto dado.

**files**

```python
 | @normalize_exceptions
 | files(names=[], per_page=50)
```

 [\[fuente\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1070)

**Argumentos:**

* names lista – nombres de los archivos solicitados, si está vacía devuelve todos los archivos
*  per\_page int – número de resultados por página

**Devuelve:**

* Un objeto Files, que es un iterador sobre los objetos File.

**file**

```python
 | @normalize_exceptions
 | file(name)
```

 [\[fuente\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1082)

**Argumentos:**

* `name` _str_ -  nombre del archivo solicitado.

 **Devuelve:**

Un `File` que se corresponde con el argumento name.

**upload\_file**

```python
 | @normalize_exceptions
 | upload_file(path, root=".")
```

 [\[fuente\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1093)

**Argumentos:** 

* path str – nombre de archivo que se va a subir.
*  root str – la ruta raíz relativa en la cual se va a guardar el archivo. Es decir, si quieres guardar el archivo en la ejecución como “my\_dir/file.txt”, y actualmente estás posicionado en “my\_dir”, entonces deberías establecer la raíz a “../”.

**Devuelve:**

Un `File` que se corresponde con el argumento name.

**history**

```python
 | @normalize_exceptions
 | history(samples=500, keys=None, x_axis="_step", pandas=True, stream="default")
```

 [\[fuente\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1116)

Devuelve métricas muestreadas del historial para una ejecución. Es lo más simple y lo más rápido si estás de acuerdo en que lo registros del historial sean muestreados.

 **Argumentos:**

* samples int, optional – El número de muestras que se van a devolver
*  pandas booleano, optional – Devuelve un dataframe de pandas
* keys lista, optional – Sólo devuelve las métricas para claves específicas
*  x\_axis str, optional – Usa esta métrica como un valor predeterminado de xAxis para \_step
* stream str, optional - “default” para métricas, “system” para métricas de la máquina

 **Devuelve:**

. Si pandas=True, devuelve un `pandas.DataFrame` de las métricas del historial. Si pandas=False, devuelve una lista de diccionarios de las métricas del historial.

**scan\_history**

```python
 | @normalize_exceptions
 | scan_history(keys=None, page_size=1000, min_step=None, max_step=None)
```

[\[fuente\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1150)

Devuelve una colección iterable de todos los registros del historial para una ejecución.

**Ejemplo:**

Exporta todos los valores de la pérdida para una ejecución de ejemplo

```python
run = api.run("l2k2/examples-numpy-boston/i0wt6xua")
history = run.scan_history(keys=["Loss"])
losses = [row["Loss"] for row in history]
```

**Argumentos:**

* keys \[str\], optional – solamente busca estas claves, y solamente busca las filas que tengan a todas las claves definidas.
*  page\_size int, optional – tamaño de las páginas para las búsquedas desde la api

**Devuelve:**

Una colección iterable sobre los registros del historial \(diccionario\).

**use\_artifact**

```python
 | @normalize_exceptions
 | use_artifact(artifact)
```

[\[fuente\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1207)

Declara un artefacto como una entrada para una ejecución.

**Arguments**:

* `artifact` _`Artifact`_ - Un artefacto devuelto desde

  `wandb.Api().artifact(name)`

 **Devuelve:**

 Un objeto `Artifact`.

**log\_artifact**

```python
 | @normalize_exceptions
 | log_artifact(artifact, aliases=None)
```

 [\[fuente\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1234)

Declara a un artefacto como la salida de una ejecución.

Argumentos:

* artifact Artifact – Un artefacto devuelto desde wandb.Api\(\).artifact\(name\)
* aliases lista, optional – Alias que se va a aplicar a este artefacto

 **Devuelve:**

Un objeto `Artifact`.

###  Objetos Sweep

```python
class Sweep(Attrs)
```

 [\[fuente\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1314)

Un conjunto de ejecuciones asociadas con una instancia de barrido con: api.sweep\(sweep\_path\)

 ****Atributos:

* runs Runs – lista de ejecuciones
*  id str – id del barrido
*  project str – nombre del proyecto
* config str – diccionario con la configuración del barrido

**best\_run**

```python
 | best_run(order=None)
```

[\[fuente\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1440)

Ejecuta una consulta contra el backend de la nube

**get**

```python
 | @classmethod
 | get(cls, client, entity=None, project=None, sid=None, withRuns=True, order=None, query=None, **kwargs)
```

[\[fuente\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1495)

Files es una colección iterable de objetos File.

### Objetos File

```python
class Files(Paginator)
```

 [\[fuente\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1561)

Files is an iterable collection of `File` objects.

### File Objects

```python
class File(object)
```

 [\[fuente\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1561)

File es una clase que está asociada con un archivo guardado por wandb.

**Atributos:**

* name string – nombre del archivo
*  url string – ruta al archivo
* md5 string – md5 del archivo
* mimetype string – tipo mime del archivo
* updated\_at string – marca horaria de la última actualización
* size int – tamaño del archivo en bytes

**download**

```python
 | @normalize_exceptions
 | @retriable(
 |         retry_timedelta=RETRY_TIMEDELTA,
 |         check_retry_fn=util.no_retry_auth,
 |         retryable_exceptions=(RetryError, requests.RequestException),
 |     )
 | download(root=".", replace=False)
```

[\[fuente\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1618)

Descarga un archivo desde el servidor de wandb que previamente ha sido guardado por una ejecución.

  
**Argumentos:**

* replace booleano – Si es `True`, la descarga va a sobrescribir al archivo local, si el mismo existe. Su valor predeterminado es `False`.
* root str – Directorio local en el que se va a guardar el archivo. El valor por defecto es “.”.

Levanta:

ValueError si el archivo ya existe y replace=False

### Reports Objects

```python
class Reports(Paginator)
```

[\[fuente\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1641)

Reports es una colección iterable de objetos `BetaReport`.

###  Objetos QueryGenerator

```python
class QueryGenerator(object)
```

[\[fuente\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1721)

QueryGenerator es un objeto auxiliar para escribir filtros para las ejecuciones.

###  Objetos BetaReport

```python
class BetaReport(Attrs)
```

 [\[fuente\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1818)

BetaReport es una clase que esta asociada con los reportes creados en wandb.

AVISO: esta API probablemente cambie en futuras entregas

**Attributes**:

* name string – nombre del reporte
* description string – descripción del reporte
* user User – el usuario que creó el reporte
*  spec diccionario – la especificación del proyecto
*  updated\_at string – marca horaria de la última actualización

###  Objetos ArtifactType

```python
class ArtifactType(object)
```

 [\[fuente\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L2282)

**collections**

```python
 | @normalize_exceptions
 | collections(per_page=50)
```

 [\[fuente\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L2337)

 Colecciones de artefactos

### Objetos ArtifactCollection

```python
class ArtifactCollection(object)
```

 [\[fuente\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L2352)

**versions**

```python
 | @normalize_exceptions
 | versions(per_page=50)
```

 [\[fuente\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L2366)

Versiones del artefacto

### Objetos Artifact

```python
class Artifact(object)
```

 [\[fuente\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L2381)

**delete**

```python
 | delete()
```

 [\[fuente\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L2534)

Borra al artefacto y a sus archivos.

**get**

```python
 | get(name)
```

[\[fuente\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L2628)

Devuelve el recurso wandb.Media almacenado en el artefacto. Los medios puede ser almacenado en el artefacto a través de Artifact\#add\(obj: wandbMedia, name: str\)\`

**Argumentos:**

* name str – nombre del recurso.

**Devuelve:**

Un `wandb.Media` que ha sido almacenado en name.

**download**

```python
 | download(root=None)
```

[\[fuente\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L2663)

Descarga el artefacto al directorio especificado por el

**Argumentos:**

*  root str, optional – directorio en el que se va a descargar el artefacto. Si es None, el artefacto va a ser descargado en ‘./artifacts//

 **Devuelve:**

La ruta a los contenidos descargados.

**file**

```python
 | file(root=None)
```

 [\[fuente\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L2702)

Descarga un artefacto de archivo simple al directorio especificado por el

**Argumentos:**

* `root str,` optional – directorio en el que se va a descargar el artefacto. Si es None, el artefacto va a ser descargado en ‘./artifacts//

**Devuelve:**

La ruta completa al archivo descargado

**save**

```python
 | @normalize_exceptions
 | save()
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L2737)

Persiste los cambios del artefacto en el backend de wandb.

**verify**

```python
 | verify(root=None)
```

 [\[fuente\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L2776)

Verifica un artefacto por la suma de comprobación de sus contenidos descargados.

Levanta un ValueError si falla la verificación. No verifica los archivos de referencia descargados.

**Arguments**:

* `root` _str, optional_ - directorio en el que se va a descargar el artefacto. Si es None, el artefacto va a ser descargado en ‘./artifacts//’

###  Objetos ArtifactVersions

```python
class ArtifactVersions(Paginator)
```

 [\[fuente\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L2930)

Una colección iterable de las versiones del artefacto, asociada con un proyecto y un filtro optativo. En general, es utilizado indirectamente a través del método .artifact\_versions de `Api`

