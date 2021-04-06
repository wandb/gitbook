# wandb.apis.public.Sweep

[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/18a721ba0f880a64aea802ebd3e2862f394610f4/wandb/apis/public.py#L1389-L1567)

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

[View source](https://www.github.com/wandb/client/tree/18a721ba0f880a64aea802ebd3e2862f394610f4/wandb/apis/public.py#L1475-L1498)

```text
best_run(
    order=None
)
```

Returns the best run sorted by the metric defined in config or the order passed in

### `get` <a id="get"></a>

[View source](https://www.github.com/wandb/client/tree/18a721ba0f880a64aea802ebd3e2862f394610f4/wandb/apis/public.py#L1514-L1564)

```text
@classmethod
get(
    client, entity=None, project=None, sid=None, withRuns=(True), order=None,
    query=None, **kwargs
)
```

Execute a query against the cloud backend

### `load` <a id="load"></a>

[View source](https://www.github.com/wandb/client/tree/18a721ba0f880a64aea802ebd3e2862f394610f4/wandb/apis/public.py#L1455-L1464)

```text
load(
    force=(False)
)
```

### `snake_to_camel` <a id="snake_to_camel"></a>

[View source](https://www.github.com/wandb/client/tree/18a721ba0f880a64aea802ebd3e2862f394610f4/wandb/apis/public.py#L561-L563)

```text
snake_to_camel(
    string
)
```

| Class Variables |  |
| :--- | :--- |
|  QUERY |  |

