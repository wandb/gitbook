# wandb.apis.public.Run

[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/94c226afc4925535e6301c9bc9b9ee36061d99d4/wandb/apis/public.py#L849-L1407)

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

[View source](https://www.github.com/wandb/client/tree/94c226afc4925535e6301c9bc9b9ee36061d99d4/wandb/apis/public.py#L931-L971)

```text
@classmethod
create(
    api, run_id=None, project=None, entity=None
)
```

Create a run for the given project

### `delete` <a id="delete"></a>

[View source](https://www.github.com/wandb/client/tree/94c226afc4925535e6301c9bc9b9ee36061d99d4/wandb/apis/public.py#L1067-L1101)

```text
delete(
    delete_artifacts=(False)
)
```

Deletes the given run from the wandb backend.

### `file` <a id="file"></a>

[View source](https://www.github.com/wandb/client/tree/94c226afc4925535e6301c9bc9b9ee36061d99d4/wandb/apis/public.py#L1163-L1172)

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

[View source](https://www.github.com/wandb/client/tree/94c226afc4925535e6301c9bc9b9ee36061d99d4/wandb/apis/public.py#L1151-L1161)

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

[View source](https://www.github.com/wandb/client/tree/94c226afc4925535e6301c9bc9b9ee36061d99d4/wandb/apis/public.py#L1197-L1236)

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

[View source](https://www.github.com/wandb/client/tree/94c226afc4925535e6301c9bc9b9ee36061d99d4/wandb/apis/public.py#L973-L1035)

```text
load(
    force=(False)
)
```

### `log_artifact` <a id="log_artifact"></a>

[View source](https://www.github.com/wandb/client/tree/94c226afc4925535e6301c9bc9b9ee36061d99d4/wandb/apis/public.py#L1329-L1361)

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

[View source](https://www.github.com/wandb/client/tree/94c226afc4925535e6301c9bc9b9ee36061d99d4/wandb/apis/public.py#L1294-L1296)

```text
logged_artifacts(
    per_page=100
)
```

### `save` <a id="save"></a>

[View source](https://www.github.com/wandb/client/tree/94c226afc4925535e6301c9bc9b9ee36061d99d4/wandb/apis/public.py#L1103-L1104)

```text
save()
```

### `scan_history` <a id="scan_history"></a>

[View source](https://www.github.com/wandb/client/tree/94c226afc4925535e6301c9bc9b9ee36061d99d4/wandb/apis/public.py#L1238-L1292)

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

[View source](https://www.github.com/wandb/client/tree/94c226afc4925535e6301c9bc9b9ee36061d99d4/wandb/apis/public.py#L567-L569)

```text
snake_to_camel(
    string
)
```

### `update` <a id="update"></a>

[View source](https://www.github.com/wandb/client/tree/94c226afc4925535e6301c9bc9b9ee36061d99d4/wandb/apis/public.py#L1037-L1065)

```text
update()
```

Persists changes to the run object to the wandb backend.

### `upload_file` <a id="upload_file"></a>

[View source](https://www.github.com/wandb/client/tree/94c226afc4925535e6301c9bc9b9ee36061d99d4/wandb/apis/public.py#L1174-L1195)

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

[View source](https://www.github.com/wandb/client/tree/94c226afc4925535e6301c9bc9b9ee36061d99d4/wandb/apis/public.py#L1302-L1327)

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

[View source](https://www.github.com/wandb/client/tree/94c226afc4925535e6301c9bc9b9ee36061d99d4/wandb/apis/public.py#L1298-L1300)

```text
used_artifacts(
    per_page=100
)
```

