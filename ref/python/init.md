# init



[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/v0.10.30/wandb/sdk/wandb_init.py#L557-L782)




Start a new tracked run with <code>wandb.init()</code>.

<pre><code>init(
    job_type: Optional[str] = None,
    dir=None,
    config: Union[Dict, str, None] = None,
    project: Optional[str] = None,
    entity: Optional[str] = None,
    reinit: bool = None,
    tags: Optional[Sequence] = None,
    group: Optional[str] = None,
    name: Optional[str] = None,
    notes: Optional[str] = None,
    magic: Union[dict, str, bool] = None,
    config_exclude_keys=None,
    config_include_keys=None,
    anonymous: Optional[str] = None,
    mode: Optional[str] = None,
    allow_val_change: Optional[bool] = None,
    resume: Optional[Union[bool, str]] = None,
    force: Optional[bool] = None,
    tensorboard=None,
    sync_tensorboard=None,
    monitor_gym=None,
    save_code=None,
    id=None,
    settings: Union[Settings, Dict[str, Any], None] = None
) -> Union[Run, RunDisabled, None]</code></pre>




In an ML training pipeline, you could add <code>wandb.init()</code>
to the beginning of your training script as well as your evaluation
script, and each piece would be tracked as a run in W&B.

<code>wandb.init()</code> spawns a new background process to log data to a run, and it
also syncs data to wandb.ai by default so you can see live visualizations.
Call <code>wandb.init()</code> to start a run before logging data with <code>wandb.log()</code>.

<code>wandb.init()</code> returns a run object, and you can also access the run object
with wandb.run.

<!-- Tabular view -->
<table>
<tr><th>Arguments</th></tr>

<tr>
<td>
<code>project</code>
</td>
<td>
(str, optional) The name of the project where you're sending
the new run. If the project is not specified, the run is put in an
"Uncategorized" project.
</td>
</tr><tr>
<td>
<code>entity</code>
</td>
<td>
(str, optional) An entity is a username or team name where
you're sending runs. This entity must exist before you can send runs
there, so make sure to create your account or team in the UI before
starting to log runs.
If you don't specify an entity, the run will be sent to your default
entity, which is usually your username. Change your default entity
in [Settings](wandb.ai/settings) under "default location to create
new projects".
</td>
</tr><tr>
<td>
<code>config</code>
</td>
<td>
(dict, argparse, absl.flags, str, optional)
This sets wandb.config, a dictionary-like object for saving inputs
to your job, like hyperparameters for a model or settings for a data
preprocessing job. The config will show up in a table in the UI that
you can use to group, filter, and sort runs. Keys should not contain
<code>.</code> in their names, and values should be under 10 MB.
If dict, argparse or absl.flags: will load the key value pairs into
the wandb.config object.
If str: will look for a yaml file by that name, and load config from
that file into the wandb.config object.
</td>
</tr><tr>
<td>
<code>save_code</code>
</td>
<td>
(bool, optional) Turn this on to save the main script or
notebook to W&B. This is valuable for improving experiment
reproducibility and to diff code across experiments in the UI. By
default this is off, but you can flip the default behavior to on
in [Settings](wandb.ai/settings).
</td>
</tr><tr>
<td>
<code>group</code>
</td>
<td>
(str, optional) Specify a group to organize individual runs into
a larger experiment. For example, you might be doing cross
validation, or you might have multiple jobs that train and evaluate
a model against different test sets. Group gives you a way to
organize runs together into a larger whole, and you can toggle this
on and off in the UI. For more details, see
[Grouping](docs.wandb.com/library/grouping).
</td>
</tr><tr>
<td>
<code>job_type</code>
</td>
<td>
(str, optional) Specify the type of run, which is useful when
you're grouping runs together into larger experiments using group.
For example, you might have multiple jobs in a group, with job types
like train and eval. Setting this makes it easy to filter and group
similar runs together in the UI so you can compare apples to apples.
</td>
</tr><tr>
<td>
<code>tags</code>
</td>
<td>
(list, optional) A list of strings, which will populate the list
of tags on this run in the UI. Tags are useful for organizing runs
together, or applying temporary labels like "baseline" or
"production". It's easy to add and remove tags in the UI, or filter
down to just runs with a specific tag.
</td>
</tr><tr>
<td>
<code>name</code>
</td>
<td>
(str, optional) A short display name for this run, which is how
you'll identify this run in the UI. By default we generate a random
two-word name that lets you easily cross-reference runs from the
table to charts. Keeping these run names short makes the chart
legends and tables easier to read. If you're looking for a place to
save your hyperparameters, we recommend saving those in config.
</td>
</tr><tr>
<td>
<code>notes</code>
</td>
<td>
(str, optional) A longer description of the run, like a -m commit
message in git. This helps you remember what you were doing when you
ran this run.
</td>
</tr><tr>
<td>
<code>dir</code>
</td>
<td>
(str, optional) An absolute path to a directory where metadata will
be stored. When you call <code>download()</code> on an artifact, this is the
directory where downloaded files will be saved. By default this is
the ./wandb directory.
</td>
</tr><tr>
<td>
<code>resume</code>
</td>
<td>
(bool, str, optional) Sets the resuming behavior. Options:
`"allow"<code>, </code>"must"<code>, </code>"never"<code>, </code>"auto"<code> or </code>None<code>. Defaults to </code>None`.
Cases:
- <code>None</code> (default): If the new run has the same ID as a previous run,
this run overwrites that data.
- `"auto"<code> (or </code>True`): if the preivous run on this machine crashed,
automatically resume it. Otherwise, start a new run.
- `"allow"`: if id is set with `init(id="UNIQUE_ID")` or
`WANDB_RUN_ID="UNIQUE_ID"` and it is identical to a previous run,
wandb will automatically resume the run with that id. Otherwise,
wandb will start a new run.
- `"never"`: if id is set with `init(id="UNIQUE_ID")` or
`WANDB_RUN_ID="UNIQUE_ID"` and it is identical to a previous run,
wandb will crash.
- `"must"`: if id is set with `init(id="UNIQUE_ID")` or
`WANDB_RUN_ID="UNIQUE_ID"` and it is identical to a previous run,
wandb will automatically resume the run with the id. Otherwise
wandb will crash.
See https://docs.wandb.com/library/advanced/resuming for more.
</td>
</tr><tr>
<td>
<code>reinit</code>
</td>
<td>
(bool, optional) Allow multiple <code>wandb.init()</code> calls in the same
process. (default: False)
</td>
</tr><tr>
<td>
<code>magic</code>
</td>
<td>
(bool, dict, or str, optional) The bool controls whether we try to
auto-instrument your script, capturing basic details of your run
without you having to add more wandb code. (default: <code>False</code>)
You can also pass a dict, json string, or yaml filename.
</td>
</tr><tr>
<td>
<code>config_exclude_keys</code>
</td>
<td>
(list, optional) string keys to exclude from
<code>wandb.config</code>.
</td>
</tr><tr>
<td>
<code>config_include_keys</code>
</td>
<td>
(list, optional) string keys to include in
<code>wandb.config</code>.
</td>
</tr><tr>
<td>
<code>anonymous</code>
</td>
<td>
(str, optional) Controls anonymous data logging. Options:
- `"never"` (default): requires you to link your W&B account before
tracking the run so you don't accidentally create an anonymous
run.
- `"allow"`: lets a logged-in user track runs with their account, but
lets someone who is running the script without a W&B account see
the charts in the UI.
- `"must"`: sends the run to an anonymous account instead of to a
signed-up user account.
</td>
</tr><tr>
<td>
<code>mode</code>
</td>
<td>
(str, optional) Can be `"online"<code>, </code>"offline"<code> or </code>"disabled"`. Defaults to
online.
</td>
</tr><tr>
<td>
<code>allow_val_change</code>
</td>
<td>
(bool, optional) Whether to allow config values to
change after setting the keys once. By default we throw an exception
if a config value is overwritten. If you want to track something
like a varying learning rate at multiple times during training, use
<code>wandb.log()</code> instead. (default: <code>False</code> in scripts, <code>True</code> in Jupyter)
</td>
</tr><tr>
<td>
<code>force</code>
</td>
<td>
(bool, optional) If <code>True</code>, this crashes the script if a user isn't
logged in to W&B. If <code>False</code>, this will let the script run in offline
mode if a user isn't logged in to W&B. (default: <code>False</code>)
</td>
</tr><tr>
<td>
<code>sync_tensorboard</code>
</td>
<td>
(bool, optional) Synchronize wandb logs from tensorboard or
tensorboardX and saves the relevant events file. (default: <code>False</code>)
</td>
</tr><tr>
<td>
<code>monitor_gym</code>
</td>
<td>
(bool, optional) Automatically logs videos of environment when
using OpenAI Gym. (default: <code>False</code>)
See https://docs.wandb.com/library/integrations/openai-gym
</td>
</tr><tr>
<td>
<code>id</code>
</td>
<td>
(str, optional) A unique ID for this run, used for resuming. It must
be unique in the project, and if you delete a run you can't reuse
the ID. Use the name field for a short descriptive name, or config
for saving hyperparameters to compare across runs. The ID cannot
contain special characters.
See https://docs.wandb.com/library/resuming
</td>
</tr>
</table>



#### Examples:

Basic usage
```
wandb.init()
```

Launch multiple runs from the same script
```
for x in range(10):
    with wandb.init(project="my-projo") as run:
        for y in range(100):
            run.log({"metric": x+y})
```



<!-- Tabular view -->
<table>
<tr><th>Raises</th></tr>

<tr>
<td>
<code>Exception</code>
</td>
<td>
if problem.
</td>
</tr>
</table>



<!-- Tabular view -->
<table>
<tr><th>Returns</th></tr>
<tr>
<td>
A <code>Run</code> object.
</td>
</tr>

</table>

