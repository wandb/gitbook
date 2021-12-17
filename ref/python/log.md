# log



[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/v0.12.9/wandb/sdk/wandb_run.py#L1115-L1341)



Logs a dictonary of data to the current run's history.

```python
log(
    data: Dict[str, Any],
    step: int = None,
    commit: bool = None,
    sync: bool = None
) -> None
```




Use `wandb.log` to log data from runs, such as scalars, images, video,
histograms, plots, and tables.

See our [guides to logging](https://docs.wandb.ai/guides/track/log) for
live examples, code snippets, best practices, and more.

The most basic usage is `wandb.log({"train-loss": 0.5, "accuracy": 0.9})`.
This will save the loss and accuracy to the run's history and update
the summary values for these metrics.

Visualize logged data in the workspace at [wandb.ai](https://wandb.ai),
or locally on a [self-hosted instance](https://docs.wandb.ai/self-hosted)
of the W&B app, or export data to visualize and explore locally, e.g. in
Jupyter notebooks, with [our API](https://docs.wandb.ai/guides/track/public-api-guide).

In the UI, summary values show up in the run table to compare single values across runs.
Summary values can also be set directly with `wandb.run.summary["key"] = value`.

Logged values don't have to be scalars. Logging any wandb object is supported.
For example `wandb.log({"example": wandb.Image("myimage.jpg")})` will log an
example image which will be displayed nicely in the W&B UI.
See the [reference documentation](https://docs.wandb.com/library/reference/data_types)
for all of the different supported types or check out our
[guides to logging](https://docs.wandb.ai/guides/track/log) for examples,
from 3D molecular structures and segmentation masks to PR curves and histograms.
`wandb.Table`s can be used to logged structured data. See our
[guide to logging tables](https://docs.wandb.ai/guides/data-vis/log-tables)
for details.

Logging nested metrics is encouraged and is supported in the W&B UI.
If you log with a nested dictionary like `wandb.log({"train":
{"acc": 0.9}, "val": {"acc": 0.8}})`, the metrics will be organized into
`train` and `val` sections in the W&B UI.

wandb keeps track of a global step, which by default increments with each
call to `wandb.log`, so logging related metrics together is encouraged.
If it's inconvenient to log related metrics together
calling `wandb.log({"train-loss": 0.5, commit=False})` and then
`wandb.log({"accuracy": 0.9})` is equivalent to calling
`wandb.log({"train-loss": 0.5, "accuracy": 0.9})`.

`wandb.log` is not intended to be called more than a few times per second.
If you want to log more frequently than that it's better to aggregate
the data on the client side or you may get degraded performance.

| Arguments |  |
| :--- | :--- |
|  `row` |  (dict, optional) A dict of serializable python objects i.e `str`, `ints`, `floats`, `Tensors`, `dicts`, or any of the `wandb.data_types`. |
|  `commit` |  (boolean, optional) Save the metrics dict to the wandb server and increment the step. If false `wandb.log` just updates the current metrics dict with the row argument and metrics won't be saved until `wandb.log` is called with `commit=True`. |
|  `step` |  (integer, optional) The global step in processing. This persists any non-committed earlier steps but defaults to not committing the specified step. |
|  `sync` |  (boolean, True) This argument is deprecated and currently doesn't change the behaviour of `wandb.log`. |



#### Examples:

For more and more detailed examples, see
[our guides to logging](https://docs.wandb.com/guides/track/log).

### Basic usage
<!--yeadoc-test:init-and-log-basic-->
```python
import wandb
wandb.init()
wandb.log({'accuracy': 0.9, 'epoch': 5})
```

### Incremental logging
<!--yeadoc-test:init-and-log-incremental-->
```python
import wandb
wandb.init()
wandb.log({'loss': 0.2}, commit=False)
# Somewhere else when I'm ready to report this step:
wandb.log({'accuracy': 0.8})
```

### Histogram
<!--yeadoc-test:init-and-log-histogram-->
```python
import numpy as np
import wandb

# sample gradients at random from normal distribution
gradients = np.random.randn(100, 100)
wandb.init()
wandb.log({"gradients": wandb.Histogram(gradients)})
```

### Image from numpy
<!--yeadoc-test:init-and-log-image-numpy-->
```python
import numpy as np
import wandb

wandb.init()
examples = []
for i in range(3):
    pixels = np.random.randint(low=0, high=256, size=(100, 100, 3))
    image = wandb.Image(pixels, caption=f"random field {i}")
    examples.append(image)
wandb.log({"examples": examples})
```

### Image from PIL
<!--yeadoc-test:init-and-log-image-pillow-->
```python
import numpy as np
from PIL import Image as PILImage
import wandb

wandb.init()
examples = []
for i in range(3):
    pixels = np.random.randint(low=0, high=256, size=(100, 100, 3), dtype=np.uint8)
    pil_image = PILImage.fromarray(pixels, mode="RGB")
    image = wandb.Image(pil_image, caption=f"random field {i}")
    examples.append(image)
wandb.log({"examples": examples})
```

### Video from numpy
<!--yeadoc-test:init-and-log-video-numpy-->
```python
import numpy as np
import wandb

wandb.init()
# axes are (time, channel, height, width)
frames = np.random.randint(low=0, high=256, size=(10, 3, 100, 100), dtype=np.uint8)
wandb.log({"video": wandb.Video(frames, fps=4)})
```

### Matplotlib Plot
<!--yeadoc-test:init-and-log-matplotlib-->
```python
from matplotlib import pyplot as plt
import numpy as np
import wandb

wandb.init()
fig, ax = plt.subplots()
x = np.linspace(0, 10)
y = x * x
ax.plot(x, y)  # plot y = x^2
wandb.log({"chart": fig})
```

### PR Curve
```python
wandb.log({'pr': wandb.plots.precision_recall(y_test, y_probas, labels)})
```

### 3D Object
```python
wandb.log({"generated_samples":
[wandb.Object3D(open("sample.obj")),
    wandb.Object3D(open("sample.gltf")),
    wandb.Object3D(open("sample.glb"))]})
```



| Raises |  |
| :--- | :--- |
|  `wandb.Error` |  if called before `wandb.init` |
|  `ValueError` |  if invalid data is passed |

