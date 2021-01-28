# Artifacts

## wandb.sdk.wandb\_artifacts

 [\[voir\_source\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_artifacts.py#L2)

### Objets Artefacts

```python
class Artifact(object)
```

 [\[voir\_source\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_artifacts.py#L69)

Un objet artefact dans lequel vous pouvez écrire des fichiers, et passer dans log\_artifact.

**add**

```python
 | add(obj, name)
```

 [\[voir\_source\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_artifacts.py#L229)

 Ajoute `obj` à l’artefact, localisé à name. Vous pouvez utiliser Artifact\#get\(`name`\) après avoir téléchargé l’artefact pour récupérer cet objet.

**Arguments**:

* `obj` _wandb.Media_ - L’objet à sauvegarder dans un artefact
* `name` _str_ - Le chemin à sauvegarder

**get\_added\_local\_path\_name**

```python
 | get_added_local_path_name(local_path)
```

 [\[voir\_source\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_artifacts.py#L278)

Si local\_path a déjà été ajouté à artefact, renvoie son nom interne.

###  Objets ArtifactManifestV1

```python
class ArtifactManifestV1(ArtifactManifest)
```

[\[view\_source\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_artifacts.py#L328)

**to\_manifest\_json**

```python
 | to_manifest_json()
```

[\[view\_source\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_artifacts.py#L368)

C’est le JSON qui est stocké dans wandb\_manifest.json

Si include\_local est True, nous incluons aussi les chemins locaux aux fichiers. C’est utilisé pour représenter un artefact qui attend d’être enregistré dans le système actuel. Nous n’avons pas besoin d’inclure les chemins locaux dans les contenus manifestes d’artefact.

### Objets TrackingHandler

```python
class TrackingHandler(StorageHandler)
```

[\[view\_source\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_artifacts.py#L636)

**\_\_init\_\_**

```python
 | __init__(scheme=None)
```

 [\[voir\_source\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_artifacts.py#L637)

Trace les chemins comme tels, sans modification ni traitement spécial. Utile lorsque les chemins qui sont tracés sont sur des fichiers systèmes montés à un endroit standardisé.

Par exemple, si les données à tracer sont localisées sur un partage NFS monté sur /data, alors il est suffisant de simplement tracer les chemins comme tels.

### Objets LocalFileHandler

```python
class LocalFileHandler(StorageHandler)
```

 [\[voir\_source\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_artifacts.py#L682)

Handles file:// references

**\_\_init\_\_**

```python
 | __init__(scheme=None)
```

 [\[voir\_source\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_artifacts.py#L686)

Garde une trace des fichiers ou des répertoires sur un système de fichier local. Les répertoires sont étendus pour créer une entrée pour chaque fichier compris dedans.

### WBArtifactHandler Objects

```python
class WBArtifactHandler(StorageHandler)
```

[\[voir\_source\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_artifacts.py#L1172)

 Gère le chargement et le stockage des fichiers Artefact de type référence

