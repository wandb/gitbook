# Artifact

[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_artifacts.py#L37-L302)

An artifact object you can write files into, and pass to log\_artifact.

```text
Artifact(
    name, type, description=None, metadata=None
)
```

| Attributes |  |
| :--- | :--- |
|  `digest` |  |
|  `entity` |  |
|  `id` |  |
|  `manifest` |  |
|  `project` |  |

## Methods

### `add` <a id="add"></a>

[View source](https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_artifacts.py#L220-L262)

```text
add(
    obj, name
)
```

Adds `obj` to the artifact, located at `name`. You can use [`Artifact.get(name)`](artifact.md#get) after downloading the artifact to retrieve this object.

| Arguments |
| :--- |
|  obj \(wandb.WBValue\): The object to save in an artifact name \(str\): The path to save |

### `add_dir` <a id="add_dir"></a>

[View source](https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_artifacts.py#L150-L183)

```text
add_dir(
    local_path, name=None
)
```

### `add_file` <a id="add_file"></a>

[View source](https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_artifacts.py#L124-L148)

```text
add_file(
    local_path, name=None, is_tmp=False
)
```

Adds a local file to the artifact

| Args |
| :--- |
|  local\_path \(str\): path to the file name \(str, optional\): new path and filename to assign inside artifact. Defaults to None. is\_tmp \(bool, optional\): If true, then the file is renamed deterministically. Defaults to False. |

| Returns |  |
| :--- | :--- |
|  `ArtifactManifestEntry` |  the added entry |

### `add_reference` <a id="add_reference"></a>

[View source](https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_artifacts.py#L185-L218)

```text
add_reference(
    uri, name=None, checksum=True, max_objects=None
)
```

adds `uri` to the artifact via a reference, located at `name`. You can use [`Artifact.get_path(name)`](artifact.md#get_path) to retrieve this object.

| Arguments |
| :--- |
|  uri \(str\) - the URI path of the reference to add. Can be an object returned from Artifact.get\_path to store a reference to another artifact's entry. name \(str\) - the path to save |

### `download` <a id="download"></a>

[View source](https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_artifacts.py#L274-L275)

```text
download()
```

### `finalize` <a id="finalize"></a>

[View source](https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_artifacts.py#L280-L286)

```text
finalize()
```

### `get` <a id="get"></a>

[View source](https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_artifacts.py#L277-L278)

```text
get()
```

### `get_added_local_path_name` <a id="get_added_local_path_name"></a>

[View source](https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_artifacts.py#L264-L269)

```text
get_added_local_path_name(
    local_path
)
```

If local\_path was already added to artifact, return its internal name.

### `get_path` <a id="get_path"></a>

[View source](https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_artifacts.py#L271-L272)

```text
get_path(
    name
)
```

### `new_file` <a id="new_file"></a>

[View source](https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_artifacts.py#L109-L122)

```text
@contextlib.contextmanager
new_file(
    name, mode='w'
)
```

