# wandb.Run

[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/v0.12.2/wandb/sdk/wandb_run.py#L221-L2453)

A unit of computation logged by wandb. Typically this is an ML experiment.

```python
Run(
    settings: Settings,
    config: Optional[Dict[str, Any]] = None,
    sweep_config: Optional[Dict[str, Any]] = None
) -> None
```

Create a run with `wandb.init()`.

In distributed training, use `wandb.init()` to create a run for each process, and set the group argument to organize runs into a larger experiment.

Currently there is a parallel Run object in the wandb.Api. Eventually these two objects will be merged.

| Attributes      |                                                                                                                                                                                                                                                                                                                                  |
| --------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `history`       | (History) Time series values, created with `wandb.log()`. History can contain scalar values, rich media, or even custom plots across multiple steps.                                                                                                                                                                             |
| `summary`       | (Summary) Single values set for each `wandb.log()` key. By default, summary is set to the last value logged. You can manually set summary to the best value, like max accuracy, instead of the final value.                                                                                                                      |
| `config`        | Returns the config object associated with this run.                                                                                                                                                                                                                                                                              |
| `dir`           | Returns the directory where files associated with the run are saved.                                                                                                                                                                                                                                                             |
| `entity`        | Returns the name of the W\&B entity associated with the run. Entity can be a user name or the name of a team or organization.                                                                                                                                                                                                    |
| `group`         | Returns the name of the group associated with the run. Setting a group helps the W\&B UI organize runs in a sensible way. If you are doing a distributed training you should give all of the runs in the training the same group. If you are doing crossvalidation you should give all the crossvalidation folds the same group. |
| `id`            | Returns the identifier for this run.                                                                                                                                                                                                                                                                                             |
| `mode`          | For compatibility with `0.9.x` and earlier, deprecate eventually.                                                                                                                                                                                                                                                                |
| `name`          | Returns the display name of the run. Display names are not guaranteed to be unique and may be descriptive. By default, they are randomly generated.                                                                                                                                                                              |
| `notes`         | Returns the notes associated with the run, if there are any. Notes can be a multiline string and can also use markdown and latex equations inside `$$`, like `$x + 3$`.                                                                                                                                                          |
| `path`          | Returns the path to the run. Run paths include entity, project, and run ID, in the format `entity/project/run_id`.                                                                                                                                                                                                               |
| `project`       | Returns the name of the W\&B project associated with the run.                                                                                                                                                                                                                                                                    |
| `resumed`       | Returns True if the run was resumed, False otherwise.                                                                                                                                                                                                                                                                            |
| `start_time`    | Returns the unix time stamp, in seconds, when the run started.                                                                                                                                                                                                                                                                   |
| `starting_step` | Returns the first step of the run.                                                                                                                                                                                                                                                                                               |
| `step`          | Returns the current value of the step. This counter is incremented by `wandb.log`.                                                                                                                                                                                                                                               |
| `sweep_id`      | Returns the ID of the sweep associated with the run, if there is one.                                                                                                                                                                                                                                                            |
| `tags`          | Returns the tags associated with the run, if there are any.                                                                                                                                                                                                                                                                      |
| `url`           | Returns the W\&B url associated with the run.                                                                                                                                                                                                                                                                                    |

## Methods

### `alert` <a href="alert" id="alert"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.2/wandb/sdk/wandb_run.py#L2396-L2432)

```python
alert(
    title: str,
    text: str,
    level: Union[str, None] = None,
    wait_duration: Union[int, float, timedelta, None] = None
) -> None
```

Launch an alert with the given title and text.

| Arguments       |                                                                                                                  |
| --------------- | ---------------------------------------------------------------------------------------------------------------- |
| `title`         | (str) The title of the alert, must be less than 64 characters long.                                              |
| `text`          | (str) The text body of the alert.                                                                                |
| `level`         | (str or wandb.AlertLevel, optional) The alert level to use, either: `INFO`, `WARN`, or `ERROR`.                  |
| `wait_duration` | (int, float, or timedelta, optional) The time to wait (in seconds) before sending another alert with this title. |

### `define_metric` <a href="define_metric" id="define_metric"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.2/wandb/sdk/wandb_run.py#L1993-L2085)

```python
define_metric(
    name: str,
    step_metric: Union[str, wandb_metric.Metric, None] = None,
    step_sync: bool = None,
    hidden: bool = None,
    summary: str = None,
    goal: str = None,
    overwrite: bool = None,
    **kwargs
) -> wandb_metric.Metric
```

Define metric properties which will later be logged with `wandb.log()`.

| Arguments     |                                                                                                                                                                                   |
| ------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `name`        | Name of the metric.                                                                                                                                                               |
| `step_metric` | Independent variable associated with the metric.                                                                                                                                  |
| `step_sync`   | Automatically add `step_metric` to history if needed. Defaults to True if step_metric is specified.                                                                               |
| `hidden`      | Hide this metric from automatic plots.                                                                                                                                            |
| `summary`     | Specify aggregate metrics added to summary. Supported aggregations: "min,max,mean,best,last,none" Default aggregation is `copy` Aggregation `best` defaults to `goal`==`minimize` |
| `goal`        | Specify direction for optimizing the metric. Supported direections: "minimize,maximize"                                                                                           |

| Returns                                                    |   |
| ---------------------------------------------------------- | - |
| A metric object is returned that can be further specified. |   |

### `finish` <a href="finish" id="finish"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.2/wandb/sdk/wandb_run.py#L1266-L1282)

```python
finish(
    exit_code: int = None
) -> None
```

Marks a run as finished, and finishes uploading all data.

This is used when creating multiple runs in the same process. We automatically call this method when your script exits or if you use the run context manager.

### `finish_artifact` <a href="finish_artifact" id="finish_artifact"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.2/wandb/sdk/wandb_run.py#L2233-L2283)

```python
finish_artifact(
    artifact_or_path: Union[wandb_artifacts.Artifact, str],
    name: Optional[str] = None,
    type: Optional[str] = None,
    aliases: Optional[List[str]] = None,
    distributed_id: Optional[str] = None
) -> wandb_artifacts.Artifact
```

Finishes a non-finalized artifact as output of a run.

Subsequent "upserts" with the same distributed ID will result in a new version.

| Arguments          |                                                                                                                                                                                                                                                          |
| ------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `artifact_or_path` | (str or Artifact) A path to the contents of this artifact, can be in the following forms: - `/local/directory` - `/local/directory/file.txt` - `s3://bucket/path` You can also pass an Artifact object created by calling `wandb.Artifact`.              |
| `name`             | (str, optional) An artifact name. May be prefixed with entity/project. Valid names can be in the following forms: - name:version - name:alias - digest This will default to the basename of the path prepended with the current run id if not specified. |
| `type`             | (str) The type of artifact to log, examples include `dataset`, `model`                                                                                                                                                                                   |
| `aliases`          | (list, optional) Aliases to apply to this artifact, defaults to `["latest"]`                                                                                                                                                                             |
| `distributed_id`   | (string, optional) Unique string that all distributed jobs share. If None, defaults to the run's group name.                                                                                                                                             |

| Returns               |   |
| --------------------- | - |
| An `Artifact` object. |   |

### `get_project_url` <a href="get_project_url" id="get_project_url"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.2/wandb/sdk/wandb_run.py#L719-L727)

```python
get_project_url() -> Optional[str]
```

Returns the url for the W\&B project associated with the run, if there is one.

Offline runs will not have a project url.

### `get_sweep_url` <a href="get_sweep_url" id="get_sweep_url"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.2/wandb/sdk/wandb_run.py#L729-L734)

```python
get_sweep_url() -> Optional[str]
```

Returns the url for the sweep associated with the run, if there is one.

### `get_url` <a href="get_url" id="get_url"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.2/wandb/sdk/wandb_run.py#L709-L717)

```python
get_url() -> Optional[str]
```

Returns the url for the W\&B run, if there is one.

Offline runs will not have a url.

### `join` <a href="join" id="join"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.2/wandb/sdk/wandb_run.py#L1284-L1286)

```python
join(
    exit_code: int = None
) -> None
```

Deprecated alias for `finish()` - please use finish.

### `log` <a href="log" id="log"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.2/wandb/sdk/wandb_run.py#L995-L1164)

```python
log(
    data: Dict[str, Any],
    step: int = None,
    commit: bool = None,
    sync: bool = None
) -> None
```

Logs a dictonary of data to the current run's history.

Use `wandb.log` to log data from runs, such as scalars, images, video, histograms, plots, and tables.

See our [guides to logging](https://docs.wandb.ai/guides/track/log) for live examples, code snippets, best practices, and more.

The most basic usage is `wandb.log({"train-loss": 0.5, "accuracy": 0.9})`. This will save the loss and accuracy to the run's history and update the summary values for these metrics.

Visualize logged data in the workspace at [wandb.ai](https://wandb.ai), or locally on a [self-hosted instance](https://docs.wandb.ai/self-hosted) of the W\&B app, or export data to visualize and explore locally, e.g. in Jupyter notebooks, with [our API](https://docs.wandb.ai/guides/track/public-api-guide).

In the UI, summary values show up in the run table to compare single values across runs. Summary values can also be set directly with `wandb.run.summary["key"] = value`.

Logged values don't have to be scalars. Logging any wandb object is supported. For example `wandb.log({"example": wandb.Image("myimage.jpg")})` will log an example image which will be displayed nicely in the W\&B UI. See the [reference documentation](https://docs.wandb.com/library/reference/data_types) for all of the different supported types or check out our [guides to logging](https://docs.wandb.ai/guides/track/log) for examples, from 3D molecular structures and segmentation masks to PR curves and histograms. `wandb.Table`s can be used to logged structured data. See our [guide to logging tables](https://docs.wandb.ai/guides/data-vis/log-tables) for details.

Logging nested metrics is encouraged and is supported in the W\&B UI. If you log with a nested dictionary like `wandb.log({"train": {"acc": 0.9}, "val": {"acc": 0.8}})`, the metrics will be organized into `train` and `val` sections in the W\&B UI.

wandb keeps track of a global step, which by default increments with each call to `wandb.log`, so logging related metrics together is encouraged. If it's inconvenient to log related metrics together calling `wandb.log({"train-loss": 0.5, commit=False})` and then `wandb.log({"accuracy": 0.9})` is equivalent to calling `wandb.log({"train-loss": 0.5, "accuracy": 0.9})`.

`wandb.log` is not intended to be called more than a few times per second. If you want to log more frequently than that it's better to aggregate the data on the client side or you may get degraded performance.

| Arguments |                                                                                                                                                                                                                                                   |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `row`     | (dict, optional) A dict of serializable python objects i.e `str`, `ints`, `floats`, `Tensors`, `dicts`, or any of the `wandb.data_types`.                                                                                                         |
| `commit`  | (boolean, optional) Save the metrics dict to the wandb server and increment the step. If false `wandb.log` just updates the current metrics dict with the row argument and metrics won't be saved until `wandb.log` is called with `commit=True`. |
| `step`    | (integer, optional) The global step in processing. This persists any non-committed earlier steps but defaults to not committing the specified step.                                                                                               |
| `sync`    | (boolean, True) This argument is deprecated and currently doesn't change the behaviour of `wandb.log`.                                                                                                                                            |

#### Examples:

For more and more detailed examples, see [our guides to logging](https://docs.wandb.com/guides/track/log).

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

| Raises        |                               |
| ------------- | ----------------------------- |
| `wandb.Error` | if called before `wandb.init` |
| `ValueError`  | if invalid data is passed     |

### `log_artifact` <a href="log_artifact" id="log_artifact"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.2/wandb/sdk/wandb_run.py#L2148-L2179)

```python
log_artifact(
    artifact_or_path: Union[wandb_artifacts.Artifact, str],
    name: Optional[str] = None,
    type: Optional[str] = None,
    aliases: Optional[List[str]] = None
) -> wandb_artifacts.Artifact
```

Declare an artifact as an output of a run.

| Arguments          |                                                                                                                                                                                                                                                          |
| ------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `artifact_or_path` | (str or Artifact) A path to the contents of this artifact, can be in the following forms: - `/local/directory` - `/local/directory/file.txt` - `s3://bucket/path` You can also pass an Artifact object created by calling `wandb.Artifact`.              |
| `name`             | (str, optional) An artifact name. May be prefixed with entity/project. Valid names can be in the following forms: - name:version - name:alias - digest This will default to the basename of the path prepended with the current run id if not specified. |
| `type`             | (str) The type of artifact to log, examples include `dataset`, `model`                                                                                                                                                                                   |
| `aliases`          | (list, optional) Aliases to apply to this artifact, defaults to `["latest"]`                                                                                                                                                                             |

| Returns               |   |
| --------------------- | - |
| An `Artifact` object. |   |

### `log_code` <a href="log_code" id="log_code"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.2/wandb/sdk/wandb_run.py#L651-L707)

```python
log_code(
    root: str = ".",
    name: str = None,
    include_fn: Callable[[str], bool] = (lambda path: path.endswith(".py")),
    exclude_fn: Callable[[str], bool] = filenames.exclude_wandb_fn
) -> Optional[Artifact]
```

Saves the current state of your code to a W\&B artifact.

By default it walks the current directory and logs all files that end with `.py`.

| Arguments                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |   |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | - |
| root (str, optional): The relative (to `os.getcwd()`) or absolute path to recursively find code from. name (str, optional): The name of our code artifact. By default we'll name the artifact `source-$RUN_ID`. There may be scenarios where you want many runs to share the same artifact. Specifying name allows you to achieve that. include_fn (callable, optional): A callable that accepts a file path and returns True when it should be included and False otherwise. This defaults to: `lambda path: path.endswith(".py")` exclude_fn (callable, optional): A callable that accepts a file path and returns `True` when it should be excluded and `False` otherwise. This defaults to: `lambda path: False` |   |

#### Examples:

Basic usage

```python
run.log_code()
```

Advanced usage

```python
run.log_code("../", include_fn=lambda path: path.endswith(".py") or path.endswith(".ipynb"))
```

| Returns                                 |   |
| --------------------------------------- | - |
| An `Artifact` object if code was logged |   |

### `mark_preempting` <a href="mark_preempting" id="mark_preempting"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.2/wandb/sdk/wandb_run.py#L2447-L2453)

```python
mark_preempting() -> None
```

Marks this run as preempting.

Also tells the internal process to immediately report this to server.

### `plot_table` <a href="plot_table" id="plot_table"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.2/wandb/sdk/wandb_run.py#L1289-L1305)

```python
plot_table(
    vega_spec_name, data_table, fields, string_fields=None
)
```

Creates a custom plot on a table.

| Arguments        |                                                                                     |
| ---------------- | ----------------------------------------------------------------------------------- |
| `vega_spec_name` | the name of the spec for the plot                                                   |
| `table_key`      | the key used to log the data table                                                  |
| `data_table`     | a wandb.Table object containing the data to be used on the visualization            |
| `fields`         | a dict mapping from table keys to fields that the custom visualization needs        |
| `string_fields`  | a dict that provides values for any string constants the custom visualization needs |

### `project_name` <a href="project_name" id="project_name"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.2/wandb/sdk/wandb_run.py#L610-L612)

```python
project_name() -> str
```

### `restore` <a href="restore" id="restore"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.2/wandb/sdk/wandb_run.py#L1257-L1264)

```python
restore(
    name: str,
    run_path: Optional[str] = None,
    replace: bool = (False),
    root: Optional[str] = None
) -> Union[None, TextIO]
```

Downloads the specified file from cloud storage.

File is placed into the current directory or run directory. By default will only download the file if it doesn't already exist.

| Arguments  |                                                                                                                                     |
| ---------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| `name`     | the name of the file                                                                                                                |
| `run_path` | optional path to a run to pull files from, i.e. `username/project_name/run_id` if wandb.init has not been called, this is required. |
| `replace`  | whether to download the file even if it already exists locally                                                                      |
| `root`     | the directory to download the file to. Defaults to the current directory or the run directory if wandb.init was called.             |

| Returns                                                                  |   |
| ------------------------------------------------------------------------ | - |
| None if it can't find the file, otherwise a file object open for reading |   |

| Raises            |                                                 |
| ----------------- | ----------------------------------------------- |
| `wandb.CommError` | if we can't connect to the wandb backend        |
| `ValueError`      | if the file is not found or can't find run_path |

### `save` <a href="save" id="save"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.2/wandb/sdk/wandb_run.py#L1166-L1255)

```python
save(
    glob_str: Optional[str] = None,
    base_path: Optional[str] = None,
    policy: str = "live"
) -> Union[bool, List[str]]
```

Ensure all files matching `glob_str` are synced to wandb with the policy specified.

| Arguments   |                                                                                                                                                                                          |
| ----------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `glob_str`  | (string) a relative or absolute path to a unix glob or regular path. If this isn't specified the method is a noop.                                                                       |
| `base_path` | (string) the base path to run the glob relative to                                                                                                                                       |
| `policy`    | (string) on of `live`, `now`, or `end` - live: upload the file as it changes, overwriting the previous version - now: upload the file once now - end: only upload file when the run ends |

### `upsert_artifact` <a href="upsert_artifact" id="upsert_artifact"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.2/wandb/sdk/wandb_run.py#L2181-L2231)

```python
upsert_artifact(
    artifact_or_path: Union[wandb_artifacts.Artifact, str],
    name: Optional[str] = None,
    type: Optional[str] = None,
    aliases: Optional[List[str]] = None,
    distributed_id: Optional[str] = None
) -> wandb_artifacts.Artifact
```

Declare (or append to) a non-finalized artifact as output of a run.

Note that you must call run.finish_artifact() to finalize the artifact. This is useful when distributed jobs need to all contribute to the same artifact.

| Arguments          |                                                                                                                                                                                                                                                          |
| ------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `artifact_or_path` | (str or Artifact) A path to the contents of this artifact, can be in the following forms: - `/local/directory` - `/local/directory/file.txt` - `s3://bucket/path` You can also pass an Artifact object created by calling `wandb.Artifact`.              |
| `name`             | (str, optional) An artifact name. May be prefixed with entity/project. Valid names can be in the following forms: - name:version - name:alias - digest This will default to the basename of the path prepended with the current run id if not specified. |
| `type`             | (str) The type of artifact to log, examples include `dataset`, `model`                                                                                                                                                                                   |
| `aliases`          | (list, optional) Aliases to apply to this artifact, defaults to `["latest"]`                                                                                                                                                                             |
| `distributed_id`   | (string, optional) Unique string that all distributed jobs share. If None, defaults to the run's group name.                                                                                                                                             |

| Returns               |   |
| --------------------- | - |
| An `Artifact` object. |   |

### `use_artifact` <a href="use_artifact" id="use_artifact"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.2/wandb/sdk/wandb_run.py#L2092-L2146)

```python
use_artifact(
    artifact_or_name, type=None, aliases=None
)
```

Declare an artifact as an input to a run.

Call `download` or `file` on the returned object to get the contents locally.

| Arguments          |                                                                                                                                                                                                                                    |
| ------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `artifact_or_name` | (str or Artifact) An artifact name. May be prefixed with entity/project/. Valid names can be in the following forms: - name:version - name:alias - digest You can also pass an Artifact object created by calling `wandb.Artifact` |
| `type`             | (str, optional) The type of artifact to use.                                                                                                                                                                                       |
| `aliases`          | (list, optional) Aliases to apply to this artifact                                                                                                                                                                                 |

| Returns               |   |
| --------------------- | - |
| An `Artifact` object. |   |

### `watch` <a href="watch" id="watch"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.2/wandb/sdk/wandb_run.py#L2088-L2089)

```python
watch(
    models, criterion=None, log="gradients", log_freq=100, idx=None,
    log_graph=(False)
) -> None
```

### `__enter__` <a href="__enter__" id="__enter__"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.2/wandb/sdk/wandb_run.py#L2434-L2435)

```python
__enter__() -> "Run"
```

### `__exit__` <a href="__exit__" id="__exit__"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.2/wandb/sdk/wandb_run.py#L2437-L2445)

```python
__exit__(
    exc_type: Type[BaseException],
    exc_val: BaseException,
    exc_tb: TracebackType
) -> bool
```
