---
description: Assumes you're already using wandb.config
---

# Add Sweeps to Existing W&B Project

If you already have a script integrated with W&B, you're probably using wandb.config to manage your experiment  settings—this is how you set and track different hyperparameter values over time. In Sweeps, W&B will choose hyperparameter values for you and pass them in through the config.

### Create a sweep configuration

Specify your training script, parameter ranges, search strategy, and stopping criteria in a YAML file. 

Specify your training script, parameter ranges, search strategy, and stopping criteria in a YAML file. W&B will pass the parameter names and their values by setting them as key-value pairs in the wandb.config object for each run of your training script. Make sure your script can parse these correctly—explicitly read the parameters in from wandb.config after the wandb.init\(\) call and before using them in your script.

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

This configuration will use the Bayes optimization method to choose sets of hyperparameter values with which to call your program. 

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

Run one or more W&B agents with the given SWEEP\_ID. Multiple agents can be run in parallel. Each agent will request parameters from our parameter server and use these to launch your training script in your environment. Once an agent is running, go to the tracking URL to see live updates of the configuration settings and the results of different runs in your sweep. You can optionally stop the agent from the browser.

```text
wandb agent SWEEP_ID
```

