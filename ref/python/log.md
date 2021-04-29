# wandb.log

[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/v0.10.28/wandb/sdk/wandb_run.py#L923-L1087)

Log a dict to the global run's history.

```text
log(
    data: Dict[str, Any],
    step: int = None,
    commit: bool = None,
    sync: bool = None
) -> None
```

Use `wandb.log` to log data from runs, such as scalars, images, video, histograms, and matplotlib plots.

The most basic usage is `wandb.log({'train-loss': 0.5, 'accuracy': 0.9})`. This will save a history row associated with the run with `train-loss=0.5` and `accuracy=0.9`. Visualize logged data in the workspace at wandb.ai, or locally on a self-hosted instance of the W&B app: [https://docs.wandb.ai/self-hosted](https://docs.wandb.ai/self-hosted)

Export data to explore in a Jupyter notebook, for example, with the API: [https://docs.wandb.ai/ref/public-api](https://docs.wandb.ai/ref/public-api)

Each time you call wandb.log\(\), this adds a new row to history and updates the summary values for each key logged. In the UI, summary values show up in the run table to compare single values across runs. You might want to update summary manually to set the _best_ value instead of the _last_ value for a given metric. After you finish logging, you can set summary: `wandb.run.summary["accuracy"] = 0.9`.

Logged values don't have to be scalars. Logging any wandb object is supported. For example `wandb.log({"example": wandb.Image("myimage.jpg")})` will log an example image which will be displayed nicely in the wandb UI. See [https://docs.wandb.com/library/reference/data\_types](https://docs.wandb.com/library/reference/data_types) for all of the different supported types.

Logging nested metrics is encouraged and is supported in the wandb API, so you could log multiple accuracy values with `wandb.log({'dataset-1': {'acc': 0.9, 'loss': 0.3} ,'dataset-2': {'acc': 0.8, 'loss': 0.2}})` and the metrics will be organized in the wandb UI.

W&B keeps track of a global step so logging related metrics together is encouraged, so by default each time wandb.log is called a global step is incremented. If it's inconvenient to log related metrics together calling `wandb.log({'train-loss': 0.5, commit=False})` and then `wandb.log({'accuracy': 0.9})` is equivalent to calling `wandb.log({'train-loss': 0.5, 'accuracy': 0.9})`

wandb.log is not intended to be called more than a few times per second. If you want to log more frequently than that it's better to aggregate the data on the client side or you may get degraded performance.

| Arguments |  |
| :--- | :--- |
|  `row` |  \(dict, optional\) A dict of serializable python objects i.e `str`, `ints`, `floats`, `Tensors`, `dicts`, or `wandb.data_types`. |
|  `commit` |  \(boolean, optional\) Save the metrics dict to the wandb server and increment the step. If false `wandb.log` just updates the current metrics dict with the row argument and metrics won't be saved until `wandb.log` is called with `commit=True`. |
|  `step` |  \(integer, optional\) The global step in processing. This persists any non-committed earlier steps but defaults to not committing the specified step. |
|  `sync` |  \(boolean, True\) This argument is deprecated and currently doesn't change the behaviour of `wandb.log`. |

## Examples:

Basic usage

```python
wandb.log({'accuracy': 0.9, 'epoch': 5})
```

Incremental logging

```python
wandb.log({'loss': 0.2}, commit=False)
# Somewhere else when I'm ready to report this step:
wandb.log({'accuracy': 0.8})
```

Histogram

```python
wandb.log({"gradients": wandb.Histogram(numpy_array_or_sequence)})
```

Image

```python
wandb.log({"examples": [wandb.Image(numpy_array_or_pil, caption="Label")]})
```

Video

```python
wandb.log({"video": wandb.Video(numpy_array_or_video_path, fps=4,
    format="gif")})
```

Matplotlib Plot

```python
wandb.log({"chart": plt})
```

PR Curve

```python
wandb.log({'pr': wandb.plots.precision_recall(y_test, y_probas, labels)})
```

3D Object

```python
wandb.log({"generated_samples":
[wandb.Object3D(open("sample.obj")),
    wandb.Object3D(open("sample.gltf")),
    wandb.Object3D(open("sample.glb"))]})
```

For more examples, see [https://docs.wandb.com/library/log](https://docs.wandb.com/library/log)

| Raises |  |
| :--- | :--- |
|  `wandb.Error` |  if called before `wandb.init` |
|  `ValueError` |  if invalid data is passed |

