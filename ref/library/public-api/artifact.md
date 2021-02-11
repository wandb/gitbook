# Artifact

[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L2447-L3148)

```text
Artifact(
    client, entity, project, name, attrs=None
)
```

| Attributes |  |
| :--- | :--- |
|  `aliases` |  |
|  `created_at` |  |
|  `description` |  |
|  `digest` |  |
|  `id` |  |
|  `manifest` |  |
|  `metadata` |  |
|  `name` |  |
|  `size` |  |
|  `state` |  |
|  `type` |  |
|  `updated_at` |  |

## Methods

### `add_dir` <a id="add_dir"></a>

[View source](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L2666-L2667)

```text
add_dir(
    path, name=None
)
```

### `add_file` <a id="add_file"></a>

[View source](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L2663-L2664)

```text
add_file(
    path, name=None
)
```

### `add_reference` <a id="add_reference"></a>

[View source](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L2669-L2670)

```text
add_reference(
    path, name=None
)
```

### `delete` <a id="delete"></a>

[View source](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L2643-L2658)

```text
delete()
```

Delete artifact and its files.

### `download` <a id="download"></a>

[View source](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L2799-L2841)

```text
download(
    root=None, recursive=False
)
```

Download the artifact to dir specified by the 

| Arguments |
| :--- |
|  root \(str, optional\): directory to download artifact to. If None artifact will be downloaded to './artifacts//' recursive \(bool, optional\): if set to true, then all dependent artifacts are eagerly downloaded as well. If false, then the dependent artifact will only be downloaded when needed. |

| Returns |
| :--- |
|  The path to the downloaded contents. |

### `expected_type` <a id="expected_type"></a>

[View source](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L2601-L2641)

```text
@staticmethod
expected_type(
    client, name, entity_name, project_name
)
```

Returns the expected type for a given artifact name and project

### `file` <a id="file"></a>

[View source](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L2843-L2864)

```text
file(
    root=None
)
```

Download a single file artifact to dir specified by the 

| Arguments |
| :--- |
|  root \(str, optional\): directory to download artifact to. If None artifact will be downloaded to './artifacts//' |

| Returns |
| :--- |
|  The full path of the downloaded file |

### `from_id` <a id="from_id"></a>

[View source](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L2469-L2509)

```text
@classmethod
from_id(
    artifact_id, client
)
```

### `get` <a id="get"></a>

[View source](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L2763-L2797)

```text
get(
    name
)
```

Returns the wandb.Media resource stored in the artifact. Media can be stored in the artifact via Artifact\#add\(obj: wandbMedia, name: str\)\` Arguments: name \(str\): name of resource.

| Returns |
| :--- |
|  A \`wandb.Media\` which has been stored at \`name\` |

### `get_path` <a id="get_path"></a>

[View source](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L2692-L2761)

```text
get_path(
    name
)
```

### `logged_by` <a id="logged_by"></a>

[View source](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L3115-L3148)

```text
logged_by()
```

Retrieves the run which logged this artifact

| Returns |  |
| :--- | :--- |
|  `Run` |  Run object which logged this artifact |

### `new_file` <a id="new_file"></a>

[View source](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L2660-L2661)

```text
new_file(
    name, mode=None
)
```

### `save` <a id="save"></a>

[View source](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L2877-L2915)

```text
save()
```

Persists artifact changes to the wandb backend.

### `used_by` <a id="used_by"></a>

[View source](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L3071-L3113)

```text
used_by()
```

Retrieves the runs which use this artifact directly

| Returns |
| :--- |
|  \[Run\]: a list of Run objects which use this artifact |

### `verify` <a id="verify"></a>

[View source](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L2917-L2942)

```text
verify(
    root=None
)
```

Verify an artifact by checksumming its downloaded contents.

Raises a ValueError if the verification fails. Does not verify downloaded reference files.

| Arguments |
| :--- |
|  root \(str, optional\): directory to download artifact to. If None artifact will be downloaded to './artifacts//' |

| Class Variables |  |
| :--- | :--- |
|  QUERY |  |

