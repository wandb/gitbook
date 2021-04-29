# wandb.Run

[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/v0.10.28/wandb/sdk/wandb_run.py#L208-L2349)

A unit of computation logged by wandb. Typically this is an ML experiment.

```text
Run(
    settings: Settings,
    config: Optional[Dict[str, Any]] = None,
    sweep_config: Optional[Dict[str, Any]] = None
) -> None
```

Create a run with `wandb.init()`.

In distributed training, use `wandb.init()` to create a run for each process, and set the group argument to organize runs into a larger experiment.

Currently there is a parallel Run object in the wandb.Api. Eventually these two objects will be merged.

| Attributes |  |
| :--- | :--- |
|  `history` |  \(History\) Time series values, created with `wandb.log()`. History can contain scalar values, rich media, or even custom plots across multiple steps. |
|  `summary` |  \(Summary\) Single values set for each `wandb.log()` key. By default, summary is set to the last value logged. You can manually set summary to the best value, like max accuracy, instead of the final value. |
|  `config` |  Returns: \(Config\): A config object \(similar to a nested dict\) of key value pairs associated with the hyperparameters of the run. |
|  `dir` |  Returns: \(str\): The directory where all of the files associated with the run are placed. |
|  `entity` |  Returns: \(str\): name of W&B entity associated with run. Entity is either a user name or an organization name. |
|  `group` |  Setting a group helps the W&B UI organize runs in a sensible way. If you are doing a distributed training you should give all of the runs in the training the same group. If you are doing crossvalidation you should give all the crossvalidation folds the same group. |
|  `id` |  id property. |
|  `mode` |  For compatibility with `0.9.x` and earlier, deprecate eventually. |
|  `name` |  Returns: \(str\): the display name of the run. It does not need to be unique and ideally is descriptive. |
|  `notes` |  Returns: \(str\): notes associated with the run. Notes can be a multiline string and can also use markdown and latex equations inside $$ like $\\{x} |
|  `path` |  Returns: \(str\): the path to the run \`\[entity\]/\[project\]/\[run\_id\]\` |
|  `project` |  Returns: \(str\): name of W&B project associated with run. |
|  `resumed` |  Returns: \(bool\): whether or not the run was resumed |
|  `start_time` |  Returns: \(int\): the unix time stamp in seconds when the run started |
|  `starting_step` |  Returns: \(int\): the first step of the run |
|  `step` |  Every time you call wandb.log\(\) it will by default increment the step counter. |
|  `sweep_id` |  Returns: \(str, optional\): the sweep id associated with the run or None |
|  `tags` |  Returns: \(Tuple\[str\]\): tags associated with the run |
|  `url` |  Returns: \(str\): name of W&B url associated with run. |

## Methods

### `alert` <a id="alert"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.28/wandb/sdk/wandb_run.py#L2300-L2336)

```text
alert(
    title: str,
    text: str,
    level: Union[str, None] = None,
    wait_duration: Union[int, float, timedelta, None] = None
) -> None
```

Launch an alert with the given title and text.

| Arguments |  |
| :--- | :--- |
|  `title` |  \(str\) The title of the alert, must be less than 64 characters long. |
|  `text` |  \(str\) The text body of the alert. |
|  `level` |  \(str or wandb.AlertLevel, optional\) The alert level to use, either: `INFO`, `WARN`, or `ERROR`. |
|  `wait_duration` |  \(int, float, or timedelta, optional\) The time to wait \(in seconds\) before sending another alert with this title. |

### `define_metric` <a id="define_metric"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.28/wandb/sdk/wandb_run.py#L1914-L2006)

```text
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

| Arguments |  |
| :--- | :--- |
|  `name` |  Name of the metric. |
|  `step_metric` |  Independent variable associated with the metric. |
|  `step_sync` |  Automatically add `step_metric` to history if needed. Defaults to True if step\_metric is specified. |
|  `hidden` |  Hide this metric from automatic plots. |
|  `summary` |  Specify aggregate metrics added to summary. Supported aggregations: "min,max,mean,best,last,none" Default aggregation is `copy` Aggregation `best` defaults to `goal`==`minimize` |
|  `goal` |  Specify direction for optimizing the metric. Supported direections: "minimize,maximize" |

| Returns |
| :--- |
|  A metric object is returned that can be further specified. |

### `finish` <a id="finish"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.28/wandb/sdk/wandb_run.py#L1189-L1204)

```text
finish(
    exit_code: int = None
) -> None
```

Marks a run as finished, and finishes uploading all data. This is used when creating multiple runs in the same process. We automatically call this method when your script exits.

### `finish_artifact` <a id="finish_artifact"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.28/wandb/sdk/wandb_run.py#L2149-L2198)

```text
finish_artifact(
    artifact_or_path: Union[wandb_artifacts.Artifact, str],
    name: Optional[str] = None,
    type: Optional[str] = None,
    aliases: Optional[List[str]] = None,
    distributed_id: Optional[str] = None
) -> wandb_artifacts.Artifact
```

Finish a non-finalized artifact as output of a run. Subsequent "upserts" with the same distributed ID will result in a new version

| Arguments |  |
| :--- | :--- |
|  `artifact_or_path` |  \(str or Artifact\) A path to the contents of this artifact, can be in the following forms: - \`/local/directory\` - \`/local/directory/file.txt\` - \`s3://bucket/path\` You can also pass an Artifact object created by calling `wandb.Artifact`. |
|  `name` |  \(str, optional\) An artifact name. May be prefixed with entity/project. Valid names can be in the following forms: - name:version - name:alias - digest This will default to the basename of the path prepended with the current run id if not specified. |
|  `type` |  \(str\) The type of artifact to log, examples include `dataset`, `model` |
|  `aliases` |  \(list, optional\) Aliases to apply to this artifact, defaults to \`\["latest"\]\` |
|  `distributed_id` |  \(string, optional\) Unique string that all distributed jobs share. If None, defaults to the run's group name. |

| Returns |
| :--- |
|  An `Artifact` object. |

### `get_project_url` <a id="get_project_url"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.28/wandb/sdk/wandb_run.py#L724-L733)

```text
get_project_url() -> Optional[str]
```

Returns: A url \(str, optional\) for the W&B project associated with the run or None if the run is offline

### `get_sweep_url` <a id="get_sweep_url"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.28/wandb/sdk/wandb_run.py#L735-L744)

```text
get_sweep_url() -> Optional[str]
```

Returns: A url \(str, optional\) for the sweep associated with the run or None if there is no associated sweep or the run is offline.

### `get_url` <a id="get_url"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.28/wandb/sdk/wandb_run.py#L713-L722)

```text
get_url() -> Optional[str]
```

Returns: A url \(str, optional\) for the W&B run or None if the run is offline

### `join` <a id="join"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.28/wandb/sdk/wandb_run.py#L1206-L1208)

```text
join(
    exit_code: int = None
) -> None
```

Deprecated alias for `finish()` - please use finish

### `log` <a id="log"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.28/wandb/sdk/wandb_run.py#L923-L1087)

```text
log(
    data: Dict[str, Any],
    step: int = None,
    commit: bool = None,
    sync: bool = None
) -> None
```

Log a dict to the global run's history.

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

#### Examples:

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

### `log_artifact` <a id="log_artifact"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.28/wandb/sdk/wandb_run.py#L2065-L2096)

```text
log_artifact(
    artifact_or_path: Union[wandb_artifacts.Artifact, str],
    name: Optional[str] = None,
    type: Optional[str] = None,
    aliases: Optional[List[str]] = None
) -> wandb_artifacts.Artifact
```

Declare an artifact as output of a run.

| Arguments |  |
| :--- | :--- |
|  `artifact_or_path` |  \(str or Artifact\) A path to the contents of this artifact, can be in the following forms: - \`/local/directory\` - \`/local/directory/file.txt\` - \`s3://bucket/path\` You can also pass an Artifact object created by calling `wandb.Artifact`. |
|  `name` |  \(str, optional\) An artifact name. May be prefixed with entity/project. Valid names can be in the following forms: - name:version - name:alias - digest This will default to the basename of the path prepended with the current run id if not specified. |
|  `type` |  \(str\) The type of artifact to log, examples include `dataset`, `model` |
|  `aliases` |  \(list, optional\) Aliases to apply to this artifact, defaults to \`\["latest"\]\` |

| Returns |
| :--- |
|  An `Artifact` object. |

### `log_code` <a id="log_code"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.28/wandb/sdk/wandb_run.py#L654-L711)

```text
log_code(
    root: str = '.',
    name: str = None,
    include_fn: Callable[[str], bool] = (lambda path: path.endswith('.py')),
    exclude_fn: Callable[[str], bool] = (lambda path: os.sep + 'wandb' + os.sep in path)
) -> Optional[Artifact]
```

log\_code\(\) saves the current state of your code to a W&B artifact. By default it walks the current directory and logs all files that end with ".py".

| Arguments |
| :--- |
|  root \(str, optional\): The relative \(to os.getcwd\(\)\) or absolute path to recursively find code from. name \(str, optional\): The name of our code artifact. By default we'll name the artifact "source-$RUN\_ID". There may be scenarios where you want many runs to share the same artifact. Specifying name allows you to achieve that. include\_fn \(callable, optional\): A callable that accepts a file path and returns True when it should be included and False otherwise. This defaults to: \`lambda path: path.endswith\(".py"\)\` exclude\_fn \(callable, optional\): A callable that accepts a file path and returns True when it should be excluded and False otherwise. This defaults to: \`lambda path: False\` |

#### Examples:

Basic usage

```python
run.log_code()
```

Advanced usage

```python
run.log_code("../", include_fn=lambda path: path.endswith(".py") or path.endswith(".ipynb"))
```

| Returns |
| :--- |
|  An `Artifact` object if code was logged |

### `plot_table` <a id="plot_table"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.28/wandb/sdk/wandb_run.py#L1211-L1226)

```text
plot_table(
    vega_spec_name, data_table, fields, string_fields=None
)
```

Creates a custom plot on a table.

| Arguments |  |
| :--- | :--- |
|  `vega_spec_name` |  the name of the spec for the plot |
|  `table_key` |  the key used to log the data table |
|  `data_table` |  a wandb.Table object containing the data to be used on the visualization |
|  `fields` |  a dict mapping from table keys to fields that the custom visualization needs |
|  `string_fields` |  a dict that provides values for any string constants the custom visualization needs |

### `project_name` <a id="project_name"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.28/wandb/sdk/wandb_run.py#L608-L610)

```text
project_name() -> str
```

### `restore` <a id="restore"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.28/wandb/sdk/wandb_run.py#L1180-L1187)

```text
restore(
    name: str,
    run_path: Optional[str] = None,
    replace: bool = (False),
    root: Optional[str] = None
) -> Union[None, TextIO]
```

Downloads the specified file from cloud storage into the current directory or run directory. By default this will only download the file if it doesn't already exist.

| Arguments |  |
| :--- | :--- |
|  `name` |  the name of the file |
|  `run_path` |  optional path to a run to pull files from, i.e. \`username/project\_name/run\_id\` if wandb.init has not been called, this is required. |
|  `replace` |  whether to download the file even if it already exists locally |
|  `root` |  the directory to download the file to. Defaults to the current directory or the run directory if wandb.init was called. |

| Returns |
| :--- |
|  None if it can't find the file, otherwise a file object open for reading |

| Raises |  |
| :--- | :--- |
|  `wandb.CommError` |  if we can't connect to the wandb backend |
|  `ValueError` |  if the file is not found or can't find run\_path |

### `save` <a id="save"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.28/wandb/sdk/wandb_run.py#L1089-L1178)

```text
save(
    glob_str: Optional[str] = None,
    base_path: Optional[str] = None,
    policy: str = 'live'
) -> Union[bool, List[str]]
```

Ensure all files matching _glob\_str_ are synced to wandb with the policy specified.

| Arguments |  |
| :--- | :--- |
|  `glob_str` |  \(string\) a relative or absolute path to a unix glob or regular path. If this isn't specified the method is a noop. |
|  `base_path` |  \(string\) the base path to run the glob relative to |
|  `policy` |  \(string\) on of `live`, `now`, or `end` - live: upload the file as it changes, overwriting the previous version - now: upload the file once now - end: only upload file when the run ends |

### `upsert_artifact` <a id="upsert_artifact"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.28/wandb/sdk/wandb_run.py#L2098-L2147)

```text
upsert_artifact(
    artifact_or_path: Union[wandb_artifacts.Artifact, str],
    name: Optional[str] = None,
    type: Optional[str] = None,
    aliases: Optional[List[str]] = None,
    distributed_id: Optional[str] = None
) -> wandb_artifacts.Artifact
```

Declare \(or append tp\) a non-finalized artifact as output of a run. Note that you must call run.finish\_artifact\(\) to finalize the artifact. This is useful when distributed jobs need to all contribute to the same artifact.

| Arguments |  |
| :--- | :--- |
|  `artifact_or_path` |  \(str or Artifact\) A path to the contents of this artifact, can be in the following forms: - \`/local/directory\` - \`/local/directory/file.txt\` - \`s3://bucket/path\` You can also pass an Artifact object created by calling `wandb.Artifact`. |
|  `name` |  \(str, optional\) An artifact name. May be prefixed with entity/project. Valid names can be in the following forms: - name:version - name:alias - digest This will default to the basename of the path prepended with the current run id if not specified. |
|  `type` |  \(str\) The type of artifact to log, examples include `dataset`, `model` |
|  `aliases` |  \(list, optional\) Aliases to apply to this artifact, defaults to \`\["latest"\]\` |
|  `distributed_id` |  \(string, optional\) Unique string that all distributed jobs share. If None, defaults to the run's group name. |

| Returns |
| :--- |
|  An `Artifact` object. |

### `use_artifact` <a id="use_artifact"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.28/wandb/sdk/wandb_run.py#L2013-L2063)

```text
use_artifact(
    artifact_or_name, type=None, aliases=None
)
```

Declare an artifact as an input to a run, call `download` or `file` on the returned object to get the contents locally.

| Arguments |  |
| :--- | :--- |
|  `artifact_or_name` |  \(str or Artifact\) An artifact name. May be prefixed with entity/project. Valid names can be in the following forms: - name:version - name:alias - digest You can also pass an Artifact object created by calling `wandb.Artifact` |
|  `type` |  \(str, optional\) The type of artifact to use. |
|  `aliases` |  \(list, optional\) Aliases to apply to this artifact |

| Returns |
| :--- |
|  An `Artifact` object. |

### `watch` <a id="watch"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.28/wandb/sdk/wandb_run.py#L2009-L2010)

```text
watch(
    models, criterion=None, log='gradients', log_freq=100, idx=None
) -> None
```

### `__enter__` <a id="__enter__"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.28/wandb/sdk/wandb_run.py#L2338-L2339)

```text
__enter__() -> "Run"
```

### `__exit__` <a id="__exit__"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.28/wandb/sdk/wandb_run.py#L2341-L2349)

```text
__exit__(
    exc_type: Type[BaseException],
    exc_val: BaseException,
    exc_tb: TracebackType
) -> bool
```

