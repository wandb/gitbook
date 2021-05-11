# Run



[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/v0.10.30/wandb/sdk/wandb_run.py#L216-L2360)




A unit of computation logged by wandb. Typically this is an ML experiment.

<pre><code>Run(
    settings: Settings,
    config: Optional[Dict[str, Any]] = None,
    sweep_config: Optional[Dict[str, Any]] = None
) -> None</code></pre>




Create a run with <code>wandb.init()</code>.

In distributed training, use <code>wandb.init()</code> to create a run for
each process, and set the group argument to organize runs into a larger experiment.

Currently there is a parallel Run object in the wandb.Api. Eventually these
two objects will be merged.



<!-- Tabular view -->
<table>
<tr><th>Attributes</th></tr>

<tr>
<td>
<code>history</code>
</td>
<td>
(History) Time series values, created with <code>wandb.log()</code>.
History can contain scalar values, rich media, or even custom plots
across multiple steps.
</td>
</tr><tr>
<td>
<code>summary</code>
</td>
<td>
(Summary) Single values set for each <code>wandb.log()</code> key. By
default, summary is set to the last value logged. You can manually
set summary to the best value, like max accuracy, instead of the
final value.
</td>
</tr><tr>
<td>
<code>config</code>
</td>
<td>
Returns:
(Config): A config object (similar to a nested dict) of key
value pairs associated with the hyperparameters of the run.
</td>
</tr><tr>
<td>
<code>dir</code>
</td>
<td>
Returns:
(str): The directory where all of the files associated with the run are
placed.
</td>
</tr><tr>
<td>
<code>entity</code>
</td>
<td>
Returns:
(str): name of W&B entity associated with run. Entity is either
a user name or an organization name.
</td>
</tr><tr>
<td>
<code>group</code>
</td>
<td>
Setting a group helps the W&B UI organize runs in a sensible way.

If you are doing a distributed training you should give all of the
runs in the training the same group.
If you are doing crossvalidation you should give all the crossvalidation
folds the same group.
</td>
</tr><tr>
<td>
<code>id</code>
</td>
<td>
id property.
</td>
</tr><tr>
<td>
<code>mode</code>
</td>
<td>
For compatibility with <code>0.9.x</code> and earlier, deprecate eventually.
</td>
</tr><tr>
<td>
<code>name</code>
</td>
<td>
Returns:
(str): the display name of the run. It does not need to be unique
and ideally is descriptive.
</td>
</tr><tr>
<td>
<code>notes</code>
</td>
<td>
Returns:
(str): notes associated with the run. Notes can be a multiline string
and can also use markdown and latex equations inside $$ like $\\{x}
</td>
</tr><tr>
<td>
<code>path</code>
</td>
<td>
Returns:
(str): the path to the run `[entity]/[project]/[run_id]`
</td>
</tr><tr>
<td>
<code>project</code>
</td>
<td>
Returns:
(str): name of W&B project associated with run.
</td>
</tr><tr>
<td>
<code>resumed</code>
</td>
<td>
Returns:
(bool): whether or not the run was resumed
</td>
</tr><tr>
<td>
<code>start_time</code>
</td>
<td>
Returns:
(int): the unix time stamp in seconds when the run started
</td>
</tr><tr>
<td>
<code>starting_step</code>
</td>
<td>
Returns:
(int): the first step of the run
</td>
</tr><tr>
<td>
<code>step</code>
</td>
<td>
Every time you call wandb.log() it will by default increment the step
counter.
</td>
</tr><tr>
<td>
<code>sweep_id</code>
</td>
<td>
Returns:
(str, optional): the sweep id associated with the run or None
</td>
</tr><tr>
<td>
<code>tags</code>
</td>
<td>
Returns:
(Tuple[str]): tags associated with the run
</td>
</tr><tr>
<td>
<code>url</code>
</td>
<td>
Returns:
(str): name of W&B url associated with run.
</td>
</tr>
</table>



## Methods

<h3 id="alert"><code>alert</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.30/wandb/sdk/wandb_run.py#L2311-L2347">View source</a>

<pre><code>alert(
    title: str,
    text: str,
    level: Union[str, None] = None,
    wait_duration: Union[int, float, timedelta, None] = None
) -> None</code></pre>

Launch an alert with the given title and text.


<!-- Tabular view -->
<table>
<tr><th>Arguments</th></tr>

<tr>
<td>
<code>title</code>
</td>
<td>
(str) The title of the alert, must be less than 64 characters long.
</td>
</tr><tr>
<td>
<code>text</code>
</td>
<td>
(str) The text body of the alert.
</td>
</tr><tr>
<td>
<code>level</code>
</td>
<td>
(str or wandb.AlertLevel, optional) The alert level to use, either: <code>INFO</code>, <code>WARN</code>, or <code>ERROR</code>.
</td>
</tr><tr>
<td>
<code>wait_duration</code>
</td>
<td>
(int, float, or timedelta, optional) The time to wait (in seconds) before sending another
alert with this title.
</td>
</tr>
</table>



<h3 id="define_metric"><code>define_metric</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.30/wandb/sdk/wandb_run.py#L1925-L2017">View source</a>

<pre><code>define_metric(
    name: str,
    step_metric: Union[str, wandb_metric.Metric, None] = None,
    step_sync: bool = None,
    hidden: bool = None,
    summary: str = None,
    goal: str = None,
    overwrite: bool = None,
    **kwargs
) -> wandb_metric.Metric</code></pre>

Define metric properties which will later be logged with <code>wandb.log()</code>.


<!-- Tabular view -->
<table>
<tr><th>Arguments</th></tr>

<tr>
<td>
<code>name</code>
</td>
<td>
Name of the metric.
</td>
</tr><tr>
<td>
<code>step_metric</code>
</td>
<td>
Independent variable associated with the metric.
</td>
</tr><tr>
<td>
<code>step_sync</code>
</td>
<td>
Automatically add <code>step_metric</code> to history if needed.
Defaults to True if step_metric is specified.
</td>
</tr><tr>
<td>
<code>hidden</code>
</td>
<td>
Hide this metric from automatic plots.
</td>
</tr><tr>
<td>
<code>summary</code>
</td>
<td>
Specify aggregate metrics added to summary.
Supported aggregations: "min,max,mean,best,last,none"
Default aggregation is <code>copy</code>
Aggregation <code>best</code> defaults to <code>goal</code>==<code>minimize</code>
</td>
</tr><tr>
<td>
<code>goal</code>
</td>
<td>
Specify direction for optimizing the metric.
Supported direections: "minimize,maximize"
</td>
</tr>
</table>



<!-- Tabular view -->
<table>
<tr><th>Returns</th></tr>
<tr>
<td>
A metric object is returned that can be further specified.
</td>
</tr>

</table>



<h3 id="finish"><code>finish</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.30/wandb/sdk/wandb_run.py#L1200-L1215">View source</a>

<pre><code>finish(
    exit_code: int = None
) -> None</code></pre>

Marks a run as finished, and finishes uploading all data.  This is
used when creating multiple runs in the same process.  We automatically
call this method when your script exits.

<h3 id="finish_artifact"><code>finish_artifact</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.30/wandb/sdk/wandb_run.py#L2160-L2209">View source</a>

<pre><code>finish_artifact(
    artifact_or_path: Union[wandb_artifacts.Artifact, str],
    name: Optional[str] = None,
    type: Optional[str] = None,
    aliases: Optional[List[str]] = None,
    distributed_id: Optional[str] = None
) -> wandb_artifacts.Artifact</code></pre>

Finish a non-finalized artifact as output of a run. Subsequent "upserts" with
the same distributed ID will result in a new version

<!-- Tabular view -->
<table>
<tr><th>Arguments</th></tr>

<tr>
<td>
<code>artifact_or_path</code>
</td>
<td>
(str or Artifact) A path to the contents of this artifact,
can be in the following forms:
- `/local/directory`
- `/local/directory/file.txt`
- `s3://bucket/path`
You can also pass an Artifact object created by calling
<code>wandb.Artifact</code>.
</td>
</tr><tr>
<td>
<code>name</code>
</td>
<td>
(str, optional) An artifact name. May be prefixed with entity/project.
Valid names can be in the following forms:
- name:version
- name:alias
- digest
This will default to the basename of the path prepended with the current
run id  if not specified.
</td>
</tr><tr>
<td>
<code>type</code>
</td>
<td>
(str) The type of artifact to log, examples include <code>dataset</code>, <code>model</code>
</td>
</tr><tr>
<td>
<code>aliases</code>
</td>
<td>
(list, optional) Aliases to apply to this artifact,
defaults to `["latest"]`
</td>
</tr><tr>
<td>
<code>distributed_id</code>
</td>
<td>
(string, optional) Unique string that all distributed jobs share. If None,
defaults to the run's group name.
</td>
</tr>
</table>



<!-- Tabular view -->
<table>
<tr><th>Returns</th></tr>
<tr>
<td>
An <code>Artifact</code> object.
</td>
</tr>

</table>



<h3 id="get_project_url"><code>get_project_url</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.30/wandb/sdk/wandb_run.py#L732-L741">View source</a>

<pre><code>get_project_url() -> Optional[str]</code></pre>

Returns:
    A url (str, optional) for the W&B project associated with
    the run or None if the run is offline

<h3 id="get_sweep_url"><code>get_sweep_url</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.30/wandb/sdk/wandb_run.py#L743-L752">View source</a>

<pre><code>get_sweep_url() -> Optional[str]</code></pre>

Returns:
    A url (str, optional) for the sweep associated with the run
    or None if there is no associated sweep or the run is offline.

<h3 id="get_url"><code>get_url</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.30/wandb/sdk/wandb_run.py#L721-L730">View source</a>

<pre><code>get_url() -> Optional[str]</code></pre>

Returns:
    A url (str, optional) for the W&B run or None if the run
    is offline

<h3 id="join"><code>join</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.30/wandb/sdk/wandb_run.py#L1217-L1219">View source</a>

<pre><code>join(
    exit_code: int = None
) -> None</code></pre>

Deprecated alias for <code>finish()</code> - please use finish


<h3 id="log"><code>log</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.30/wandb/sdk/wandb_run.py#L934-L1098">View source</a>

<pre><code>log(
    data: Dict[str, Any],
    step: int = None,
    commit: bool = None,
    sync: bool = None
) -> None</code></pre>

Log a dict to the global run's history.

Use <code>wandb.log</code> to log data from runs, such as scalars, images, video,
histograms, and matplotlib plots.

The most basic usage is `wandb.log({'train-loss': 0.5, 'accuracy': 0.9})`.
This will save a history row associated with the run with `train-loss=0.5`
and <code>accuracy=0.9</code>. Visualize logged data in the workspace at wandb.ai,
or locally on a self-hosted instance of the W&B app:
https://docs.wandb.ai/self-hosted

Export data to explore in a Jupyter notebook, for example, with the API:
https://docs.wandb.ai/ref/public-api

Each time you call wandb.log(), this adds a new row to history and updates
the summary values for each key logged. In the UI, summary values show
up in the run table to compare single values across runs. You might want
to update summary manually to set the *best* value instead of the *last*
value for a given metric. After you finish logging, you can set summary:
`wandb.run.summary["accuracy"] = 0.9`.

Logged values don't have to be scalars. Logging any wandb object is supported.
For example `wandb.log({"example": wandb.Image("myimage.jpg")})` will log an
example image which will be displayed nicely in the wandb UI. See
https://docs.wandb.com/library/reference/data_types for all of the different
supported types.

Logging nested metrics is encouraged and is supported in the wandb API, so
you could log multiple accuracy values with `wandb.log({'dataset-1':
{'acc': 0.9, 'loss': 0.3} ,'dataset-2': {'acc': 0.8, 'loss': 0.2}})`
and the metrics will be organized in the wandb UI.

W&B keeps track of a global step so logging related metrics together is
encouraged, so by default each time wandb.log is called a global step
is incremented. If it's inconvenient to log related metrics together
calling `wandb.log({'train-loss': 0.5, commit=False})` and then
`wandb.log({'accuracy': 0.9})` is equivalent to calling
`wandb.log({'train-loss': 0.5, 'accuracy': 0.9})`

wandb.log is not intended to be called more than a few times per second.
If you want to log more frequently than that it's better to aggregate
the data on the client side or you may get degraded performance.

<!-- Tabular view -->
<table>
<tr><th>Arguments</th></tr>

<tr>
<td>
<code>row</code>
</td>
<td>
(dict, optional) A dict of serializable python objects i.e <code>str</code>,
<code>ints</code>, <code>floats</code>, <code>Tensors</code>, <code>dicts</code>, or <code>wandb.data_types</code>.
</td>
</tr><tr>
<td>
<code>commit</code>
</td>
<td>
(boolean, optional) Save the metrics dict to the wandb server
and increment the step.  If false <code>wandb.log</code> just updates the current
metrics dict with the row argument and metrics won't be saved until
<code>wandb.log</code> is called with <code>commit=True</code>.
</td>
</tr><tr>
<td>
<code>step</code>
</td>
<td>
(integer, optional) The global step in processing. This persists
any non-committed earlier steps but defaults to not committing the
specified step.
</td>
</tr><tr>
<td>
<code>sync</code>
</td>
<td>
(boolean, True) This argument is deprecated and currently doesn't
change the behaviour of <code>wandb.log</code>.
</td>
</tr>
</table>



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

For more examples, see https://docs.wandb.com/library/log



<!-- Tabular view -->
<table>
<tr><th>Raises</th></tr>

<tr>
<td>
<code>wandb.Error</code>
</td>
<td>
if called before <code>wandb.init</code>
</td>
</tr><tr>
<td>
<code>ValueError</code>
</td>
<td>
if invalid data is passed
</td>
</tr>
</table>



<h3 id="log_artifact"><code>log_artifact</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.30/wandb/sdk/wandb_run.py#L2076-L2107">View source</a>

<pre><code>log_artifact(
    artifact_or_path: Union[wandb_artifacts.Artifact, str],
    name: Optional[str] = None,
    type: Optional[str] = None,
    aliases: Optional[List[str]] = None
) -> wandb_artifacts.Artifact</code></pre>

Declare an artifact as output of a run.


<!-- Tabular view -->
<table>
<tr><th>Arguments</th></tr>

<tr>
<td>
<code>artifact_or_path</code>
</td>
<td>
(str or Artifact) A path to the contents of this artifact,
can be in the following forms:
- `/local/directory`
- `/local/directory/file.txt`
- `s3://bucket/path`
You can also pass an Artifact object created by calling
<code>wandb.Artifact</code>.
</td>
</tr><tr>
<td>
<code>name</code>
</td>
<td>
(str, optional) An artifact name. May be prefixed with entity/project.
Valid names can be in the following forms:
- name:version
- name:alias
- digest
This will default to the basename of the path prepended with the current
run id  if not specified.
</td>
</tr><tr>
<td>
<code>type</code>
</td>
<td>
(str) The type of artifact to log, examples include <code>dataset</code>, <code>model</code>
</td>
</tr><tr>
<td>
<code>aliases</code>
</td>
<td>
(list, optional) Aliases to apply to this artifact,
defaults to `["latest"]`
</td>
</tr>
</table>



<!-- Tabular view -->
<table>
<tr><th>Returns</th></tr>
<tr>
<td>
An <code>Artifact</code> object.
</td>
</tr>

</table>



<h3 id="log_code"><code>log_code</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.30/wandb/sdk/wandb_run.py#L662-L719">View source</a>

<pre><code>log_code(
    root: str = &#x27;.&#x27;,
    name: str = None,
    include_fn: Callable[[str], bool] = (lambda path: path.endswith(&#x27;.py&#x27;)),
    exclude_fn: Callable[[str], bool] = (lambda path: os.sep + &#x27;wandb&#x27; + os.sep in path)
) -> Optional[Artifact]</code></pre>

log_code() saves the current state of your code to a W&B artifact.  By
default it walks the current directory and logs all files that end with ".py".

<!-- Tabular view -->
<table>
<tr><th>Arguments</th></tr>
<tr>
<td>
root (str, optional): The relative (to os.getcwd()) or absolute path to
recursively find code from.
name (str, optional): The name of our code artifact.  By default we'll name
the artifact "source-$RUN_ID".  There may be scenarios where you want
many runs to share the same artifact.  Specifying name allows you to achieve that.
include_fn (callable, optional): A callable that accepts a file path and
returns True when it should be included and False otherwise.  This
defaults to: `lambda path: path.endswith(".py")`
exclude_fn (callable, optional): A callable that accepts a file path and
returns True when it should be excluded and False otherwise.  This
defaults to: `lambda path: False`
</td>
</tr>

</table>



#### Examples:

Basic usage
```python
run.log_code()
```

Advanced usage
```python
run.log_code("../", include_fn=lambda path: path.endswith(".py") or path.endswith(".ipynb"))
```



<!-- Tabular view -->
<table>
<tr><th>Returns</th></tr>
<tr>
<td>
An <code>Artifact</code> object if code was logged
</td>
</tr>

</table>



<h3 id="plot_table"><code>plot_table</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.30/wandb/sdk/wandb_run.py#L1222-L1237">View source</a>

<pre><code>plot_table(
    vega_spec_name, data_table, fields, string_fields=None
)</code></pre>

Creates a custom plot on a table.


<!-- Tabular view -->
<table>
<tr><th>Arguments</th></tr>

<tr>
<td>
<code>vega_spec_name</code>
</td>
<td>
the name of the spec for the plot
</td>
</tr><tr>
<td>
<code>table_key</code>
</td>
<td>
the key used to log the data table
</td>
</tr><tr>
<td>
<code>data_table</code>
</td>
<td>
a wandb.Table object containing the data to
be used on the visualization
</td>
</tr><tr>
<td>
<code>fields</code>
</td>
<td>
a dict mapping from table keys to fields that the custom
visualization needs
</td>
</tr><tr>
<td>
<code>string_fields</code>
</td>
<td>
a dict that provides values for any string constants
the custom visualization needs
</td>
</tr>
</table>



<h3 id="project_name"><code>project_name</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.30/wandb/sdk/wandb_run.py#L616-L618">View source</a>

<pre><code>project_name() -> str</code></pre>




<h3 id="restore"><code>restore</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.30/wandb/sdk/wandb_run.py#L1191-L1198">View source</a>

<pre><code>restore(
    name: str,
    run_path: Optional[str] = None,
    replace: bool = (False),
    root: Optional[str] = None
) -> Union[None, TextIO]</code></pre>

Downloads the specified file from cloud storage into the current directory
or run directory.  By default this will only download the file if it doesn't
already exist.

<!-- Tabular view -->
<table>
<tr><th>Arguments</th></tr>

<tr>
<td>
<code>name</code>
</td>
<td>
the name of the file
</td>
</tr><tr>
<td>
<code>run_path</code>
</td>
<td>
optional path to a run to pull files from, i.e. `username/project_name/run_id`
if wandb.init has not been called, this is required.
</td>
</tr><tr>
<td>
<code>replace</code>
</td>
<td>
whether to download the file even if it already exists locally
</td>
</tr><tr>
<td>
<code>root</code>
</td>
<td>
the directory to download the file to.  Defaults to the current
directory or the run directory if wandb.init was called.
</td>
</tr>
</table>



<!-- Tabular view -->
<table>
<tr><th>Returns</th></tr>
<tr>
<td>
None if it can't find the file, otherwise a file object open for reading
</td>
</tr>

</table>



<!-- Tabular view -->
<table>
<tr><th>Raises</th></tr>

<tr>
<td>
<code>wandb.CommError</code>
</td>
<td>
if we can't connect to the wandb backend
</td>
</tr><tr>
<td>
<code>ValueError</code>
</td>
<td>
if the file is not found or can't find run_path
</td>
</tr>
</table>



<h3 id="save"><code>save</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.30/wandb/sdk/wandb_run.py#L1100-L1189">View source</a>

<pre><code>save(
    glob_str: Optional[str] = None,
    base_path: Optional[str] = None,
    policy: str = &#x27;live&#x27;
) -> Union[bool, List[str]]</code></pre>

Ensure all files matching *glob_str* are synced to wandb with the policy specified.


<!-- Tabular view -->
<table>
<tr><th>Arguments</th></tr>

<tr>
<td>
<code>glob_str</code>
</td>
<td>
(string) a relative or absolute path to a unix glob or regular
path.  If this isn't specified the method is a noop.
</td>
</tr><tr>
<td>
<code>base_path</code>
</td>
<td>
(string) the base path to run the glob relative to
</td>
</tr><tr>
<td>
<code>policy</code>
</td>
<td>
(string) on of <code>live</code>, <code>now</code>, or <code>end</code>
- live: upload the file as it changes, overwriting the previous version
- now: upload the file once now
- end: only upload file when the run ends
</td>
</tr>
</table>



<h3 id="upsert_artifact"><code>upsert_artifact</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.30/wandb/sdk/wandb_run.py#L2109-L2158">View source</a>

<pre><code>upsert_artifact(
    artifact_or_path: Union[wandb_artifacts.Artifact, str],
    name: Optional[str] = None,
    type: Optional[str] = None,
    aliases: Optional[List[str]] = None,
    distributed_id: Optional[str] = None
) -> wandb_artifacts.Artifact</code></pre>

Declare (or append tp) a non-finalized artifact as output of a run. Note that you must call
run.finish_artifact() to finalize the artifact. This is useful when distributed jobs
need to all contribute to the same artifact.

<!-- Tabular view -->
<table>
<tr><th>Arguments</th></tr>

<tr>
<td>
<code>artifact_or_path</code>
</td>
<td>
(str or Artifact) A path to the contents of this artifact,
can be in the following forms:
- `/local/directory`
- `/local/directory/file.txt`
- `s3://bucket/path`
You can also pass an Artifact object created by calling
<code>wandb.Artifact</code>.
</td>
</tr><tr>
<td>
<code>name</code>
</td>
<td>
(str, optional) An artifact name. May be prefixed with entity/project.
Valid names can be in the following forms:
- name:version
- name:alias
- digest
This will default to the basename of the path prepended with the current
run id  if not specified.
</td>
</tr><tr>
<td>
<code>type</code>
</td>
<td>
(str) The type of artifact to log, examples include <code>dataset</code>, <code>model</code>
</td>
</tr><tr>
<td>
<code>aliases</code>
</td>
<td>
(list, optional) Aliases to apply to this artifact,
defaults to `["latest"]`
</td>
</tr><tr>
<td>
<code>distributed_id</code>
</td>
<td>
(string, optional) Unique string that all distributed jobs share. If None,
defaults to the run's group name.
</td>
</tr>
</table>



<!-- Tabular view -->
<table>
<tr><th>Returns</th></tr>
<tr>
<td>
An <code>Artifact</code> object.
</td>
</tr>

</table>



<h3 id="use_artifact"><code>use_artifact</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.30/wandb/sdk/wandb_run.py#L2024-L2074">View source</a>

<pre><code>use_artifact(
    artifact_or_name, type=None, aliases=None
)</code></pre>

Declare an artifact as an input to a run, call <code>download</code> or <code>file</code> on
the returned object to get the contents locally.

<!-- Tabular view -->
<table>
<tr><th>Arguments</th></tr>

<tr>
<td>
<code>artifact_or_name</code>
</td>
<td>
(str or Artifact) An artifact name.
May be prefixed with entity/project. Valid names
can be in the following forms:
- name:version
- name:alias
- digest
You can also pass an Artifact object created by calling <code>wandb.Artifact</code>
</td>
</tr><tr>
<td>
<code>type</code>
</td>
<td>
(str, optional) The type of artifact to use.
</td>
</tr><tr>
<td>
<code>aliases</code>
</td>
<td>
(list, optional) Aliases to apply to this artifact
</td>
</tr>
</table>



<!-- Tabular view -->
<table>
<tr><th>Returns</th></tr>
<tr>
<td>
An <code>Artifact</code> object.
</td>
</tr>

</table>



<h3 id="watch"><code>watch</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.30/wandb/sdk/wandb_run.py#L2020-L2021">View source</a>

<pre><code>watch(
    models, criterion=None, log=&#x27;gradients&#x27;, log_freq=100, idx=None
) -> None</code></pre>




<h3 id="__enter__"><code>__enter__</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.30/wandb/sdk/wandb_run.py#L2349-L2350">View source</a>

<pre><code>__enter__() -> "Run"</code></pre>




<h3 id="__exit__"><code>__exit__</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.30/wandb/sdk/wandb_run.py#L2352-L2360">View source</a>

<pre><code>__exit__(
    exc_type: Type[BaseException],
    exc_val: BaseException,
    exc_tb: TracebackType
) -> bool</code></pre>






