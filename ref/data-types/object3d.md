# Object3D

​[​![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)](https://www.github.com/wandb/client/tree/master/wandb/data_types.py#L986-L1148) [GitHub에서 소스 확인하기](https://www.github.com/wandb/client/tree/master/wandb/data_types.py#L986-L1148)**​**​

3D 포인트 클라우드에 대한 Wandb 클래스

```text
Object3D(    data_or_path, **kwargs)
```

| **전달인자** | ​ |
| :--- | :--- |
|  `data_or_path` |  \(numpy array, string, io\) 파일 또는 넘파이 배열에서 Object3D를 초기화할 수 있습니다. 지원되는 파일 유형은 obj, gltf, babylon, stl입니다. 사용자는 파일 또는 io 객체 경로 및 'obj', 'gltf', 'babylon', 'stl'\` 중 하나여야 하는 file\_type을 전달할 수 있습니다. |

 넘파이 배열의 형태는 반드시 다음 중 하나여야 합니다:

```text
[[x y z],       ...] nx3[x y z c],     ...] nx4 where c is a category with supported range [1, 14][x y z r g b], ...] nx4 where is rgb is color
```

| **클래스 변수** | ​ |
| :--- | :--- |
|  SUPPORTED\_TYPES | ​ |
|  artifact\_type |  \`'object3D-file'\` |

