# wandb.apis.public.Sweep

[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/94c226afc4925535e6301c9bc9b9ee36061d99d4/wandb/apis/public.py#L1410-L1593)

A set of runs associated with a sweep.

```text
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
|  `runs` |  \(`Runs`\) list of runs |
|  `id` |  \(str\) sweep id |
|  `project` |  \(str\) name of project |
|  `config` |  \(str\) dictionary of sweep configuration |

## Methods

### `best_run` <a id="best_run"></a>

[View source](https://www.github.com/wandb/client/tree/94c226afc4925535e6301c9bc9b9ee36061d99d4/wandb/apis/public.py#L1501-L1524)

```text
best_run(
    order=None
)
```

Returns the best run sorted by the metric defined in config or the order passed in

### `get` <a id="get"></a>

[View source](https://www.github.com/wandb/client/tree/94c226afc4925535e6301c9bc9b9ee36061d99d4/wandb/apis/public.py#L1540-L1590)

```text
@classmethod
get(
    client, entity=None, project=None, sid=None, withRuns=(True), order=None,
    query=None, **kwargs
)
```

Execute a query against the cloud backend

### `load` <a id="load"></a>

[View source](https://www.github.com/wandb/client/tree/94c226afc4925535e6301c9bc9b9ee36061d99d4/wandb/apis/public.py#L1481-L1490)

```text
load(
    force=(False)
)
```

### `snake_to_camel` <a id="snake_to_camel"></a>

[View source](https://www.github.com/wandb/client/tree/94c226afc4925535e6301c9bc9b9ee36061d99d4/wandb/apis/public.py#L567-L569)

```text
snake_to_camel(
    string
)
```

| Class Variables |  |
| :--- | :--- |
|  QUERY |  |

