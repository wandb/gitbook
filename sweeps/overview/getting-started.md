---
description: How to use sweeps in a new project
---

# Getting started

Note: If you're already using W&B and want to add sweeps to an existing project, see Adding Sweeps to W&B. Otherwise, use p

### Initialize the W&B project

In the repository or folder for your existing training code, initialize a W&B project from the command line. This allows you to create or select the named project in which to store the sweep.

```python
pip install wandb # if you haven't already
wandb init
```

### Create a sweep configuration

Specify your training script, parameter ranges, search strategy, and stopping criteria in a YAML file. W&B will pass these parameters and their values as command line arguments to your training script. Make sure your script can parse these arguments correctly. 

See the [Configuration section ](../configuration.md)for full specs. Here's an example configuration **sweep.yaml**:

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

This configuration will use the Bayes optimization method to choose sets of hyperparameter values with which to call your program. It will launch experiments as follows:

```text
python train.py --learning-rate 0.005 --optimizer adam
python train.py --learning-rate 0.03 --optimizer sgd
...
```

#### Log your result metric

If you're specifying a metric to optimize, your script must log it to W&B:

```python
# [model training code that outpus validation loss as, e.g., valid_loss]
wandb.log({"val_loss" : valid_loss})
```

### Initialize the sweep

Set up the sweep from the command line. This returns a unique identifier for the sweep \(SWEEP\_ID\) and a URL to track all your runs.

```python
wandb sweep sweep.yaml # prints out SWEEP_ID and tracking URL
```

### Run agent\(s\)

Run one or more W&B agents with the given SWEEP\_ID. Multiple agents can be run in parallel. Each agent will request parameters from our parameter server with which to launch your training script in your environment.

```text
wandb agent SWEEP_ID
```

