# wandb.data\_types.Graph

[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/v0.12.2/wandb/data_types.py#L1233-L1393)

Wandb class for graphs

```python
Graph(
    format="keras"
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

[View source](https://www.github.com/wandb/client/tree/v0.12.2/wandb/data_types.py#L1319-L1323)

```python
add_edge(
    from_node, to_node
)
```

### `add_node` <a id="add_node"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.2/wandb/data_types.py#L1305-L1317)

```python
add_node(
    node=None, **node_kwargs
)
```

### `from_keras` <a id="from_keras"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.2/wandb/data_types.py#L1325-L1354)

```python
@classmethod
from_keras(
    model
)
```

### `pprint` <a id="pprint"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.2/wandb/data_types.py#L1299-L1303)

```python
pprint()
```

### `__getitem__` <a id="__getitem__"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.2/wandb/data_types.py#L1296-L1297)

```python
__getitem__(
    nid
)
```

