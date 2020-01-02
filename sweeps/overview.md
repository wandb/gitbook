# Sweeps Overview

Use W&B to manage hyperparameter sweeps. Sweeps are useful for efficiently finding the best version of your model.

## Getting Started

### Initialize the project

In your project repo, initialize your project from the command line. This allows you to select the project you want to save runs into.

```text
wandb init
```

### Create a sweep configuration

Specify your training script, parameter ranges, search strategy and stopping criteria in a YAML file.

Here's an example configuration **sweep.yaml**:

```text
program: train.py
method: bayes
metric:
  name: val_loss
  goal: minimize
parameters:
  learning-rate:
    min: 0.001
    max: 0.1
  optimizer:
    values: ["adam", "sgd"]
```

### Initialize the sweep

Run this from the command line to get a SWEEP\_ID and a URL to track all your runs.

```text
wandb sweep sweep.yaml # prints out SWEEP_ID.
```

### Run agent\(s\)

Run one or more wandb agents with the SWEEP\_ID. Agents will request parameters from the parameter server and launch your training script.

```text
wandb agent SWEEP_ID
```

{% page-ref page="configuration.md" %}

{% page-ref page="local-controller.md" %}

{% page-ref page="python-api.md" %}

## Common **Issues**

### **Sweeps agents stop after the first runs finish**

One common reason for this is that the metric your are optimizing in your configuration YAML file is not a metric that you are logging. For example, you could be optimizing the metric **f1**, but logging **validation\_f1**. Double check that you're logging the exact metric name that you're optimizing.

