# Image

[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/master/wandb/data_types.py#L1647-L2062)

Wandb class for images.

```text
Image(
    data_or_path, mode=None, caption=None, grouping=None, classes=None, boxes=None,
    masks=None
)
```

| Arguments |  |
| :--- | :--- |
|  `data_or_path` |  \(numpy array, string, io\) Accepts numpy array of image data, or a PIL image. The class attempts to infer the data format and converts it. |
|  `mode` |  \(string\) The PIL mode for an image. Most common are "L", "RGB", "RGBA". Full explanation at https://pillow.readthedocs.io/en/4.2.x/handbook/concepts.html\#concept-modes. |
|  `caption` |  \(string\) Label for display of image. |

## Methods

### `all_boxes` <a id="all_boxes"></a>

[View source](https://www.github.com/wandb/client/tree/master/wandb/data_types.py#L2027-L2042)

```text
@classmethod
all_boxes(
    images, run, run_key, step
)
```

### `all_captions` <a id="all_captions"></a>

[View source](https://www.github.com/wandb/client/tree/master/wandb/data_types.py#L2044-L2049)

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

[View source](https://www.github.com/wandb/client/tree/master/wandb/data_types.py#L1912-L1926)

```text
guess_mode(
    data
)
```

Guess what type of image the np.array is representing

### `to_uint8` <a id="to_uint8"></a>

[View source](https://www.github.com/wandb/client/tree/master/wandb/data_types.py#L1928-L1950)

```text
@classmethod
to_uint8(
    data
)
```

Converts floating point image on the range \[0,1\] and integer images on the range \[0,255\] to uint8, clipping if necessary.

| Class Variables |  |
| :--- | :--- |
|  MAX\_DIMENSION |  \`65500\` |
|  MAX\_ITEMS |  \`108\` |
|  artifact\_type |  \`'image-file'\` |

