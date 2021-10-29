# Image



[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/v0.12.6/wandb/sdk/data_types.py#L1762-L2255)



Wandb class for images.

```python
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
|  `data_or_path` |  (numpy array, string, io) Accepts numpy array of image data, or a PIL image. The class attempts to infer the data format and converts it. |
|  `mode` |  (string) The PIL mode for an image. Most common are "L", "RGB", "RGBA". Full explanation at https://pillow.readthedocs.io/en/4.2.x/handbook/concepts.html#concept-modes. |
|  `caption` |  (string) Label for display of image. |





| Attributes |  |
| :--- | :--- |



## Methods

<h3 id="all_boxes"><code>all_boxes</code></h3>

[View source](https://www.github.com/wandb/client/tree/v0.12.6/wandb/sdk/data_types.py#L2180-L2201)

```python
@classmethod
all_boxes(
    images: Sequence['Image'],
    run: "LocalRun",
    run_key: str,
    step: Union[int, str]
) -> Union[List[Optional[dict]], bool]
```




<h3 id="all_captions"><code>all_captions</code></h3>

[View source](https://www.github.com/wandb/client/tree/v0.12.6/wandb/sdk/data_types.py#L2203-L2207)

```python
@classmethod
all_captions(
    images: Sequence['Media']
) -> Union[bool, Sequence[Optional[str]]]
```




<h3 id="all_masks"><code>all_masks</code></h3>

[View source](https://www.github.com/wandb/client/tree/v0.12.6/wandb/sdk/data_types.py#L2157-L2178)

```python
@classmethod
all_masks(
    images: Sequence['Image'],
    run: "LocalRun",
    run_key: str,
    step: Union[int, str]
) -> Union[List[Optional[dict]], bool]
```




<h3 id="guess_mode"><code>guess_mode</code></h3>

[View source](https://www.github.com/wandb/client/tree/v0.12.6/wandb/sdk/data_types.py#L2051-L2065)

```python
guess_mode(
    data: "np.ndarray"
) -> str
```

Guess what type of image the np.array is representing


<h3 id="to_uint8"><code>to_uint8</code></h3>

[View source](https://www.github.com/wandb/client/tree/v0.12.6/wandb/sdk/data_types.py#L2067-L2089)

```python
@classmethod
to_uint8(
    data: "np.ndarray"
) -> "np.ndarray"
```

Converts floating point image on the range [0,1] and integer images
on the range [0,255] to uint8, clipping if necessary.





| Class Variables |  |
| :--- | :--- |
|  `MAX_DIMENSION`<a id="MAX_DIMENSION"></a> |  `65500` |
|  `MAX_ITEMS`<a id="MAX_ITEMS"></a> |  `108` |

