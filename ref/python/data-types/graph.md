# wandb.data\_types.Graph

[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/c129c32964aca6a8509d98a0cc3c9bc46f2d8a4c/wandb/data_types.py#L1223-L1381)

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

[View source](https://www.github.com/wandb/client/tree/c129c32964aca6a8509d98a0cc3c9bc46f2d8a4c/wandb/data_types.py#L1307-L1311)

```text
add_edge(
    from_node, to_node
)
```

### `add_node` <a id="add_node"></a>

[View source](https://www.github.com/wandb/client/tree/c129c32964aca6a8509d98a0cc3c9bc46f2d8a4c/wandb/data_types.py#L1293-L1305)

```text
add_node(
    node=None, **node_kwargs
)
```

### `from_keras` <a id="from_keras"></a>

[View source](https://www.github.com/wandb/client/tree/c129c32964aca6a8509d98a0cc3c9bc46f2d8a4c/wandb/data_types.py#L1313-L1342)

```text
@classmethod
from_keras(
    model
)
```

### `pprint` <a id="pprint"></a>

[View source](https://www.github.com/wandb/client/tree/c129c32964aca6a8509d98a0cc3c9bc46f2d8a4c/wandb/data_types.py#L1287-L1291)

```text
pprint()
```

### `__getitem__` <a id="__getitem__"></a>

[View source](https://www.github.com/wandb/client/tree/c129c32964aca6a8509d98a0cc3c9bc46f2d8a4c/wandb/data_types.py#L1284-L1285)

```text
__getitem__(
    nid
)
```

| Class Variables |  |
| :--- | :--- |
|  artifact\_type |  `None` |

