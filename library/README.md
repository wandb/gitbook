---
description: >-
  Weights & Biases for experiment tracking, dataset versioning, and model
  management
---

# Library

Use the `wandb` Python library to track machine learning experiments with a few lines of code. If you're using a popular framework like [PyTorch](../integrations/pytorch.md) or [Keras](../integrations/keras.md), we have lightweight [integrations](../integrations/).

## Integrating W&B in your script

Below are the simple building blocks to track an experiment with W&B. We also have a whole host of special integrations for [PyTorch](../integrations/pytorch.md), [Keras](../integrations/keras.md), [Scikit](../integrations/scikit.md), etc. See [**Integrations**](../integrations/).

1. \*\*\*\*[**wandb.init\(\)**](init.md): Initialize a new run at the top of your script. This returns a Run object and creates a local directory where all logs and files are saved, then streamed asynchronously to a W&B server. If you want to use a private server instead of our hosted cloud server, we offer [Self-Hosting](../self-hosted/).
2. \*\*\*\*[**wandb.config**](config.md): Save a dictionary of hyperparameters such as learning rate or model type. The model settings you capture in config are useful later to organize and query your results.
3. \*\*\*\*[**wandb.log\(\)**](log.md): Log metrics over time in a training loop, such as accuracy and loss. By default, when you call wandb.log\(\) it appends a new step to the history object and updates the summary object. 
   * **history**: An array of dictionary-like objects that tracks metrics over time. These time series values are shown as default line plots in the UI.
   * **summary**: By default, the final value of a metric logged with wandb.log\(\). You can set the summary for a metric manually to capture the highest accuracy or lowest loss instead of the final value. These values are used in the table, and plots that compare runs — for example, you could visualize at the final accuracy for all runs in your project.
4. \*\*\*\*[**Artifacts**](../artifacts/): Save outputs of a run, like the model weights or a table of predictions. This lets you track not just model training, but all the pipeline steps that affect the final model.

## Best Practices

The `wandb` library is incredibly flexible. Here are some suggested guidelines.

1. **Config**: Track hyperparameters, architecture, dataset, and anything else you'd like to use to reproduce your model. These will show up in columns— use config columns to group, sort, and filter runs dynamically in the app.
2. **Project**: A project is a set of experiments you can compare together. Each project gets a dedicated dashboard page, and you can easily turn on and off different groups of runs to compare different model versions.
3. **Notes**: A quick commit message to yourself, the note can be set from your script and is editable in the table.
4. **Tags**: Identify baseline runs and favorite runs. You can filter runs using tags, and they're editable in the table.

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

## What data is logged?

All the data logged from your script is saved locally to your machine in a **wandb** directory, then sync'd to the W&B cloud or your [private server](../self-hosted/).

### **Logged Automatically**

* **System metrics**: CPU and GPU utilization, network, etc. These come from [nvidia-smi](https://developer.nvidia.com/nvidia-system-management-interface) and are shown in the System tab on the run page.
* **Command line**: The stdout and stderr are picked up and show in the logs tab on the run page.

Turn on [Code Saving](http://wandb.me/code-save-colab) in your account's [Settings page](https://wandb.ai/settings) to get:

* **Git commit**: Pick up the latest git commit and see it on the overview tab of the run page, as well as a diff.patch file if there are any uncommitted changes.
* **Files**: The `requirements.txt` file will be uploaded and shown on the files tab of the run page, along with any files you save to the **wandb** directory for the run.

### Logged with specific calls

Where data and model metrics are concerned, you get to decide exactly what you want to log.

* **Dataset**: You have to specifically log images or other dataset samples for them to stream to W&B.
* **PyTorch gradients**: Add `wandb.watch(model)` to see gradients of the weights as histograms in the UI.
* **Config**: Log hyperparameters, a link to your dataset, or the name of the architecture you're using as config parameters, passed in like this: `wandb.init(config=your_config_dictionary)`.
* **Metrics**: Use `wandb.log()` to see metrics from your model. If you log metrics like accuracy and loss from inside your training loop, you'll get live updating graphs in the UI.

