# Run

## wandb.sdk.wandb\_run

 [\[fuente\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L4)

### Objetos Run

```python
class Run(object)
```

 [\[fuente\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L131)

El objeto run se corresponde con una ejecución simple de tu script, siendo esto típicamente un experimento de ML. Crea un run con wandb.init\(\).            

En el entrenamiento distribuido, utiliza wandb.init\(\) para crear una ejecución por cada proceso, y establece el argumento del grupo para organizar las ejecuciones en un experimento más grande.

Actualmente, existe un objeto Run paralelo en wandb.Api. Con el tiempo, estos dos objetos van a ser mezclados.

**Atributos:**

* `history` _`History`_ - Valores de series de tiempo creados con wandb.log\(\). History puede contener valores escalares, medios avanzados, e incluso gráficos personalizados a través de múltiples pasos.
* `summary` _`Summary`_ - Valores simples establecidos por cada clave de wandb.log\(\). Por defecto, summary es establecido al último valor registrado. Puedes establecer summary de forma manual al mejor valor, como la precisión máxima, en lugar del valor final.

**dir**

```python
 | @property
 | dir()
```

 [\[fuente\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L333)

str: El directorio donde son ubicados todos los archivos asociados con la ejecución.

**config**

```python
 | @property
 | config()
```

 [\[fuente\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L333)

\(`Config`\):Un objeto config \(similar a un diccionario anidado\) de pares de clave-valor asociado con los hiperparámetros de la ejecución.

**name**

```python
 | @property
 | name()
```

[\[fuente\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L351) 

str: el nombre que se muestra de la ejecución. No necesita ser único e idealmente es descriptivo.

**notes**

```python
 | @property
 | notes()
```

 [\[fuente\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L367)

str: notas asociadas con la ejecución. Las notas pueden ser un string de múltiples líneas, y también puede usar markdown o ecuaciones latex dentro de $$ como ${x}

**tags**

```python
 | @property
 | tags() -> Optional[Tuple]
```

 [\[fuente\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L383)

 Tupla\[str\]: etiquetas asociadas con la ejecución

**id**

```python
 | @property
 | id()
```

[\[fuente\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L397)

str: el run\_id asociado con la ejecución

**sweep\_id**

```python
 | @property
 | sweep_id()
```

 [\[fuente\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L402)

\(str, optional\): el id del barrido asociado con la ejecución, o None

**path**

```python
 | @property
 | path()
```

[\[fuente\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L409)

str: la ruta a la ejecución \[entity\]/\[project\]/\[run\_id\]

**start\_time**

```python
 | @property
 | start_time()
```

 [\[fuente\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L418)

int: la marca horaria de tipo unix en segundos, desde cuando comenzó la ejecución

**starting\_step**

```python
 | @property
 | starting_step()
```

[\[source\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L426)

 int: el primer paso de la ejecución

**resumed**

```python
 | @property
 | resumed()
```

 [\[fuente\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L434)

 bool: si la ejecución fue reiniciada o no

**step**

```python
 | @property
 | step()
```

 [\[fuente\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L442)

int: contador de pasos.

Cada vez que llamas a wandb.log\(\), por defecto incrementará el contador de pasos.

**mode**

```python
 | @property
 | mode()
```

[\[fuente\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L455)

Para la compatibilidad con las versiones 0.9.x y anteriores. Con el tiempo va a quedar obsoleto.

**group**

```python
 | @property
 | group()
```

 [\[fuente\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L468)

str: nombre del grupo de W&B asociado con la ejecución.

Establecer un grupo ayuda a organizar las ejecuciones en la Interfaz de Usuario de W&B en una forma razonable.

Si estás haciendo un entrenamiento distribuido, deberías darle el mismo nombre de grupo a todas las ejecuciones en dicho entrenamiento. Si estás realizando una validación cruzada, deberías darle el mismo nombre de grupo a todos los pliegues de dicha validación.

**project**

```python
 | @property
 | project()
```

[\[source\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L487)

 str: nombre del proyecto de W&B asociado con la ejecución

**get\_url**

```python
 | get_url()
```

[\[source\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L491)

Devuelve: \(str, optional\): URL para la ejecución de W&B, o None si la ejecución está fuera de línea

**get\_project\_url**

```python
 | get_project_url()
```

 [\[fuente\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L499)

Devuelve: \(str, optional\): URL para el proyecto de W&B asociado con la ejecución, o None si la ejecución está fuera de línea.

**get\_sweep\_url**

```python
 | get_sweep_url()
```

 [\[fuente\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L507)

Devuelve: \(str, optional\): URL para el barrido asociado con la ejecución, o None si no hay asociado ningún barrido o si la ejecución está fuera de línea.

**url**

```python
 | @property
 | url()
```

[\[fuente\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L516)

str: nombre de la URL de W&B asociada con la ejecución.

**entity**

```python
 | @property
 | entity()
```

 [\[fuente\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L521)

str: nombre de la entidad de W&B asociada con la ejecución. La entidad es un nombre de usuario o un nombre de una organización

**log**

```python
 | log(data, step=None, commit=None, sync=None)
```

 [\[fuente\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L675)

Registra un diccionario en el historial de la ejecución global.

wandb.log puede ser usado para registrar todo, desde escalares a histogramas, medios y gráficos de matplotlib.

El uso más básico es wandb.log\({'train-loss': 0.5, 'accuracy': 0.9}\). Esto va a guardar una fila del historial asociada con la ejecución con train-loss=0.5 y accuracy=0.9. Los valores del historial pueden ser graficados en app.wandb.ai o en un servidor local. Los valores del historial también pueden ser descargados a través de la API de wandb.

Al registrar un valor se van a actualizar todos los valores de la síntesis para cualquier métrica registrada. Los valores de la síntesis van a aparecer en la tabla de las ejecuciones en app.wandb.ai o en un servidor local. Si un valor de la síntesis es establecido manualmente, por ejemplo con wandb.run.summary\["accuracy"\] = 0.9, wandb.log no va a actualizar más de forma automática la precisión de la ejecución.

Los valores registrados no tienen que ser escalares. Es posible registrar cualquier objeto de wandb. Por ejemplo, wandb.log\({"example": wandb.Image\("myimage.jpg"\)}\) va a registrar una imagen de ejemplo que va a ser visualizada en la Interfaz de Usuario de wandb. Mirar [https://docs.wandb.com/library/reference/data\_types](https://docs.wandb.com/library/reference/data_types) para ver todos los diferentes tipos soportados.

El registro de métricas anidadas es alentado y soportado en la API de wandb, de este modo podrías registrar múltiples valores de precisión con wandb.log\({'dataset-1': {'acc': 0.9, 'loss': 0.3} ,'dataset-2': {'acc': 0.8, 'loss': 0.2}}\), y las métricas van a ser organizadas en la Interfaz de Usuario de wandb.

W&B lleva el rastro de un paso global y alienta a registrar de forma conjunta a las métricas relacionadas, así que, por defecto, cada vez que se llama a wandb.log, también se incrementa dicho paso global. Si resulta inapropiado registrar en forma conjunta a las métricas relacionadas, hay que llamar a  wandb.log\({'train-loss': 0.5, commit=False}\) y entonces a wandb.log\({'accuracy': 0.9}\), que es equivalente a llamar a wandb.log\({'train-loss': 0.5, 'accuracy': 0.9}\).

No es deseado que se llame a wandb.log más de unas pocas veces por segundo. Si deseas hacer registros de forma más frecuente, es mejor agregar los datos en el lado del cliente o podrías tener un desempeño degradado.    

 **Argumentos:**

* `row` _dict, optional_ - Un diccionario de objetos python serializables, es decir, str, ints, floats, Tensor, diccionarios, o wandb.data\_types.
* `commit` _boolean, optional_ - Guarda el diccionario de las métricas al servidor de wandb e incrementa el paso. Si está en false, wandb.log sólo actualiza el diccionario de las métricas actuales con el argumento row, y las métricas no van a ser guardadas hasta que wandb.log sea llamado con commit=True.
* `step` _integer, optional_ - El paso global en procesamiento. Esto persiste cualquier paso anterior que no haya sido ingresado, pero la acción predeterminada es no ingresar el paso especificado.
* `sync` _boolean, True_ - Este argumento es obsoleto y actualmente no cambia el comportamiento de wandb.log

**Ejemplos:**

Uso básico

```text
- `wandb.log({'accuracy'` - 0.9, 'epoch': 5})
```

Registro incremental

```text
- `wandb.log({'loss'` - 0.2}, commit=False)
# Somewhere else when I'm ready to report this step:
- `wandb.log({'accuracy'` - 0.8})
```

Histograma

```text
- `wandb.log({"gradients"` - wandb.Histogram(numpy_array_or_sequence)})
```

Imagen

```text
- `wandb.log({"examples"` - [wandb.Image(numpy_array_or_pil, caption="Label")]})
```

Video

```text
- `wandb.log({"video"` - wandb.Video(numpy_array_or_video_path, fps=4,
format="gif")})
```

Gráfico de Matplotlib

```text
- `wandb.log({"chart"` - plt})
```

 Curva PR

```text
- `wandb.log({'pr'` - wandb.plots.precision_recall(y_test, y_probas, labels)})
```

Objeto en 3D

```text
wandb.log({"generated_samples":
[wandb.Object3D(open("sample.obj")),
wandb.Object3D(open("sample.gltf")),
wandb.Object3D(open("sample.glb"))]})
```

para más ejemplos, ve a [https://docs.wandb.com/library/log](https://docs.wandb.com/library/log)

 **Levanta:**

wandb.Error, si es llamado antes que wand.init. ValueError, si se pasan datos inválidos.

**save**

```python
 | save(glob_str: Optional[str] = None, base_path: Optional[str] = None, policy: str = "live")
```

[\[fuente\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L810)

Se asegura de que todos los archivos que se correspondan con glob\_str estén sincronizados con wandb, usando la política especificada.

 **Argumentos:**

* `glob_str string` – una ruta relativa o absoluta a un glob de unix, o una ruta regular. Si esto no está especificado el método es una noop.
* `base_path string` – la ruta base relativa a la ejecución del glob
* `policy string` – aceptando los valores “live”, “now” o “end”
* `live` – sube el archivo a medida que éste cambia, sobrescribiendo la versión previa: ahora sube el archivo una vez
* `end` – sólo sube archivos cuando la ejecución finaliza.

**finish**

```python
 | finish(exit_code=None)
```

 [\[fuente\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L906)

Marca a una ejecución como finalizada, y termina de subir todos los datos. Es usado cuando se crean múltiples ejecuciones en el mismo proceso. Llamamos automáticamente a este método cuando tu script termina.

**join**

```python
 | join(exit_code=None)
```

 [\[fuente\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L920)

Alias obsoleto para finish\(\) - por favor, utiliza finish

**plot\_table**

```python
 | plot_table(vega_spec_name, data_table, fields, string_fields=None)
```

 [\[fuente\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L924)

Crea un gráfico personalizado en la tabla

 **Argumentos:**

* `vega_spec_name` - el nombre de la especificación para el gráfico
* `table_key` - la clave utilizada para registrar la tabla de datos
* `data_table` - un objeto wandb.Table que contiene los datos que van a ser usados en la visualización
* `fields` - un diccionario que mapea las claves de la tabla a los campos que necesita la visualización personalizada
* `string_fields` - un diccionario que provee valores para cualquier constante de tipo string que necesite la visualización personalizada

**use\_artifact**

```python
 | use_artifact(artifact_or_name, type=None, aliases=None)
```

[\[fuente\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L1566)

 Declara un artefacto como una entrada para una ejecución, llama a `download` o a `file` sobre el objeto devuelto para obtener los contenidos localmente

**Argumentos:**

* `artifact_or_name` str o Artifact – Un nombre de un artefacto. Puede estar prefijado con entity/project. Los nombres validos pueden tener los siguientes formatos:

  name:version

  name:alias

  digest

  También puedes pasar un objeto Artifact creado al llamar a wandb.Artifact

*  `type str`, `optional` – El tipo de artefacto que se va a usar.
* `aliases lista`, `optional` – Alias que se aplican al artefacto

 **Un objeto Artifact**

A `Artifact` object.

**log\_artifact**

```python
 | log_artifact(artifact_or_path, name=None, type=None, aliases=None)
```

 [\[fuente\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L1621)

Declara un artefacto como la salida de una ejecución.

 **Argumentos:**

* artifact\_or\_path str o Artifact – Una ruta a los contenidos de este artefacto, puede ser de las siguientes formas:

  /local/directory

  /local/directory/file.txt

  s3://bucket/path

  También puedes pasar un objeto Artifact creado al llamar a wandb.Artifact.

* name str, optional – Un nombre de un artefacto. Puede estar prefijado con entity/project. Los nombres válidos pueden ser de las siguientes formas:

  name:version

  name:alias

  digest

  El valor por defecto será el nombre de la ruta antepuesto con el id de la ejecución actual, si no está especificado.

* type str – El tipo del artefacto a registrar, los ejemplos incluyen “dataset”, ”model
* aliases lista, optional – Alias para aplicar a este artefacto, el valor predeterminado es \[“latest\]

 **Devuelve:**

A `Artifact` object.

 **Un objeto Artifact.**

```python
 | alert(title, text, level=None, wait_duration=None)
```

 [\[fuente\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L1675)

Lanza una alerta con el título y el texto dados.

  **Argumentos:**

* `title` _str_ - El título de la alerta, debe tener menos de 64 caracteres de largo
* `text` _str_ - El cuerpo de texto de la alerta
* `level` _str or wandb.AlertLevel, optional_ - El nivel de la alerta que se va a usar, puede ser: “INFO”, “WARN” o “ERROR”
* `wait_duration` _int, float, or timedelta, optional_ - El tiempo que hay que esperar \(en segundos\) antes de enviar otra alerta con este título

