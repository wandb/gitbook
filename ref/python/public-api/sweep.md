# wandb.apis.public.Sweep

[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/v0.11.0/wandb/apis/public.py#L1406-L1563)

A set of runs associated with a sweep.

```python
Sweep(
    client, entity, project, sweep_id, attrs={}
)
```

#### Examples:

Instantiate with:

```text
api = wandb.Api()
sweep = api.sweep(path/to/sweep)
```

| Attributes |  |
| :--- | :--- |
| `runs` | \(`Runs`\) list of runs |
| `id` | \(str\) sweep id |
| `project` | \(str\) name of project |
| `config` | \(str\) dictionary of sweep configuration |
| `state` | \(str\) the state of the sweep |

## Methods

### `best_run` <a id="best_run"></a>

[View source](https://www.github.com/wandb/client/tree/v0.11.0/wandb/apis/public.py#L1482-L1505)

```python
best_run(
    order=None
)
```

Returns the best run sorted by the metric defined in config or the order passed in

### `get` <a id="get"></a>

[View source](https://www.github.com/wandb/client/tree/v0.11.0/wandb/apis/public.py#L1521-L1560)

```python
@classmethod
get(
    client, entity=None, project=None, sid=None, order=None, query=None, **kwargs
)
```

Execute a query against the cloud backend

### `load` <a id="load"></a>

[View source](https://www.github.com/wandb/client/tree/v0.11.0/wandb/apis/public.py#L1463-L1471)

```python
load(
    force=(False)
)
```

### `snake_to_camel` <a id="snake_to_camel"></a>

[View source](https://www.github.com/wandb/client/tree/v0.11.0/wandb/apis/public.py#L567-L569)

```python
snake_to_camel(
    string
)
```

| Class Variables |  |
| :--- | :--- |
| `QUERY` |  |

