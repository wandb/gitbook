# Artifacts API

Utiliza los Artefactos de W&B para hacer el seguimiento del conjunto datos y el versionado de los modelos. Inicializa una ejecución, crea un artefacto, y entonces úsalo en otra parte de tu entorno de trabajo. 

Puedes usar artefactos para rastrear y guardar archivos, o para rastrear URIs externas.Esta característica está disponible en el cliente a partir de la versión 0.9.0 de `wandb`.

## 1. Inicializa una ejecución

Para hacer el seguimiento de tu proceso de trabajo, inicializa una ejecución en tu script. Especifica un string para job\_type, para diferenciar diferentes pasos del proceso de trabajo – preprocesamiento, entrenamiento, evaluación, etc. Si nunca instrumentaste una ejecución en W&B, tenemos guías más detalladas para rastrear los experimentos en nuestra documentación de la [**Biblioteca de Python**](https://docs.wandb.ai/library).

```python
run = wandb.init(job_type='train')
```

## 2. Crea un artefacto

Un artefacto es como un directorio de datos, con contenidos que son archivos reales almacenados en el artefacto o referencias a URIs externas. Para crear un artefacto, regístralo como la salida de una ejecución. Especifica un string para type para diferenciar a los diferentes artefactos – conjunto de datos, modelo, resultado, etc. Dale a este artefacto un **nombre**, como bike-dataset, para que te ayude a recordar que es lo que hay adentro del mismo. En un paso posterior de tu proceso de trabajo, puedes usar este nombre, conjuntamente con una versión como bike-dataset:v1, para descargar dicho artefacto.

Cuando llamas a log\_artifact, verificamos para ver si los contenidos del artefacto han cambiado y, de ser así, creamos automáticamente una nueva versión del artefacto: v0, v1, v2, etc.

**wandb.Artifact\(\)**

* **type \(str\)**: Diferencia clases de artefactos, es utilizado con propósitos organizativos. Te recomendamos que te adhieras a “dataset”, “model” y “result”.
* **name \(str\)**: Le da al artefacto un nombre único, es utilizado cuando referencias al artefacto en otro lugar. En el nombre puedes usar números, letras, guiones bajos, guiones y puntos.
* **description \(str, optional\)**: Texto libre visualizado al lado de la versión del artefacto en la interfaz de usuario.
* **metadata \(dict, optional\)**: Datos estructurados que están asociados con el artefacto, por ejemplo la distribución de la clase de un conjunto de datos. A medida que ampliemos la interfaz web, vas a ser capaz de utilizar estos datos para hacer consultas y gráficos.

```python
artifact = wandb.Artifact('bike-dataset', type='dataset')

# Add a file to the artifact's contents
artifact.add_file('bicycle-data.h5')

# Save the artifact version to W&B and mark it as the output of this run
run.log_artifact(artifact)
```

{% hint style="warning" %}
**NOTA:** Las llamadas a log\_artifact son ejecutadas asincrónicamente para las subidas eficientes. Esto puede provocar un comportamiento inesperado cuando registres artefactos en un ciclo. Por ejemplo:

```text
for i in range(10):
    a = wandb.Artifact('race', type='dataset', metadata={
        "index": i,
    })
    # ... add files to artifact a ...
    run.log_artifact(a)
```

O está garantizado que la versión del artefacto **v0** tenga un índice 0 en sus metadatos, dado que los artefactos pueden ser registrados en un orden arbitrario.
{% endhint %}

## 3. Utiliza un artefacto

 Puedes usar un artefacto como entrada para una ejecución. Por ejemplo, podríamos timar a `bike-dataset`:v0, la primera versión de `bike-dataset`, y utilizarla en el próximo script en nuestro proceso de trabajo. Cuando llamas a use\_artifact, tu script consulta a W&B para encontrar a ese artefacto nombrado de esa forma y lo marca como entrada para la ejecución.

```python
# Query W&B for an artifact and mark it as input to this run
artifact = run.use_artifact('bike-dataset:v0')

# Download the artifact's contents
artifact_dir = artifact.download()
```

**Utilizando un artefacto desde un proyecto diferente**  
 Puedes referenciar libremente a los artefactos desde cualquier proyecto al que tengas acceso, al calificar el nombre del artefacto con el nombre de su proyecto. También puedes referenciar a los artefactos a través de las entidades, al calificar además el nombre del artefacto con el nombre de su entidad.

```python
# Query W&B for an artifact from another project and mark it
# as an input to this run.
artifact = run.use_artifact('my-project/bike-model:v0')

# Use an artifact from another entity and mark it as an input
# to this run.
artifact = run.use_artifact('my-entity/my-project/bike-model:v0')
```

**Utilizando un artefacto que no haya sido registrado**  
También puedes construir un objeto artefacto y pasarlo a **use\_artifac**t. Nosotros verificamos si el artefacto ya existe en W&B, y de no ser así crearemos uno nuevo. Esto es idempotente – le pasas un artefacto a use\_artifact tantas veces como quieras, y lo vamos a deduplicar siempre y cuando los contenidos sigan siendo los mismos.

```python
artifact = wandb.Artifact('bike-model', type='model')
artifact.add_file('model.h5')
run.use_artifact(artifact)
```

##  Versiones y alias

Cuando registras un artefacto por primera vez, creamos la versión v0. Cuando registras nuevamente el mismo artefacto, comprobamos la suma de verificación de los contenidos, y si el artefacto ha cambiado guardamos una nueva versión v1.

Puedes usar alias como punteros a versiones específicas. Por defecto, run.log\_artifact agrega el **último** alias a la versión registrada

.Puedes buscar un artefacto usando un alias. Por ejemplo, si quieres que tu script de entrenamiento tome siempre la versión más reciente de un conjunto de datos, especifica latest cuando uses ese artefacto.

```python
artifact = run.use_artifact('bike-dataset:latest')
```

 También puedes aplicar un alias personalizado a la versión de un artefacto. Por ejemplo, si deseas marcar qué punto de control del modelo es el mejor para la métrica AP-50, podrías agregar el string best-ap50 como un alias cuando registras el artefacto del modelo.

```python
artifact = wandb.Artifact('run-3nq3ctyy-bike-model', type='model')
artifact.add_file('model.h5')
run.log_artifact(artifact, aliases=['latest','best-ap50'])
```

## Construyendo artefactos

Un artefacto es como un directorio de datos. Cada entrada es un archivo real almacenado en el artefacto, o una referencia a una URI externa. Puedes anidar directorios dentro de un artefacto tal como lo harías con un sistema de archivos regular. Construye nuevos artefactos al inicializar la clase `wandb.Artifact()`.

 Puedes pasar los siguientes campos al constructor `Artifact()`, o establecerlos directamente en el objeto artefacto:

*  **type: Debería ser ‘dataset’, ‘model’ o ‘result’.. description: Texto en un formato libre que va a ser exhibido en la interfaz de usuario..** 
* **metadata: Un diccionario que puede contener cualquier dato estructurado. Vas a ser capaz de usar estos datos para consultar y hacer gráficos. Por ejemplo, puedes elegir almacenar la distribución de la clase para un artefacto de tipo conjunto de datos como metadatos.**

```python
artifact = wandb.Artifact('bike-dataset', type='dataset')
```

  
Utiliza name para especificar un nombre de archivo opcional, o un prefijo de la ruta del archivo si estás agregando un directorio.

```python
# Add a single file
artifact.add_file(path, name='optional-name')

# Recursively add a directory
artifact.add_dir(path, name='optional-prefix')

# Return a writeable file-like object, stored as <name> in the artifact
with artifact.new_file(name) as f:
    ...  # Write contents into the file 

# Add a URI reference
artifact.add_reference(uri, name='optional-name')
```

### Agregando archivos y directorios

Para los siguientes ejemplos, vamos a asumir que tenemos un directorio del proyecto con estos archivos:

```text
project-directory
|-- images
|   |-- cat.png
|   +-- dog.png
|-- checkpoints
|   +-- model.h5
+-- model.h5
```

<table>
  <thead>
    <tr>
      <th style="text-align:left">Llamada a la API</th>
      <th style="text-align:left">Contenidos del artefacto resultante</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">artifact.add_file(&apos;model.h5&apos;)</td>
      <td style="text-align:left">model.h5</td>
    </tr>
    <tr>
      <td style="text-align:left">artifact.add_file(&apos;checkpoints/model.h5&apos;)</td>
      <td style="text-align:left">model.h5</td>
    </tr>
    <tr>
      <td style="text-align:left">artifact.add_file(&apos;model.h5&apos;, name=&apos;models/mymodel.h5&apos;)</td>
      <td
      style="text-align:left">models/mymodel.h5</td>
    </tr>
    <tr>
      <td style="text-align:left">artifact.add_dir(&apos;images&apos;)</td>
      <td style="text-align:left">
        <p>cat.png</p>
        <p>dog.png</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">artifact.add_dir(&apos;images&apos;, name=&apos;images&apos;)</td>
      <td
      style="text-align:left">
        <p>images/cat.png</p>
        <p>images/dog.png</p>
        </td>
    </tr>
    <tr>
      <td style="text-align:left">artifact.new_file(&apos;hello.txt&apos;)</td>
      <td style="text-align:left">hello.txt</td>
    </tr>
  </tbody>
</table>

###  Agregando referencias

```python
artifact.add_reference(uri, name=None, checksum=True)
```

* **uri \(string\):** La URI de referencia a la que hay que hacerle el seguimiento.
* **name \(string\):** un nombre optativo. Si no es provisto, es inferido un nombre desde la **uri**.
* **checksum \(bool\):** si es true, la referencia recoge la información de la suma de verificación y los metadatos de la uri con propósitos de validación. 

Puedes agregarle a los artefactos referencias a URIs externas, en vez de archivos reales. Si una URI tiene un esquema que wandb sabe cómo manejar, el artefacto rastreará las sumas de verificación y otra información para la reproducibilidad. Los artefactos actualmente soportan los siguientes esquemas de URIs:

* `http(s)://`: Una ruta a un archivo accesible sobre HTTP. El artefacto va a hacer un seguimiento de las sumas de verificación en la forma de etags y metadatos de tamaños, si el servidor soporta las cabeceras de respuesta `ETag` y Content-Length.
* `s3://`: Una ruta a un objeto o a un prefijo de un objeto en S3. El artefacto va a hacer el seguimiento de las sumas de verificación y de la información de la versión \(si el bucket tiene habilitado el versionado de los objetos\) para los objetos referenciados. Los prefijos de los objetos son expandidos para incluir a los objetos bajo el prefijo, hasta un máximo de 10.000 objetos.
* `gs://`: Una ruta a un objeto o a un prefijo de un objeto en GCS. El artefacto va a hacer el seguimiento de las sumas de verificación y de la información de la versión \(si el bucket tiene habilitado el versionado de los objetos\) para los objetos referenciados. Los prefijos de los objetos son expandidos para incluir a los objetos bajo el prefijo, hasta un máximo de 10.000 objetos.

Para los siguientes ejemplos, vamos a asumir que tenemos un bucket S3 con estos archivos:

```text
s3://my-bucket
|-- images
|   |-- cat.png
|   +-- dog.png
|-- checkpoints
|   +-- model.h5
+-- model.h5
```

<table>
  <thead>
    <tr>
      <th style="text-align:left">Llamada a la API</th>
      <th style="text-align:left">Contenidos del artefacto resultantes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">artifact.add_reference(&apos;s3://my-bucket/model.h5&apos;)</td>
      <td style="text-align:left">model.h5</td>
    </tr>
    <tr>
      <td style="text-align:left">artifact.add_reference(&apos;s3://my-bucket/checkpoints/model.h5&apos;)</td>
      <td
      style="text-align:left">model.h5</td>
    </tr>
    <tr>
      <td style="text-align:left">artifact.add_reference(&apos;s3://my-bucket/model.h5&apos;, name=&apos;models/mymodel.h5&apos;)</td>
      <td
      style="text-align:left">models/mymodel.h5</td>
    </tr>
    <tr>
      <td style="text-align:left">artifact.add_reference(&apos;s3://my-bucket/images&apos;)</td>
      <td style="text-align:left">
        <p>cat.png</p>
        <p>dog.png</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">artifact.add_reference(&apos;s3://my-bucket/images&apos;, name=&apos;images&apos;)</td>
      <td
      style="text-align:left">
        <p>images/cat.png</p>
        <p>images/dog.png</p>
        </td>
    </tr>
  </tbody>
</table>

## Usando y descargando artefactos

```python
run.use_artifact(artifact=None)
```

* Marca un artefacto como una entrada para tu ejecución

Existen dos patrones para usar artefactos. Puedes utilizar el nombre de un artefacto que esté explícitamente almacenado en W&B, o puedes construir un objeto artefacto y pasarlo para que sea deduplicado cuando sea necesario.

### Utiliza un artefacto almacenado en W&B

```python
artifact = run.use_artifact('bike-dataset:latest')
```

 Puedes llamar a los siguientes métodos sobre el artefacto devuelto:

```python
datadir = artifact.download(root=None)
```

* Descarga todos los contenidos del artefacto que no estén actualmente presentes. Esto devuelve una ruta a un directorio que contiene los contenidos del artefacto. Puedes especificar de forma explícita el destino de la descarga al establecer root.

```python
path = artifact.get_path(name)
```

* Trae solamente al archivo de la ruta `name`. Devuelve un objeto `Entry` con los siguientes métodos:
  * **Entry.download\(\)**: Descarga el archivo del artefacto en la ruta `name`
  * **Entry.ref\(\)**: Si la entrada fue almacenada como una referencia usando `add_reference`, devuelve la URI

 Las referencias que tengan esquemas que W&B sepa cómo manejar, pueden ser descargadas igual que los archivos de los artefactos. La API consumidora es la misma.

###   Construir y utilizar un artefacto 

 También puedes construir un objeto artefacto y pasarle use\_artifact. Esto va a crear el artefacto en W&B, si éste no existe aún. Este es idempotente, así que puedes hacerlo tantas veces como lo desees. El artefacto solamente se va a crear una vez, siempre y cuando los contenidos de `model.h5` sigan siendo los mismos.

```python
artifact = wandb.Artifact('reference model')
artifact.add_file('model.h5')
run.use_artifact(artifact)
```

### Descarga un artefacto fuera de una ejecución

```python
api = wandb.Api()
artifact = api.artifact('entity/project/artifact:alias')
artifact.download()
```

##  Actualizando artefactos

Puedes actualizar las variables `description`, `metadata` y `aliases` de un artefacto solamente estableciéndolas a los valores deseados y entonces llamando a `save()`.

```python
api = wandb.Api()
artifact = api.artifact('bike-dataset:latest')

# Update the description
artifact.description = "My new description"

# Selectively update metadata keys
artifact.metadata["oldKey"] = "new value"

# Replace the metadata entirely
artifact.metadata = {"newKey": "new value"}

# Add an alias
artifact.aliases.append('best')

# Remove an alias
artifact.aliases.remove('latest')

# Completely replace the aliases
artifact.aliases = ['replaced']

# Persist all artifact modifications
artifact.save()
```



```python
artifact = wandb.use_artifact('data:v0')
producer_run = artifact.logged_by()
input_artifacts = producer_run.used_artifacts()
```

## Privacidad de los datos

Los artefactos utilizan un control de acceso seguro a nivel de API. Los archivos son encriptados cuando están en reposo y en curso. Los artefactos también pueden rastrear referencias a buckets privados sin enviar los contenidos de los archivoa a W&B. Para más alternativas, contáctanos en [contact@wandb.com](mailto:contact@wandb.com) para hablar acerca de la nube privada y de las instalaciones en un entorno local.

