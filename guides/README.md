---
description: >-
  Use Weights & Biases for experiment tracking, dataset versioning, and model
  management
---

# Guides

Use the `wandb` Python library to track machine learning experiments with a few lines of code. If you're using a popular framework like [PyTorch](integrations/pytorch.md) or [Keras](integrations/keras.md), we have lightweight [integrations](integrations/).

Export your data to Python using our [Public API](track/public-api-guide.md).

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

All the data logged from your script is saved locally to your machine in a **wandb** directory, then sync'd to the W&B cloud or your [private server](self-hosted/).

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

