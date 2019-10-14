---
description: >-
  Get your script integrated quickly to see our experiment tracking and
  visualization features on your own project
---

# Quickstart

Get started logging machine learning experiments in 3 quick steps.

### 1. Install Library

Install our library in an environment using Python 3.

```text
pip install wandb
```

{% hint style="info" %}
If you are training models in an automated environment where it's inconvenient to run shell commands, such as Google's CloudML, you should look at the documentation on [Running in Automated Environments](https://docs.wandb.com/advanced/automated).
{% endhint %}

### 2. Create Account

Sign up for a free account in your shell or go to our [sign up page](https://app.wandb.ai/login?signup=true).

```text
wandb login
```

### 3. Modify your training script

Add a few lines to your script to log hyperparameters and metrics.

{% hint style="info" %}
Weights and Biases is framework agnostic, but if you are using a common ML framework, you may find framework-specific examples even easier for getting started and in many cases we've build framework specific hooks to simplify the integration. You can find examples for [Keras](https://docs.wandb.com/frameworks/keras), [TensorFlow](https://docs.wandb.com/frameworks/tensorflow), [PyTorch](https://docs.wandb.com/frameworks/pytorch), [Fast.ai](https://docs.wandb.com/frameworks/fastai), [Scikit-learn](https://docs.wandb.com/frameworks/scikit), [XGBoost](https://docs.wandb.com/frameworks/xgboost), [Catalyst](https://docs.wandb.com/frameworks/catalyst) and [Jax](https://docs.wandb.com/frameworks/jax-example).
{% endhint %}

#### Initialize Wandb

Initialize `wandb` at the beginning of your script right after the imports.

```text
# Inside my model training code
import wandb
wandb.init(project="my-project")
```

We automatically create the project for you if it doesn't exist. \(See the [wandb.init](library/python/init.md) documentation for more initialization options.\)

#### Declare Hyperparameters

It's easy to save hyperparameters with the [wandb.config](library/python/config.md) object.

```text
wandb.config.dropout = 0.2
wandb.config.hidden_layer_size = 128
```

#### Log Metrics

Log metrics like loss or accuracy as your model trains or log more complicated things like histograms, graphs or images with [wandb.log](library/python/log.md).

Then log a few metrics:

```text
def my_train_loop():
    for epoch in range(10):
        loss = 0 # change as appropriate :)
        wandb.log({'epoch': epoch, 'loss': loss})
```

#### Save Files

Anything saved in the `wandb.run.dir` directory will be uploaded to W&B and saved along with your run when it completes. This is especially convenient for saving the literal weights and biases in your model:

```text
model.save(os.path.join(wandb.run.dir, "mymodel.h5"))
```

Great! Now run your script normally and we'll sync logs in a background process. Your terminal logs, metrics, and files will be synced to the cloud along with a record of your git state if you're running from a git repo.

{% hint style="info" %}
If you're testing and want to disable wandb syncing, set the [environment variable](library/advanced/environment-variables.md) WANDB\_MODE=dryrun
{% endhint %}



