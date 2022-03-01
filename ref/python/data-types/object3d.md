# Object3D



[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/v0.12.11/wandb/sdk/data_types.py#L694-L867)



Wandb class for 3D point clouds.

```python
Object3D(
    data_or_path: Union['np.ndarray', str, 'TextIO'],
    **kwargs
) -> None
```





| Arguments |  |
| :--- | :--- |
|  `data_or_path` |  (numpy array, string, io) Object3D can be initialized from a file or a numpy array. You can pass a path to a file or an io object and a file_type which must be one of `"obj"`, `"gltf"`, `"glb"`, `"babylon"`, `"stl"`, `"pts.json"`. |


The shape of the numpy array must be one of either:
```python
[[x y z],       ...] nx3
[x y z c],     ...] nx4 where c is a category with supported range [1, 14]
[x y z r g b], ...] nx4 where is rgb is color
```



| Class Variables |  |
| :--- | :--- |
|  `SUPPORTED_TYPES`<a id="SUPPORTED_TYPES"></a> |   |

