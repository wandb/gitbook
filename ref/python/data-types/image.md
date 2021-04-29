# wandb.data\_types.Image

[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/v0.10.28/wandb/sdk/data_types.py#L1526-L1996)

Wandb class for images.

```text
Image(
    data_or_path: "ImageDataOrPathType",
    mode: Optional[str] = None,
    caption: Optional[str] = None,
    grouping: Optional[str] = None,
    classes: Optional[Union['Classes', Sequence[dict]]] = None,
    boxes: Optional[Union[Dict[str, 'BoundingBoxes2D'], Dict[str, dict]]] = None,
    masks: Optional[Union[Dict[str, 'ImageMask'], Dict[str, dict]]] = None
) -> None
```

| Arguments |  |
| :--- | :--- |
|  `data_or_path` |  \(numpy array, string, io\) Accepts numpy array of image data, or a PIL image. The class attempts to infer the data format and converts it. |
|  `mode` |  \(string\) The PIL mode for an image. Most common are "L", "RGB", "RGBA". Full explanation at https://pillow.readthedocs.io/en/4.2.x/handbook/concepts.html\#concept-modes. |
|  `caption` |  \(string\) Label for display of image. |

## Methods

### `all_boxes` <a id="all_boxes"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.28/wandb/sdk/data_types.py#L1945-L1966)

```text
@classmethod
all_boxes(
    images: Sequence['Image'],
    run: "LocalRun",
    run_key: str,
    step: Union[int, str]
) -> Union[List[Optional[dict]], bool]
```

### `all_captions` <a id="all_captions"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.28/wandb/sdk/data_types.py#L1968-L1972)

```text
@classmethod
all_captions(
    images: Sequence['Media']
) -> Union[bool, Sequence[Optional[str]]]
```

### `all_masks` <a id="all_masks"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.28/wandb/sdk/data_types.py#L1922-L1943)

```text
@classmethod
all_masks(
    images: Sequence['Image'],
    run: "LocalRun",
    run_key: str,
    step: Union[int, str]
) -> Union[List[Optional[dict]], bool]
```

### `guess_mode` <a id="guess_mode"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.28/wandb/sdk/data_types.py#L1816-L1830)

```text
guess_mode(
    data: "np.ndarray"
) -> str
```

Guess what type of image the np.array is representing

### `to_uint8` <a id="to_uint8"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.28/wandb/sdk/data_types.py#L1832-L1854)

```text
@classmethod
to_uint8(
    data: "np.ndarray"
) -> "np.ndarray"
```

Converts floating point image on the range \[0,1\] and integer images on the range \[0,255\] to uint8, clipping if necessary.

| Class Variables |  |
| :--- | :--- |
|  MAX\_DIMENSION |  `65500` |
|  MAX\_ITEMS |  `108` |

