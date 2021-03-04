---
description: Call wandb.init() at the top of your script to start a new run
---

# wandb.init\(\)

Call `wandb.init()` once at the beginning of your script to initialize a new job. This creates a new run in W&B and launches a background process to sync data. 

* **On Prem**: If you need a private cloud or local instance of W&B, see our [Self Hosted](../self-hosted/) offerings. 
* **Automated Environments**: Most of these settings can also be controlled via [Environment Variables](environment-variables.md). This is often useful when you're running jobs on a cluster.

### Reference Docs

Check the reference docs for arguments.

{% page-ref page="../ref/run/init.md" %}

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

### InitStartError: Error communicating with wandb process <a id="init-start-error"></a>

The **InitStartError: Error communicating with wandb process** error indicates that the library is having difficulty launching the process which synchronizes data to the server. 

The following workarounds can help resolve the issue in certain environments: 

```text
# Try this if using linux or macos
wandb.init(wandb.Settings(start_method="fork"))
# Try this if using google colab
wandb.init(wandb.Settings(start_method="thread"))
```

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

