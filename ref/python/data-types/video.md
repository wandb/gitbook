# Video



[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/latest/wandb/sdk/data_types.py#L1160-L1349)



Format a video for logging to W&B.

```python
Video(
    data_or_path: Union['np.ndarray', str, 'TextIO'],
    caption: Optional[str] = None,
    fps: int = 4,
    format: Optional[str] = None
)
```





| Arguments |  |
| :--- | :--- |
|  `data_or_path` |  (numpy array, string, io) Video can be initialized with a path to a file or an io object. The format must be "gif", "mp4", "webm" or "ogg". The format must be specified with the format argument. Video can be initialized with a numpy tensor. The numpy tensor must be either 4 dimensional or 5 dimensional. Channels should be (time, channel, height, width) or (batch, time, channel, height width) |
|  `caption` |  (string) caption associated with the video for display |
|  `fps` |  (int) frames per second for video. Default is 4. |
|  `format` |  (string) format of video, necessary if initializing with path or io object. |



#### Examples:

### Log a numpy array as a video
<!--yeadoc-test:log-video-numpy-->
```python
import numpy as np
import wandb

wandb.init()
# axes are (time, channel, height, width)
frames = np.random.randint(low=0, high=256, size=(10, 3, 100, 100), dtype=np.uint8)
wandb.log({"video": wandb.Video(frames, fps=4)})
```


## Methods

<h3 id="encode"><code>encode</code></h3>

[View source](https://www.github.com/wandb/client/tree/latest/wandb/sdk/data_types.py#L1240-L1277)

```python
encode() -> None
```








| Class Variables |  |
| :--- | :--- |
|  `EXTS`<a id="EXTS"></a> |   |

