---
description: Overview of our client library
---

# Python Library

Use our python library to instrument your machine learning model and track experiments. Setup should only take a few lines of code. If you're using a popular framework, we have a number of integrations to make setting up wandb easy.

We have more detailed docs generated from the code in [Reference](reference/).

### **Instrumenting a model**

* wandb.init — initialize a new run at the top of your training script
* wandb.config — track hyperparameters
* wandb.log — log metrics over time within your training loop
* wandb.save — save files in association with your run, like model weights
* wandb.restore — restore the state of your code when you ran a given run

{% page-ref page="init.md" %}

{% page-ref page="config.md" %}

{% page-ref page="log.md" %}

{% page-ref page="save.md" %}

{% page-ref page="restore.md" %}

## What gets uploaded

All the data logged from your script is saved locally to your machine in a **wandb** directory, then sync'd to the cloud.

### **Logged Automatically**

* **System metrics**: CPU and GPU utilization, network, etc. These come from [nvidia-smi](https://developer.nvidia.com/nvidia-system-management-interface) and are shown in the System tab on the run page.
* **Command line**: The stdout and stderr are picked up and show in the logs tab on the run page.
* **Git commit**: We pick up the latest git commit and show it on the overview tab of the run page.
* **Files**: The requirements.txt file and any files you save to the **wandb** directory for the run will be uploaded and shown on the files tab of the run page.

### Logged with specific calls

Where data and model metrics are concerned, you get to decide exactly what you want to log.

* **Dataset**: You have to specifically log images or other dataset samples for them to stream to W&B.
* **PyTorch gradients**: Add wandb.watch\(model\) to see gradients of the weights as histograms in the UI.
* **Config**: Log hyperparameters, a link to your dataset, or the name of the architecture you're using as config parameters, passed in like this: wandb.init\(config=your\_config\_dictionary\).
* **Metrics**: Use wandb.log\(\) to see metrics from your model. If you log metrics like accuracy and loss from inside your training loop, you'll get live updating graphs in the UI.

## Common Questions

### Multiple wandb users on shared machines

If you're using a shared machine and another person is a wandb user, it's easy to make sure your runs are always logged to the proper account. Set the [WANDB\_API\_KEY environment variable](environment-variables.md) to authenticate. If you source it in your env, when you log in you'll have the right credentials, or you can set the environment variable from your script.

### Organization best practices <a id="best-practices"></a>

We provide a very flexible and customizable tool. You're free to use our tools however you'd like, but here are some guidelines for how to think about our tools.

Here's an example of setting up a run:

```python
import wandb

config = dict (
  learning_rate = 0.01,
  momentum = 0.2,
  architecture = "CNN",
  dataset_id = "peds-0192",
  infra = "AWS",
)

wandb.init(
  project="detect-pedestrians",
  notes="tweak baseline",
  tags=["baseline", "paper1"],
  config=config,
)
```

**Suggested usage**

1. **Config**: Track hyperparameters, architecture, dataset, and anything else you'd like to use to reproduce your model. These will show up in columns— use config columns to group, sort, and filter runs dynamically in the app.
2. **Project**: A project is a set of experiments you can compare together. Each project gets a dedicated dashboard page, and you can easily turn on and off different groups of runs to compare different model versions.
3. **Notes**: A quick commit message to yourself, the note can be set from your script and is editable in the table. We suggest using the notes field instead of overwriting the generated run name.
4. **Tags**: Identify baseline runs and favorite runs. You can filter runs using tags, and they're editable in the table.

### What is the difference between  .log\(\) and .summary\(\)?  

The summary is the value that shows in the table while log will save all the values for plotting later.  

For example you might want to call `wandb.log` every time the accuracy changes.   Usually you can just use .log.  `wandb.log()` will also update the summary value by default unless you have set summary manually for that metric

The scatterplot and parallel coordinate plots will also use the summary value while the line plot plots all of the values set by .log

The reason we have both is that some people like to set the summary manually because they want the summary to reflect for example the optimal accuracy instead of the last accuracy logged.

