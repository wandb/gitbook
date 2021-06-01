# wandb.data\_types.Graph

[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/v0.10.31/wandb/data_types.py#L1247-L1407)

Wandb class for graphs

```text
Graph(
    format='keras'
)
```

This class is typically used for saving and diplaying neural net models. It represents the graph as an array of nodes and edges. The nodes can have labels that can be visualized by wandb.

#### Examples:

Import a keras model:

```text
    Graph.from_keras(keras_model)
```

## Methods

### `add_edge` <a id="add_edge"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.31/wandb/data_types.py#L1333-L1337)

```text
add_edge(
    from_node, to_node
)
```

### `add_node` <a id="add_node"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.31/wandb/data_types.py#L1319-L1331)

```text
add_node(
    node=None, **node_kwargs
)
```

### `from_keras` <a id="from_keras"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.31/wandb/data_types.py#L1339-L1368)

```text
@classmethod
from_keras(
    model
)
```

### `pprint` <a id="pprint"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.31/wandb/data_types.py#L1313-L1317)

```text
pprint()
```

### `__getitem__` <a id="__getitem__"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.31/wandb/data_types.py#L1310-L1311)

```text
__getitem__(
    nid
)
```

