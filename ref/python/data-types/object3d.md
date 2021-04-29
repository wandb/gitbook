# wandb.data\_types.Object3D

[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/v0.10.28/wandb/sdk/data_types.py#L595-L776)

Wandb class for 3D point clouds.

```text
Object3D(
    data_or_path: Union['np.ndarray', str, 'TextIO'],
    **kwargs
) -> None
```

| Arguments |  |
| :--- | :--- |
|  `data_or_path` |  \(numpy array, string, io\) Object3D can be initialized from a file or a numpy array. The file types supported are obj, gltf, babylon, stl. You can pass a path to a file or an io object and a file\_type which must be one of \`'obj', 'gltf', 'babylon', 'stl'\`. |

The shape of the numpy array must be one of either:

```python
[[x y z],       ...] nx3
[x y z c],     ...] nx4 where c is a category with supported range [1, 14]
[x y z r g b], ...] nx4 where is rgb is color
```

| Class Variables |  |
| :--- | :--- |
|  SUPPORTED\_TYPES |  |

