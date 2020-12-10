---
description: Call wandb.init() at the top of your script to start a new run
---

# wandb.init\(\)

Call `wandb.init()` once at the beginning of your script to initialize a new job. This creates a new run in W&B and launches a background process to sync data. If you need a private cloud or local installation of W&B we support that in our [Self Hosted](../self-hosted/) offerings. `wandb.init()` returns a [**run**](../ref/export-api/api.md#run) object, and you can also access the run object with `wandb.run`.

### Optional Arguments

<table>
  <thead>
    <tr>
      <th style="text-align:left">Argument</th>
      <th style="text-align:left">Type</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">project</td>
      <td style="text-align:left">str</td>
      <td style="text-align:left">The name of the project where you&apos;re sending the new run. If the
        project is not specified, the run is put in an &quot;uncategorized&quot;
        project in your default entity. Change your default entity on your <a href="https://wandb.ai/settings">settings page</a> under
        &quot;default location to create new projects&quot;, or set the <code>entity</code> argument.</td>
    </tr>
    <tr>
      <td style="text-align:left">entity</td>
      <td style="text-align:left">str</td>
      <td style="text-align:left">An entity is a username or team name where you&apos;re sending runs. This
        entity <em>must exist</em> before you can send runs there, so make sure to
        create your account or team in the UI before starting to log runs.</td>
    </tr>
    <tr>
      <td style="text-align:left">save_code</td>
      <td style="text-align:left">bool</td>
      <td style="text-align:left">Turn this on to save the main script or notebook to W&amp;B. This is valuable
        for improving experiment reproducibility and to diff code across experiments
        in the UI. By default this is off, but you can change the default to on
        in <a href="https://wandb.ai/settings">Settings</a>.</td>
    </tr>
    <tr>
      <td style="text-align:left">group</td>
      <td style="text-align:left">str</td>
      <td style="text-align:left">Specify a group to organize individual runs into a larger experiment.
        For example, you might be doing k-fold cross validation, or you might have
        multiple jobs that train and evaluate a model against different test sets.
        Group gives you a way to organize runs together into a larger whole, and
        you can toggle this on and off in the UI. For more details, see <a href="grouping.md">Grouping</a>.</td>
    </tr>
    <tr>
      <td style="text-align:left">job_type</td>
      <td style="text-align:left">str</td>
      <td style="text-align:left">Specify the type of run, which is useful when you&apos;re grouping runs
        together into larger experiments using <code>group</code>. For example,
        you might have multiple jobs in a group, with job types like train and
        eval. Setting this makes it easy to filter and group similar runs together
        in the UI so you can compare apples to apples.</td>
    </tr>
    <tr>
      <td style="text-align:left">name</td>
      <td style="text-align:left">str</td>
      <td style="text-align:left">A short display name for this run, which is how you&apos;ll identify this
        run in the UI. By default we generate a random two-word name that lets
        you easily cross-reference runs from the table to charts. Keeping these
        run names short makes the chart legends and tables easier to read. If you&apos;re
        looking for a place to save your hyperparameters, we recommend saving those
        to <code>config</code> (below).</td>
    </tr>
    <tr>
      <td style="text-align:left">notes</td>
      <td style="text-align:left">str</td>
      <td style="text-align:left">A longer description of the run, like a -m commit message in git. This
        helps you remember what you were doing when you ran this run.</td>
    </tr>
    <tr>
      <td style="text-align:left">config</td>
      <td style="text-align:left">dict</td>
      <td style="text-align:left">A dictionary-like object for saving inputs to your job, like hyperparameters
        for a model or settings for a data preprocessing job. The config will show
        up in a table in the UI that you can use to group, filter, and sort runs.
        Keys should not have <code>.</code> in the names, and values should be under
        10 MB.</td>
    </tr>
    <tr>
      <td style="text-align:left">tags</td>
      <td style="text-align:left">str[]</td>
      <td style="text-align:left">A list of strings, which will populate the list of tags on this run in
        the UI. Tags are useful for organizing runs together, or applying temporary
        labels like &quot;baseline&quot; or &quot;production&quot;. It&apos;s easy
        to add and remove tags in the UI, or filter down to just runs with a specific
        tag.</td>
    </tr>
    <tr>
      <td style="text-align:left">dir</td>
      <td style="text-align:left">path</td>
      <td style="text-align:left">When you call <code>download()</code> on an artifact, this is the directory
        where downloaded files will be saved. By default this is the ./wandb directory.</td>
    </tr>
    <tr>
      <td style="text-align:left">sync_tensorboard</td>
      <td style="text-align:left">bool</td>
      <td style="text-align:left">Whether to copy all TensorBoard logs to W&amp;B; see <a href="../integrations/tensorboard.md">Tensorboard</a> (default:
        False)</td>
    </tr>
    <tr>
      <td style="text-align:left">resume</td>
      <td style="text-align:left">bool or str</td>
      <td style="text-align:left">If True, the run auto-resumes. Set to a unique run ID to manually resume
        a run. See <a href="resuming.md">Resuming</a>. (default: False)</td>
    </tr>
    <tr>
      <td style="text-align:left">reinit</td>
      <td style="text-align:left">bool</td>
      <td style="text-align:left">Whether to allow multiple <code>wandb.init()</code> calls in the same process.
        (default: False)</td>
    </tr>
    <tr>
      <td style="text-align:left">anonymous</td>
      <td style="text-align:left">&quot;allow&quot; &quot;never&quot; &quot;must&quot;</td>
      <td style="text-align:left">
        <p>Enable or disable anonymous logging &#x2014; tracking this run in the
          W&amp;B cloud without creating an account.</p>
        <ul>
          <li><b>&quot;allow&quot;</b> lets a logged-in user track runs with their account,
            but lets someone who is running the script without a W&amp;B account see
            the charts in the UI.</li>
          <li><b>&quot;never&quot; </b> (default) requires you to link your W&amp;B account
            before tracking the run so you don&apos;t accidentally create an anonymous
            run.</li>
          <li><b>&quot;must&quot;</b> sends the run to an anonymous account instead of
            to a user account.</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">force</td>
      <td style="text-align:left">bool</td>
      <td style="text-align:left">whether to force a user to be logged into wandb when running a script
        (default: False)</td>
    </tr>
    <tr>
      <td style="text-align:left">magic</td>
      <td style="text-align:left">bool</td>
      <td style="text-align:left">Whether to attempt to auto-instrument your script, capturing basic details
        of your run without you having to add more wandb code. (default: False)</td>
    </tr>
    <tr>
      <td style="text-align:left">id</td>
      <td style="text-align:left">unique str</td>
      <td style="text-align:left">A <em>unique</em> ID for this run, used for <a href="resuming.md">Resuming</a>.
        It <em>must</em> be unique in the project, and if you delete a run you can&apos;t
        reuse the ID. Use the <code>name</code> field for a short descriptive name,
        or <code>config</code> for saving hyperparameters to compare across runs.
        The ID cannot contain special characters.</td>
    </tr>
    <tr>
      <td style="text-align:left">monitor_gym</td>
      <td style="text-align:left">bool</td>
      <td style="text-align:left">Whether to log videos from the OpenAI Gym; see <a href="../integrations/ray-tune.md">Ray Tune</a> (default:
        False)</td>
    </tr>
    <tr>
      <td style="text-align:left">allow_val_change</td>
      <td style="text-align:left">bool</td>
      <td style="text-align:left">Whether to allow <a href="config.md">config</a> values to change after setting
        the keys once. By default we throw an exception if a config value is overwritten.
        If you want to track something like a varying learning_rate at multiple
        times during training, use wandb.log() instead. (default: False)</td>
    </tr>
  </tbody>
</table>

Most of these settings can also be controlled via [Environment Variables](environment-variables.md). This is often useful when you're running jobs on a cluster.

We automatically save a copy of the script where you run wandb.init\(\). Learn more about the code comparison feature here: [Code Comparer](../app/features/panels/code.md). To disable this feature, set the environment variable WANDB\_DISABLE\_CODE=true.

## Common Questions

### How do I launch multiple runs from one script?

If you're trying to start multiple runs from one script, add two things to your code:

1. run = wandb.init\(**reinit=True**\): Use this setting to allow reinitializing runs
2. **run.finish\(\)**: Use this at the end of your run to finish logging for that run

```python
import wandb
for x in range(10):
    run = wandb.init(project="runs-from-for-loop", reinit=True)
    for y in range (100):
        wandb.log({"metric": x+y})
    run.finish()
```

Alternatively you can use a python context manager which will automatically finish logging:

```python
import wandb
for x in range(10):
    run = wandb.init(reinit=True)
    with run:
        for y in range(100):
            run.log({"metric": x+y})
```

### LaunchError: Permission denied

If you're getting a **LaunchError: Launch exception: Permission denied** error, you don't have permissions to log to the project you're trying to send runs to. This might be for a few different reasons.

1. You aren't logged in on this machine. Run `wandb login` on the command line.
2. You've set an entity that doesn't exist. "Entity" should be your username or the name of an existing team. If you need to create a team, go to our [Subscriptions page](https://app.wandb.ai/billing).
3. You don't have project permissions. Ask the creator of the project to set the privacy to **Open** so you can log runs to this project.

### Get the readable run name

Get the nice, readable name for your run.

```python
import wandb

wandb.init()
run_name = wandb.run.name
```

### Set the run name to the generated run ID

If you'd like to overwrite the run name \(like snowy-owl-10\) with the run ID \(like qvlp96vk\) you can use this snippet:

```python
import wandb
wandb.init()
wandb.run.name = wandb.run.id
wandb.run.save()
```

### Save the git commit

When wandb.init\(\) is called in your script, we automatically look for git information to save a link to your repo the SHA of the latest commit. The git information should show up on your [run page](../app/pages/run-page.md#overview-tab). If you aren't seeing it appear there, make sure that your script where you call wandb.init\(\) is located in a folder that has git information.

The git commit and command used to run the experiment are visible to you but are hidden to external users, so if you have a public project, these details will remain private.

### Save logs offline

By default, wandb.init\(\) starts a process that syncs metrics in real time to our cloud hosted app. If your machine is offline or you don't have internet access, here's how to run wandb using the offline mode and sync later.

Set two environment variables:

1. **WANDB\_API\_KEY**: Set this to your account's API key, on your [settings page](https://app.wandb.ai/settings)
2. **WANDB\_MODE**: dryrun

Here's a sample of what this would look like in your script:

```python
import wandb
import os

os.environ["WANDB_API_KEY"] = YOUR_KEY_HERE
os.environ["WANDB_MODE"] = "dryrun"

config = {
  "dataset": "CIFAR10",
  "machine": "offline cluster",
  "model": "CNN",
  "learning_rate": 0.01,
  "batch_size": 128,
}

wandb.init(project="offline-demo")

for i in range(100):
  wandb.log({"accuracy": i})
```

Here's a sample terminal output:

![](../.gitbook/assets/image%20%2881%29.png)

And once I have internet, I run a sync command to send that folder to the cloud.

`wandb sync wandb/dryrun-folder-name`

![](../.gitbook/assets/image%20%2836%29.png)

