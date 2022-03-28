# Run



[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/latest/wandb/sdk/wandb_run.py#L184-L2788)



A unit of computation logged by wandb. Typically this is an ML experiment.

```python
Run(
    settings: Settings,
    config: Optional[Dict[str, Any]] = None,
    sweep_config: Optional[Dict[str, Any]] = None
) -> None
```




Create a run with `wandb.init()`:
<!--yeadoc-test:run-object-basic-->
```python
import wandb

run = wandb.init()
```

There is only ever at most one active `wandb.Run` in any process,
and it is accessible as `wandb.run`:
<!--yeadoc-test:global-run-object-->
```python
import wandb

assert wandb.run is None

wandb.init()

assert wandb.run is not None
```
anything you log with `wandb.log` will be sent to that run.

If you want to start more runs in the same script or notebook, you'll need to
finish the run that is in-flight. Runs can be finished with `wandb.finish` or
by using them in a `with` block:
<!--yeadoc-test:run-context-manager-->
```python
import wandb

wandb.init()
wandb.finish()

assert wandb.run is None

with wandb.init() as run:
    pass  # log data here

assert wandb.run is None
```

See the documentation for `wandb.init` for more on creating runs, or check out
[our guide to `wandb.init`](https://docs.wandb.ai/guides/track/launch).

In distributed training, you can either create a single run in the rank 0 process
and then log information only from that process or you can create a run in each process,
logging from each separately, and group the results together with the `group` argument
to `wandb.init`. For more details on distributed training with W&B, check out
[our guide](https://docs.wandb.ai/guides/track/advanced/distributed-training).

Currently there is a parallel `Run` object in the `wandb.Api`. Eventually these
two objects will be merged.



| Attributes |  |
| :--- | :--- |
|  `history` |  (History) Time series values, created with `wandb.log()`. History can contain scalar values, rich media, or even custom plots across multiple steps. |
|  `summary` |  (Summary) Single values set for each `wandb.log()` key. By default, summary is set to the last value logged. You can manually set summary to the best value, like max accuracy, instead of the final value. |
|  `config` |  Returns the config object associated with this run. |
|  `dir` |  Returns the directory where files associated with the run are saved. |
|  `entity` |  Returns the name of the W&B entity associated with the run. Entity can be a user name or the name of a team or organization. |
|  `group` |  Returns the name of the group associated with the run. Setting a group helps the W&B UI organize runs in a sensible way. If you are doing a distributed training you should give all of the runs in the training the same group. If you are doing crossvalidation you should give all the crossvalidation folds the same group. |
|  `id` |  Returns the identifier for this run. |
|  `mode` |  For compatibility with `0.9.x` and earlier, deprecate eventually. |
|  `name` |  Returns the display name of the run. Display names are not guaranteed to be unique and may be descriptive. By default, they are randomly generated. |
|  `notes` |  Returns the notes associated with the run, if there are any. Notes can be a multiline string and can also use markdown and latex equations inside `$$`, like `$x + 3$`. |
|  `path` |  Returns the path to the run. Run paths include entity, project, and run ID, in the format `entity/project/run_id`. |
|  `project` |  Returns the name of the W&B project associated with the run. |
|  `resumed` |  Returns True if the run was resumed, False otherwise. |
|  `settings` |  Returns a frozen copy of run's Settings object. |
|  `start_time` |  Returns the unix time stamp, in seconds, when the run started. |
|  `starting_step` |  Returns the first step of the run. |
|  `step` |  Returns the current value of the step. This counter is incremented by `wandb.log`. |
|  `sweep_id` |  Returns the ID of the sweep associated with the run, if there is one. |
|  `tags` |  Returns the tags associated with the run, if there are any. |
|  `url` |  Returns the W&B url associated with the run. |



## Methods

<h3 id="alert"><code>alert</code></h3>

[View source](https://www.github.com/wandb/client/tree/latest/wandb/sdk/wandb_run.py#L2736-L2767)

```python
alert(
    title: str,
    text: str,
    level: Union[str, 'AlertLevel'] = None,
    wait_duration: Union[int, float, timedelta, None] = None
) -> None
```

Launch an alert with the given title and text.


| Arguments |  |
| :--- | :--- |
|  `title` |  (str) The title of the alert, must be less than 64 characters long. |
|  `text` |  (str) The text body of the alert. |
|  `level` |  (str or wandb.AlertLevel, optional) The alert level to use, either: `INFO`, `WARN`, or `ERROR`. |
|  `wait_duration` |  (int, float, or timedelta, optional) The time to wait (in seconds) before sending another alert with this title. |



<h3 id="define_metric"><code>define_metric</code></h3>

[View source](https://www.github.com/wandb/client/tree/latest/wandb/sdk/wandb_run.py#L2239-L2331)

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


| Arguments |  |
| :--- | :--- |
|  `name` |  Name of the metric. |
|  `step_metric` |  Independent variable associated with the metric. |
|  `step_sync` |  Automatically add `step_metric` to history if needed. Defaults to True if step_metric is specified. |
|  `hidden` |  Hide this metric from automatic plots. |
|  `summary` |  Specify aggregate metrics added to summary. Supported aggregations: "min,max,mean,best,last,none" Default aggregation is `copy` Aggregation `best` defaults to `goal`==`minimize` |
|  `goal` |  Specify direction for optimizing the metric. Supported direections: "minimize,maximize" |



| Returns |  |
| :--- | :--- |
|  A metric object is returned that can be further specified. |



<h3 id="display"><code>display</code></h3>

[View source](https://www.github.com/wandb/client/tree/latest/wandb/sdk/wandb_run.py#L936-L943)

```python
display(
    height: int = 420,
    hidden: bool = (False)
) -> bool
```

Displays this run in jupyter.


<h3 id="finish"><code>finish</code></h3>

[View source](https://www.github.com/wandb/client/tree/latest/wandb/sdk/wandb_run.py#L1454-L1487)

```python
finish(
    exit_code: int = None,
    quiet: Optional[bool] = None
) -> None
```

Marks a run as finished, and finishes uploading all data.

This is used when creating multiple runs in the same process. We automatically
call this method when your script exits or if you use the run context manager.

| Arguments |  |
| :--- | :--- |
|  `exit_code` |  Set to something other than 0 to mark a run as failed |
|  `quiet` |  Set to true to minimize log output |



<h3 id="finish_artifact"><code>finish_artifact</code></h3>

[View source](https://www.github.com/wandb/client/tree/latest/wandb/sdk/wandb_run.py#L2559-L2609)

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

| Arguments |  |
| :--- | :--- |
|  `artifact_or_path` |  (str or Artifact) A path to the contents of this artifact, can be in the following forms: - `/local/directory` - `/local/directory/file.txt` - `s3://bucket/path` You can also pass an Artifact object created by calling `wandb.Artifact`. |
|  `name` |  (str, optional) An artifact name. May be prefixed with entity/project. Valid names can be in the following forms: - name:version - name:alias - digest This will default to the basename of the path prepended with the current run id if not specified. |
|  `type` |  (str) The type of artifact to log, examples include `dataset`, `model` |
|  `aliases` |  (list, optional) Aliases to apply to this artifact, defaults to `["latest"]` |
|  `distributed_id` |  (string, optional) Unique string that all distributed jobs share. If None, defaults to the run's group name. |



| Returns |  |
| :--- | :--- |
|  An `Artifact` object. |



<h3 id="get_project_url"><code>get_project_url</code></h3>

[View source](https://www.github.com/wandb/client/tree/latest/wandb/sdk/wandb_run.py#L820-L828)

```python
get_project_url() -> Optional[str]
```

Returns the url for the W&B project associated with the run, if there is one.

Offline runs will not have a project url.

<h3 id="get_sweep_url"><code>get_sweep_url</code></h3>

[View source](https://www.github.com/wandb/client/tree/latest/wandb/sdk/wandb_run.py#L830-L835)

```python
get_sweep_url() -> Optional[str]
```

Returns the url for the sweep associated with the run, if there is one.


<h3 id="get_url"><code>get_url</code></h3>

[View source](https://www.github.com/wandb/client/tree/latest/wandb/sdk/wandb_run.py#L810-L818)

```python
get_url() -> Optional[str]
```

Returns the url for the W&B run, if there is one.

Offline runs will not have a url.

<h3 id="join"><code>join</code></h3>

[View source](https://www.github.com/wandb/client/tree/latest/wandb/sdk/wandb_run.py#L1489-L1497)

```python
join(
    exit_code: int = None
) -> None
```

Deprecated alias for `finish()` - please use finish.


<h3 id="log"><code>log</code></h3>

[View source](https://www.github.com/wandb/client/tree/latest/wandb/sdk/wandb_run.py#L1125-L1351)

```python
log(
    data: Dict[str, Any],
    step: int = None,
    commit: bool = None,
    sync: bool = None
) -> None
```

Logs a dictonary of data to the current run's history.

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



<h3 id="log_artifact"><code>log_artifact</code></h3>

[View source](https://www.github.com/wandb/client/tree/latest/wandb/sdk/wandb_run.py#L2472-L2505)

```python
log_artifact(
    artifact_or_path: Union[wandb_artifacts.Artifact, str],
    name: Optional[str] = None,
    type: Optional[str] = None,
    aliases: Optional[List[str]] = None
) -> wandb_artifacts.Artifact
```

Declare an artifact as an output of a run.


| Arguments |  |
| :--- | :--- |
|  `artifact_or_path` |  (str or Artifact) A path to the contents of this artifact, can be in the following forms: - `/local/directory` - `/local/directory/file.txt` - `s3://bucket/path` You can also pass an Artifact object created by calling `wandb.Artifact`. |
|  `name` |  (str, optional) An artifact name. May be prefixed with entity/project. Valid names can be in the following forms: - name:version - name:alias - digest This will default to the basename of the path prepended with the current run id if not specified. |
|  `type` |  (str) The type of artifact to log, examples include `dataset`, `model` |
|  `aliases` |  (list, optional) Aliases to apply to this artifact, defaults to `["latest"]` |



| Returns |  |
| :--- | :--- |
|  An `Artifact` object. |



<h3 id="log_code"><code>log_code</code></h3>

[View source](https://www.github.com/wandb/client/tree/latest/wandb/sdk/wandb_run.py#L754-L808)

```python
log_code(
    root: str = ".",
    name: str = None,
    include_fn: Callable[[str], bool] = (lambda path: path.endswith(".py")),
    exclude_fn: Callable[[str], bool] = filenames.exclude_wandb_fn
) -> Optional[Artifact]
```

Saves the current state of your code to a W&B Artifact.

By default it walks the current directory and logs all files that end with `.py`.

| Arguments |  |
| :--- | :--- |
|  `root` |  The relative (to `os.getcwd()`) or absolute path to recursively find code from. |
|  `name` |  (str, optional) The name of our code artifact. By default we'll name the artifact `source-$RUN_ID`. There may be scenarios where you want many runs to share the same artifact. Specifying name allows you to achieve that. |
|  `include_fn` |  A callable that accepts a file path and returns True when it should be included and False otherwise. This defaults to: `lambda path: path.endswith(".py")` |
|  `exclude_fn` |  A callable that accepts a file path and returns `True` when it should be excluded and `False` otherwise. Thisdefaults to: `lambda path: False` |



#### Examples:

Basic usage
```python
run.log_code()
```

Advanced usage
```python
run.log_code("../", include_fn=lambda path: path.endswith(".py") or path.endswith(".ipynb"))
```



| Returns |  |
| :--- | :--- |
|  An `Artifact` object if code was logged |



<h3 id="mark_preempting"><code>mark_preempting</code></h3>

[View source](https://www.github.com/wandb/client/tree/latest/wandb/sdk/wandb_run.py#L2782-L2788)

```python
mark_preempting() -> None
```

Marks this run as preempting.

Also tells the internal process to immediately report this to server.

<h3 id="plot_table"><code>plot_table</code></h3>

[View source](https://www.github.com/wandb/client/tree/latest/wandb/sdk/wandb_run.py#L1500-L1516)

```python
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



<h3 id="project_name"><code>project_name</code></h3>

[View source](https://www.github.com/wandb/client/tree/latest/wandb/sdk/wandb_run.py#L706-L708)

```python
project_name() -> str
```




<h3 id="restore"><code>restore</code></h3>

[View source](https://www.github.com/wandb/client/tree/latest/wandb/sdk/wandb_run.py#L1445-L1452)

```python
restore(
    name: str,
    run_path: Optional[str] = None,
    replace: bool = (False),
    root: Optional[str] = None
) -> Union[None, TextIO]
```

Downloads the specified file from cloud storage.

File is placed into the current directory or run directory.
By default will only download the file if it doesn't already exist.

| Arguments |  |
| :--- | :--- |
|  `name` |  the name of the file |
|  `run_path` |  optional path to a run to pull files from, i.e. `username/project_name/run_id` if wandb.init has not been called, this is required. |
|  `replace` |  whether to download the file even if it already exists locally |
|  `root` |  the directory to download the file to. Defaults to the current directory or the run directory if wandb.init was called. |



| Returns |  |
| :--- | :--- |
|  None if it can't find the file, otherwise a file object open for reading |



| Raises |  |
| :--- | :--- |
|  `wandb.CommError` |  if we can't connect to the wandb backend |
|  `ValueError` |  if the file is not found or can't find run_path |



<h3 id="save"><code>save</code></h3>

[View source](https://www.github.com/wandb/client/tree/latest/wandb/sdk/wandb_run.py#L1353-L1443)

```python
save(
    glob_str: Optional[str] = None,
    base_path: Optional[str] = None,
    policy: str = "live"
) -> Union[bool, List[str]]
```

Ensure all files matching `glob_str` are synced to wandb with the policy specified.


| Arguments |  |
| :--- | :--- |
|  `glob_str` |  (string) a relative or absolute path to a unix glob or regular path. If this isn't specified the method is a noop. |
|  `base_path` |  (string) the base path to run the glob relative to |
|  `policy` |  (string) on of `live`, `now`, or `end` - live: upload the file as it changes, overwriting the previous version - now: upload the file once now - end: only upload file when the run ends |



<h3 id="to_html"><code>to_html</code></h3>

[View source](https://www.github.com/wandb/client/tree/latest/wandb/sdk/wandb_run.py#L945-L953)

```python
to_html(
    height: int = 420,
    hidden: bool = (False)
) -> str
```

Generates HTML containing an iframe displaying the current run.


<h3 id="unwatch"><code>unwatch</code></h3>

[View source](https://www.github.com/wandb/client/tree/latest/wandb/sdk/wandb_run.py#L2338-L2339)

```python
unwatch(
    models=None
) -> None
```




<h3 id="upsert_artifact"><code>upsert_artifact</code></h3>

[View source](https://www.github.com/wandb/client/tree/latest/wandb/sdk/wandb_run.py#L2507-L2557)

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

Note that you must call run.finish_artifact() to finalize the artifact.
This is useful when distributed jobs need to all contribute to the same artifact.

| Arguments |  |
| :--- | :--- |
|  `artifact_or_path` |  (str or Artifact) A path to the contents of this artifact, can be in the following forms: - `/local/directory` - `/local/directory/file.txt` - `s3://bucket/path` You can also pass an Artifact object created by calling `wandb.Artifact`. |
|  `name` |  (str, optional) An artifact name. May be prefixed with entity/project. Valid names can be in the following forms: - name:version - name:alias - digest This will default to the basename of the path prepended with the current run id if not specified. |
|  `type` |  (str) The type of artifact to log, examples include `dataset`, `model` |
|  `aliases` |  (list, optional) Aliases to apply to this artifact, defaults to `["latest"]` |
|  `distributed_id` |  (string, optional) Unique string that all distributed jobs share. If None, defaults to the run's group name. |



| Returns |  |
| :--- | :--- |
|  An `Artifact` object. |



<h3 id="use_artifact"><code>use_artifact</code></h3>

[View source](https://www.github.com/wandb/client/tree/latest/wandb/sdk/wandb_run.py#L2388-L2470)

```python
use_artifact(
    artifact_or_name, type=None, aliases=None, use_as=None
)
```

Declare an artifact as an input to a run.

Call `download` or `file` on the returned object to get the contents locally.

| Arguments |  |
| :--- | :--- |
|  `artifact_or_name` |  (str or Artifact) An artifact name. May be prefixed with entity/project/. Valid names can be in the following forms: - name:version - name:alias - digest You can also pass an Artifact object created by calling `wandb.Artifact` |
|  `type` |  (str, optional) The type of artifact to use. |
|  `aliases` |  (list, optional) Aliases to apply to this artifact |
|  `use_as` |  (string, optional) Optional string indicating what purpose the artifact was used with. Will be shown in UI. |



| Returns |  |
| :--- | :--- |
|  An `Artifact` object. |



<h3 id="watch"><code>watch</code></h3>

[View source](https://www.github.com/wandb/client/tree/latest/wandb/sdk/wandb_run.py#L2334-L2335)

```python
watch(
    models, criterion=None, log="gradients", log_freq=100, idx=None,
    log_graph=(False)
) -> None
```




<h3 id="__enter__"><code>__enter__</code></h3>

[View source](https://www.github.com/wandb/client/tree/latest/wandb/sdk/wandb_run.py#L2769-L2770)

```python
__enter__() -> "Run"
```




<h3 id="__exit__"><code>__exit__</code></h3>

[View source](https://www.github.com/wandb/client/tree/latest/wandb/sdk/wandb_run.py#L2772-L2780)

```python
__exit__(
    exc_type: Type[BaseException],
    exc_val: BaseException,
    exc_tb: TracebackType
) -> bool
```






