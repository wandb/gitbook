# Image

<!-- Insert buttons and diff -->


[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/master/wandb/sdk/data_types.py#L1451-L1912)




Wandb class for images.

<pre><code>Image(
    data_or_path: "ImageDataOrPathType",
    mode: Optional[str] = None,
    caption: Optional[str] = None,
    grouping: Optional[str] = None,
    classes: Optional[Union['Classes', Sequence[dict]]] = None,
    boxes: Optional[Union[Dict[str, 'BoundingBoxes2D'], Dict[str, dict]]] = None,
    masks: Optional[Union[Dict[str, 'ImageMask'], Dict[str, dict]]] = None
) -> None</code></pre>



<!-- Placeholder for "Used in" -->


<!-- Tabular view -->
<table>
<tr><th>Arguments</th></tr>

<tr>
<td>
<code>data_or_path</code>
</td>
<td>
(numpy array, string, io) Accepts numpy array of
image data, or a PIL image. The class attempts to infer
the data format and converts it.
</td>
</tr><tr>
<td>
<code>mode</code>
</td>
<td>
(string) The PIL mode for an image. Most common are "L", "RGB",
"RGBA". Full explanation at https://pillow.readthedocs.io/en/4.2.x/handbook/concepts.html#concept-modes.
</td>
</tr><tr>
<td>
<code>caption</code>
</td>
<td>
(string) Label for display of image.
</td>
</tr>
</table>



## Methods

<h3 id="all_boxes"><code>all_boxes</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/sdk/data_types.py#L1869-L1890">View source</a>

<pre><code>@classmethod</code>
<code>all_boxes(
    images: Sequence['Image'],
    run: "LocalRun",
    run_key: str,
    step: Union[int, str]
) -> Union[List[Optional[dict]], bool]</code></pre>




<h3 id="all_captions"><code>all_captions</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/sdk/data_types.py#L1892-L1896">View source</a>

<pre><code>@classmethod</code>
<code>all_captions(
    images: Sequence['Media']
) -> Union[bool, Sequence[Optional[str]]]</code></pre>




<h3 id="all_masks"><code>all_masks</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/sdk/data_types.py#L1846-L1867">View source</a>

<pre><code>@classmethod</code>
<code>all_masks(
    images: Sequence['Image'],
    run: "LocalRun",
    run_key: str,
    step: Union[int, str]
) -> Union[List[Optional[dict]], bool]</code></pre>




<h3 id="guess_mode"><code>guess_mode</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/sdk/data_types.py#L1740-L1754">View source</a>

<pre><code>guess_mode(
    data: "np.ndarray"
) -> str</code></pre>

Guess what type of image the np.array is representing


<h3 id="to_uint8"><code>to_uint8</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/sdk/data_types.py#L1756-L1778">View source</a>

<pre><code>@classmethod</code>
<code>to_uint8(
    data: "np.ndarray"
) -> "np.ndarray"</code></pre>

Converts floating point image on the range [0,1] and integer images
on the range [0,255] to uint8, clipping if necessary.





<!-- Tabular view -->
<table>
<tr><th>Class Variables</th></tr>

<tr>
<td>
MAX_DIMENSION<a id="MAX_DIMENSION"></a>
</td>
<td>
`65500`
</td>
</tr><tr>
<td>
MAX_ITEMS<a id="MAX_ITEMS"></a>
</td>
<td>
`108`
</td>
</tr><tr>
<td>
artifact_type<a id="artifact_type"></a>
</td>
<td>
`'image-file'`
</td>
</tr>
</table>

