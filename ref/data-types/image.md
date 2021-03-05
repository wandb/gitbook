# Image

[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)](https://www.github.com/wandb/client/tree/master/wandb/data_types.py#L1647-L2062)[GitHub](https://www.github.com/wandb/client/tree/master/wandb/data_types.py#L1647-L2062)에서 소스 확인하기

이미지에 대한 Wandb 클래스

```text
Image(
    data_or_path, mode=None, caption=None, grouping=None, classes=None, boxes=None,
    masks=None
)
```

| **전달인자** |  |
| :--- | :--- |
|  `data_or_path` |  \(numpy array, string, io\) 이미지 데이터의 넘파이 배열 또는 PIL 이미지를 허용합니다. 이 클래스는 데이터 포맷을 유추하고 변환합니다 |
|  `mode` | \(string\) 이미지에 대한 PIL 모드입니다. 가장 일반적인 경우는 “L”, “RGB”, "RGBA"입니다. 자세한 설명은 [https://pillow.readthedocs.io/en/4.2.x/handbook/concepts.html\#concept-modes](https://pillow.readthedocs.io/en/4.2.x/handbook/concepts.html#concept-modes)을 참조하시기 바랍니다. |
|  `caption` | \(string\) 이미지 표시에 대한 레이블 |

### **방법**

### `all_boxes` <a id="all_boxes"></a>

 [**소스 보기**](https://www.github.com/wandb/client/tree/master/wandb/data_types.py#L2027-L2042)

```text
@classmethod
all_boxes(
    images, run, run_key, step
)
```

### `all_captions` <a id="all_captions"></a>

 [**소스 보기**](https://www.github.com/wandb/client/tree/master/wandb/data_types.py#L2044-L2049)

```text
@classmethod
all_captions(
    images
)
```

### `all_masks` <a id="all_masks"></a>

[View source](https://www.github.com/wandb/client/tree/master/wandb/data_types.py#L2010-L2025)

```text
@classmethod
all_masks(
    images, run, run_key, step
)
```

### `guess_mode` <a id="guess_mode"></a>

 [**소스 보기**](https://www.github.com/wandb/client/tree/master/wandb/data_types.py#L2010-L2025)

```text
guess_mode(
    data
)
```

 np.array가 표현하는 이미지 유형을 추측합니다

### `to_uint8` <a id="to_uint8"></a>

[소스 보기](https://www.github.com/wandb/client/tree/master/wandb/data_types.py#L1928-L1950)

```text
@classmethod
to_uint8(
    data
)
```

범위 \[0,1\]의 부동 소수점 이미지\(floating point image\) 및 범위 \[0,255\]의 정수 이미지\(integer images\)를 uint8로 변환하며, 필요 시 클리핑\(clipping\)합니다.

| **클래스 변수** |  |
| :--- | :--- |
|  MAX\_DIMENSION |  \`65500\` |
|  MAX\_ITEMS |  \`108\` |
|  artifact\_type |  \`'image-file'\` |

