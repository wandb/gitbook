# wandb.apis.public.Run

[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/v0.10.28/wandb/apis/public.py#L843-L1400)

A single run associated with an entity and project.

```text
Run(
    client, entity, project, run_id, attrs={}
)
```

| Attributes |
| :--- |


## Methods

### `create` <a id="create"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.28/wandb/apis/public.py#L925-L965)

```text
@classmethod
create(
    api, run_id=None, project=None, entity=None
)
```

Create a run for the given project

### `delete` <a id="delete"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.28/wandb/apis/public.py#L1060-L1094)

```text
delete(
    delete_artifacts=(False)
)
```

Deletes the given run from the wandb backend.

### `file` <a id="file"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.28/wandb/apis/public.py#L1156-L1165)

```text
file(
    name
)
```

Arguments: name \(str\): name of requested file.

| Returns |
| :--- |
|  A `File` matching the name argument. |

### `files` <a id="files"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.28/wandb/apis/public.py#L1144-L1154)

```text
files(
    names=[], per_page=50
)
```

Arguments: names \(list\): names of the requested files, if empty returns all files per\_page \(int\): number of results per page

| Returns |
| :--- |
|  A `Files` object, which is an iterator over `File` obejcts. |

### `history` <a id="history"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.28/wandb/apis/public.py#L1190-L1229)

```text
history(
    samples=500, keys=None, x_axis='_step', pandas=(True),
    stream='default'
)
```

Returns sampled history metrics for a run. This is simpler and faster if you are ok with the history records being sampled.

| Arguments |
| :--- |
|  samples \(int, optional\): The number of samples to return pandas \(bool, optional\): Return a pandas dataframe keys \(list, optional\): Only return metrics for specific keys x\_axis \(str, optional\): Use this metric as the xAxis defaults to \_step stream \(str, optional\): "default" for metrics, "system" for machine metrics |

| Returns |
| :--- |
|  If pandas=True returns a `pandas.DataFrame` of history metrics. If pandas=False returns a list of dicts of history metrics. |

### `load` <a id="load"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.28/wandb/apis/public.py#L967-L1029)

```text
load(
    force=(False)
)
```

### `log_artifact` <a id="log_artifact"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.28/wandb/apis/public.py#L1322-L1354)

```text
log_artifact(
    artifact, aliases=None
)
```

Declare an artifact as output of a run.

| Arguments |
| :--- |
|  artifact \(`Artifact`\): An artifact returned from `wandb.Api().artifact(name)` aliases \(list, optional\): Aliases to apply to this artifact |

| Returns |
| :--- |
|  A `Artifact` object. |

### `logged_artifacts` <a id="logged_artifacts"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.28/wandb/apis/public.py#L1287-L1289)

```text
logged_artifacts(
    per_page=100
)
```

### `save` <a id="save"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.28/wandb/apis/public.py#L1096-L1097)

```text
save()
```

### `scan_history` <a id="scan_history"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.28/wandb/apis/public.py#L1231-L1285)

```text
scan_history(
    keys=None, page_size=1000, min_step=None, max_step=None
)
```

Returns an iterable collection of all history records for a run.

#### Example:

Export all the loss values for an example run

```python
run = api.run("l2k2/examples-numpy-boston/i0wt6xua")
history = run.scan_history(keys=["Loss"])
losses = [row["Loss"] for row in history]
```

| Arguments |
| :--- |
|  keys \(\[str\], optional\): only fetch these keys, and only fetch rows that have all of keys defined. page\_size \(int, optional\): size of pages to fetch from the api |

| Returns |
| :--- |
|  An iterable collection over history records \(dict\). |

### `snake_to_camel` <a id="snake_to_camel"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.28/wandb/apis/public.py#L561-L563)

```text
snake_to_camel(
    string
)
```

### `update` <a id="update"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.28/wandb/apis/public.py#L1031-L1058)

```text
update()
```

Persists changes to the run object to the wandb backend.

### `upload_file` <a id="upload_file"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.28/wandb/apis/public.py#L1167-L1188)

```text
upload_file(
    path, root='.'
)
```

Arguments: path \(str\): name of file to upload. root \(str\): the root path to save the file relative to. i.e. If you want to have the file saved in the run as "my\_dir/file.txt" and you're currently in "my\_dir" you would set root to "../"

| Returns |
| :--- |
|  A `File` matching the name argument. |

### `use_artifact` <a id="use_artifact"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.28/wandb/apis/public.py#L1295-L1320)

```text
use_artifact(
    artifact
)
```

Declare an artifact as an input to a run.

| Arguments |
| :--- |
|  artifact \(`Artifact`\): An artifact returned from `wandb.Api().artifact(name)` |

| Returns |
| :--- |
|  A `Artifact` object. |

### `used_artifacts` <a id="used_artifacts"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.28/wandb/apis/public.py#L1291-L1293)

```text
used_artifacts(
    per_page=100
)
```

