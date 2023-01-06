---
description: Call wandb.init() at the top of your script to start a new run
---

# Launch Experiments with wandb.init

Call `wandb.init()` once at the beginning of your script to initialize a new job. This creates a new run in W\&B and launches a background process to sync data.

* **On-Prem**: If you need a private cloud or local instance of W\&B, see our [Self Hosted](../self-hosted/) offerings.
* **Automated Environments**: Most of these settings can also be controlled via [Environment Variables](advanced/environment-variables.md). This is often useful when you're running jobs on a cluster.

### Reference Documentation

View the reference docs for this function, generated from the `wandb` Python library.

{% content-ref url="../../ref/python/init.md" %}
[init.md](../../ref/python/init.md)
{% endcontent-ref %}

## Common Questions

### How do I launch multiple runs from one script?

If you're trying to start multiple runs from one script, add two things to your code:

1. `run = wandb.init(reinit=True)`: Use this setting to allow reinitializing runs
2. `run.finish()`: Use this at the end of your run to finish logging for that run

```python
import wandb
for x in range(10):
    run = wandb.init(reinit=True)
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

### `InitStartError: Error communicating with wandb process` <a href="#init-start-error" id="init-start-error"></a>

This error indicates that the library is having difficulty launching the process which synchronizes data to the server.

The following workarounds can help resolve the issue in certain environments:

{% tabs %}
{% tab title="Linux / OS X" %}
```python
wandb.init(settings=wandb.Settings(start_method="fork"))
```
{% endtab %}

{% tab title="Google Colab" %}
For versions prior to `0.13.0` we suggest using:

```python
wandb.init(settings=wandb.Settings(start_method="thread"))
```
{% endtab %}
{% endtabs %}

### How can I use wandb with multiprocessing, e.g. distributed training? <a href="#multiprocess" id="multiprocess"></a>

If your training program uses multiple processes you will need to structure your program to avoid making wandb method calls from processes where you did not run `wandb.init()`.\
\
There are several approaches to managing multiprocess training:

1. Call `wandb.init` in all your processes, using the [group](advanced/grouping.md) keyword argument to define a shared group. Each process will have its own wandb run and the UI will group the training processes together.
2. Call `wandb.init` from just one process and pass data to be logged over [multiprocessing queues](https://docs.python.org/3/library/multiprocessing.html#exchanging-objects-between-processes).

{% hint style="info" %}
Check out the [Distributed Training Guide](advanced/distributed-training.md) for more detail on these two approaches, including code examples with Torch DDP.
{% endhint %}

### How do I programmatically access the human-readable run name?

It's available as the `.name` attribute of a [`wandb.Run`](../../ref/python/run.md).

```python
import wandb

wandb.init()
run_name = wandb.run.name
```

### Can I just set the run name to the run ID?

If you'd like to overwrite the run name (like snowy-owl-10) with the run ID (like qvlp96vk) you can use this snippet:

```python
import wandb
wandb.init()
wandb.run.name = wandb.run.id
```

### **I didn't name my run. Where is the run name coming from?**

If you do not explicitly name your run, a random run name will be assigned to the run to help identify the run in the UI. For instance, random run names will look like "pleasant-flower-4" or "misunderstood-glade-2".

### How can I save the git commit associated with my run?

When `wandb.init` is called in your script, we automatically look for git information to save, including a link to a remote repo and the SHA of the latest commit. The git information should show up on your [run page](../../ref/app/pages/run-page.md#overview-tab). If you aren't seeing it appear there, make sure that your shell's current working directory when executing your script is located in a folder managed by git.

The git commit and command used to run the experiment are visible to you but are hidden to external users, so if you have a public project, these details will remain private.

### Is it possible to save metrics offline and sync them to W\&B later?

By default, `wandb.init` starts a process that syncs metrics in real time to our cloud hosted app. If your machine is offline, you don't have internet access, or you just want to hold off on the upload, here's how to run `wandb` in offline mode and sync later.

You'll need to set two [environment variables](advanced/environment-variables.md).

1. `WANDB_API_KEY=$KEY`, where `$KEY` is the API Key from your [settings page](https://app.wandb.ai/settings)
2. `WANDB_MODE="offline"`

And here's a sample of what this would look like in your script:

```python
import wandb
import os

os.environ["WANDB_API_KEY"] = YOUR_KEY_HERE
os.environ["WANDB_MODE"] = "offline"

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

![](<../../.gitbook/assets/image (26) (3).png>)

And once you're ready, just run a sync command to send that folder to the cloud.

```python
wandb sync wandb/dryrun-folder-name
```

![](<../../.gitbook/assets/image (27).png>)

### What is the difference between wandb.init modes?

Modes can be "online", "offline" or "disabled", and default to online.

`online`(default): In this mode, the client sends data to the wandb server.

`offline`: In this mode, instead of sending data to the wandb server, the client will store data on your local machine which can be later synced with the [`wandb sync`](https://docs.wandb.ai/ref/cli/wandb-sync?q=sync) command.

`disabled`: In this mode, the client \*\*\*\* returns mocked objects and prevents all network communication. The client will essentially act like a no-op. In other words, all logging is entirely disabled. However, stubs out of all the API methods are still callable. This is usually used in tests.

### My run's state is "crashed" on the UI but is still running on my machine. What do I do to get my data back?

You most likely lost connection to your machine while training. You can recover your data by running [`wandb sync [PATH_TO_RUN]`](https://docs.wandb.ai/ref/cli/wandb-sync). The path to your run will be a folder in your `wandb` directory corresponding to the Run ID of the run in progress.

### `LaunchError: Permission denied`

If you're getting the error message `Launch Error: Permission denied`, you don't have permissions to log to the project you're trying to send runs to. This might be for a few different reasons.

1. You aren't logged in on this machine. Run [`wandb login`](https://docs.wandb.ai/ref/cli/wandb-login) on the command line.
2. You've set an entity that doesn't exist. "Entity" should be your username or the name of an existing team. If you need to create a team, go to our [Subscriptions page](https://app.wandb.ai/billing).
3. You don't have project permissions. Ask the creator of the project to set the privacy to **Open** so you can log runs to this project.

### Does W\&B uses the `multiprocessing` library?

Yes, W\&B uses the `multiprocessing` library. If you see an error message such as:

```
An attempt has been made to start a new process before the current process 
has finished its bootstrapping phase.
```

This might mean that you might need to add an entry point protection `if name == main`. Note that you would only need to add this entry point protection in case you're trying to run W\&B directly from the script.
