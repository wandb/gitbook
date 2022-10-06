# Object3D



[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/latest/wandb/sdk/data_types/object_3d.py#L76-L316)



Wandb class for 3D point clouds.

```python
Object3D(
    data_or_path: Union['np.ndarray', str, 'TextIO', dict],
    **kwargs
) -> None
```





| Arguments |  |
| :--- | :--- |
|  `data_or_path` |  (numpy array, string, io) Object3D can be initialized from a file or a numpy array. You can pass a path to a file or an io object and a file_type which must be one of SUPPORTED_TYPES |


The shape of the numpy array must be one of either:
```
[[x y z],       ...] nx3
[[x y z c],     ...] nx4 where c is a category with supported range [1, 14]
[[x y z r g b], ...] nx4 where is rgb is color
```

## Methods

<h3 id="from_file"><code>from_file</code></h3>

[View source](https://www.github.com/wandb/client/tree/latest/wandb/sdk/data_types/object_3d.py#L221-L229)

```python
@classmethod
from_file(
    data_or_path: Union['TextIO', str],
    file_type: "FileFormat3D" = None
) -> "Object3D"
```




<h3 id="from_numpy"><code>from_numpy</code></h3>

[View source](https://www.github.com/wandb/client/tree/latest/wandb/sdk/data_types/object_3d.py#L231-L244)

```python
@classmethod
from_numpy(
    data: "np.ndarray"
) -> "Object3D"
```




<h3 id="from_point_cloud"><code>from_point_cloud</code></h3>

[View source](https://www.github.com/wandb/client/tree/latest/wandb/sdk/data_types/object_3d.py#L246-L270)

```python
@classmethod
from_point_cloud(
    points: Sequence['Point'],
    boxes: Sequence['Box3D'],
    vectors: Optional[Sequence['Vector3D']] = None,
    point_cloud_type: "PointCloudType" = "lidar/beta"
) -> "Object3D"
```








| Class Variables |  |
| :--- | :--- |
|  `SUPPORTED_POINT_CLOUD_TYPES`<a id="SUPPORTED_POINT_CLOUD_TYPES"></a> |   |
|  `SUPPORTED_TYPES`<a id="SUPPORTED_TYPES"></a> |   |

