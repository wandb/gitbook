# Artifacts

## wandb.sdk.wandb\_artifacts

[\[view\_source\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_artifacts.py#L2)

### アーティファクト

```python
class Artifact(object)
```

[\[view\_source\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_artifacts.py#L69)

このアーティファクトをフェッチするために使用できる安定した名前。

**add**

```python
 | add(obj, name)
```

[\[view\_source\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_artifacts.py#L229)

Adds `obj` to the artifact, located at `name`. You can use Artifact\#get\(`name`\) after downloading the artifact to retrieve this object.

**Arguments**:

* `obj` _wandb.Media_ - The object to save in an artifact
* `name` _str_ - The path to save

**get\_added\_local\_path\_name**

```python
 | get_added_local_path_name(local_path)
```

[\[view\_source\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_artifacts.py#L278)

If local\_path was already added to artifact, return its internal name.

### ArtifactManifestV1 Objects

```python
class ArtifactManifestV1(ArtifactManifest)
```

[\[view\_source\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_artifacts.py#L328)

**to\_manifest\_json**

```python
 | to_manifest_json()
```

[\[view\_source\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_artifacts.py#L368)

This is the JSON that's stored in wandb\_manifest.json

If include\_local is True we also include the local paths to files. This is used to represent an artifact that's waiting to be saved on the current system. We don't need to include the local paths in the artifact manifest contents.

### TrackingHandler Objects

```python
class TrackingHandler(StorageHandler)
```

[\[view\_source\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_artifacts.py#L636)

**\_\_init\_\_**

```python
 | __init__(scheme=None)
```

[\[view\_source\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_artifacts.py#L637)

Tracks paths as is, with no modification or special processing. Useful when paths being tracked are on file systems mounted at a standardized location.

For example, if the data to track is located on an NFS share mounted on /data, then it is sufficient to just track the paths.

### LocalFileHandler Objects

```python
class LocalFileHandler(StorageHandler)
```

[\[view\_source\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_artifacts.py#L682)

Handles file:// references

**\_\_init\_\_**

```python
 | __init__(scheme=None)
```

[\[view\_source\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_artifacts.py#L686)

Tracks files or directories on a local filesystem. Directories are expanded to create an entry for each file contained within.

### WBArtifactHandler Objects

```python
class WBArtifactHandler(StorageHandler)
```

[\[view\_source\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_artifacts.py#L1172)

Handles loading and storing Artifact reference-type files

