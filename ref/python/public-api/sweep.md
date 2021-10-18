# wandb.apis.public.Sweep

[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/v0.12.2/wandb/apis/public.py#L1497-L1654)

A set of runs associated with a sweep.

```python
Sweep(
    client, entity, project, sweep_id, attrs={}
)
```

#### Examples:

Instantiate with:

```
api = wandb.Api()
sweep = api.sweep(path/to/sweep)
```

| Attributes |                                         |
| ---------- | --------------------------------------- |
| `runs`     | (`Runs`) list of runs                   |
| `id`       | (str) sweep id                          |
| `project`  | (str) name of project                   |
| `config`   | (str) dictionary of sweep configuration |
| `state`    | (str) the state of the sweep            |

## Methods

### `best_run` <a href="best_run" id="best_run"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.2/wandb/apis/public.py#L1573-L1596)

```python
best_run(
    order=None
)
```

Returns the best run sorted by the metric defined in config or the order passed in

### `get` <a href="get" id="get"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.2/wandb/apis/public.py#L1612-L1651)

```python
@classmethod
get(
    client, entity=None, project=None, sid=None, order=None, query=None, **kwargs
)
```

Execute a query against the cloud backend

### `load` <a href="load" id="load"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.2/wandb/apis/public.py#L1554-L1562)

```python
load(
    force=(False)
)
```

### `snake_to_camel` <a href="snake_to_camel" id="snake_to_camel"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.2/wandb/apis/public.py#L573-L575)

```python
snake_to_camel(
    string
)
```

| Class Variables |   |
| --------------- | - |
| `QUERY`         |   |
