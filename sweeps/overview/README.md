---
description: Use W&B to manage hyperparameter searches
---

# Sweeps Overview

Hyperparameter sweeps can help you optimally tune an existing model or efficiently sample a model configuration space for promising regions and ideas.

## Getting Started

### Initialize the project

Instead of manually tracking variables and results, write a short config for the hyperparameter ranges of interest and the search strategy \(grid, random, or Bayes\). Start the sweep in two commands, with optional early stopping. Our Python API will schedule, run, store, and visualize all the relevant details of your experiments.

In your project repo, initialize your project from the command line. This allows you to select the project you want to save runs into.

```text
wandb init
```

### Create a sweep configuration

This feature is not currently supported on Windows. 

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

{% page-ref page="getting-started.md" %}

{% page-ref page="add-sweeps-to-existing-project.md" %}

```text
wandb agent SWEEP_ID
```

{% page-ref page="../configuration.md" %}

{% page-ref page="../local-controller.md" %}

{% page-ref page="../python-api.md" %}

## Common **Issues**

### **Sweeps agents stop after the first runs finish**

One common reason for this is that the metric your are optimizing in your configuration YAML file is not a metric that you are logging. For example, you could be optimizing the metric **f1**, but logging **validation\_f1**. Double check that you're logging the exact metric name that you're optimizing.

