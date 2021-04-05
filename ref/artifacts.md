# Artifacts

## wandb.sdk.wandb\_artifacts

 [\[ver fuente\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_artifacts.py#L2)

###  Objetos Artifact

```python
class Artifact(object)
```

[\[ver fuente\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_artifacts.py#L69)

Puedes escribir archivos en un objeto artifact y pasarlo a log\_artifact.

**add**

```python
 | add(obj, name)
```

[\[ver fuente\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_artifacts.py#L229)

 Agrega `obj` al artefacto, ubicado en name. Puedes usar Artifact\#get\(`name`\) después de descargar el artefacto para recuperar este objeto.

 **Argumentos:**

* `obj` _wandb.Media_ - El objeto que se va a guardar en el artefacto
* `name` _str_ - La ruta para guardar

**get\_added\_local\_path\_name**

```python
 | get_added_local_path_name(local_path)
```

 [\[ver fuente\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_artifacts.py#L278)

Si local\_path ya fue agregada al artefacto, devuelve su nombre interno.

### Objetos ArtifactManifestV1

```python
class ArtifactManifestV1(ArtifactManifest)
```

 [\[ver fuente\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_artifacts.py#L328)

**to\_manifest\_json**

```python
 | to_manifest_json()
```

[\[ver fuente\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_artifacts.py#L368)

Este es el JSON que está almacenado en wandb\_manifest.json.

Si include\_local es True, también incluimos las rutas locales a los archivos. Es usado para representar un artefacto que está esperando a ser guardado en el sistema actual. No necesitamos incluir las rutas locales en los contenidos del manifiesto del artefacto.

###  Objetos TrackingHandler

```python
class TrackingHandler(StorageHandler)
```

 [\[ver fuente\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_artifacts.py#L636)

**\_\_init\_\_**

```python
 | __init__(scheme=None)
```

 [\[ver fuente\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_artifacts.py#L637)

Rastrea las rutas como están, sin modificación ni procesamiento especial. Es útil cuando las rutas que están siendo rastreadas están en sistemas de archivos montados en una ubicación estandarizada.

Por ejemplo, si los datos a los que hay que hacerles seguimiento están ubicados en una compartición NFS montada en /data, entonces es suficiente rastrear solamente a las rutas.

###  Objetos LocalFileHandler

```python
class LocalFileHandler(StorageHandler)
```

 [\[ver fuente\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_artifacts.py#L682)

Handles file:// references

**\_\_init\_\_**

```python
 | __init__(scheme=None)
```

[\[ver fuente\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_artifacts.py#L686)

Hace el seguimiento de los archivos o los directorios en el sistema de archivos local. Los directorios son expandidos para crear una entrada por cada archivo contenido en su interior.

### Objetos WBArtifactHandler

```python
class WBArtifactHandler(StorageHandler)
```

 [\[ver fuente\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_artifacts.py#L1172)

Maneja la carga y el almacenamiento de archivos de tipo de referencia a Artifact

