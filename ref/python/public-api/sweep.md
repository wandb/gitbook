# wandb.apis.public.Sweep

[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/c129c32964aca6a8509d98a0cc3c9bc46f2d8a4c/wandb/apis/public.py#L1403-L1581)

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

[View source](https://www.github.com/wandb/client/tree/c129c32964aca6a8509d98a0cc3c9bc46f2d8a4c/wandb/apis/public.py#L1489-L1512)

```text
best_run(
    order=None
)
```

Returns the best run sorted by the metric defined in config or the order passed in

### `get` <a id="get"></a>

[View source](https://www.github.com/wandb/client/tree/c129c32964aca6a8509d98a0cc3c9bc46f2d8a4c/wandb/apis/public.py#L1528-L1578)

```text
@classmethod
get(
    client, entity=None, project=None, sid=None, withRuns=(True), order=None,
    query=None, **kwargs
)
```

Execute a query against the cloud backend

### `load` <a id="load"></a>

[View source](https://www.github.com/wandb/client/tree/c129c32964aca6a8509d98a0cc3c9bc46f2d8a4c/wandb/apis/public.py#L1469-L1478)

```text
load(
    force=(False)
)
```

### `snake_to_camel` <a id="snake_to_camel"></a>

[View source](https://www.github.com/wandb/client/tree/c129c32964aca6a8509d98a0cc3c9bc46f2d8a4c/wandb/apis/public.py#L561-L563)

```text
snake_to_camel(
    string
)
```

| Class Variables |  |
| :--- | :--- |
|  QUERY |  |

