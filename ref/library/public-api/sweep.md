# Sweep

[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L1356-L1534)

A set of runs associated with a sweep

```text
Sweep(
    client, entity, project, sweep_id, attrs={}
)
```

#### Instantiate with:

api.sweep\(sweep\_path\)

| Attributes |  |
| :--- | :--- |
|  `config` |  |
|  `entity` |  |
|  `order` |  |
|  `path` |  |
|  `url` |  |
|  `username` |  |

## Methods

### `best_run` <a id="best_run"></a>

[View source](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L1442-L1465)

```text
best_run(
    order=None
)
```

Returns the best run sorted by the metric defined in config or the order passed in

### `get` <a id="get"></a>

[View source](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L1481-L1531)

```text
@classmethod
get(
    client, entity=None, project=None, sid=None, withRuns=True, order=None,
    query=None, **kwargs
)
```

Execute a query against the cloud backend

### `load` <a id="load"></a>

[View source](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L1422-L1431)

```text
load(
    force=False
)
```

### `snake_to_camel` <a id="snake_to_camel"></a>

[View source](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L528-L530)

```text
snake_to_camel(
    string
)
```

| Class Variables |  |
| :--- | :--- |
|  QUERY |  |

