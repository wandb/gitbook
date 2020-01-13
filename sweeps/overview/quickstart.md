---
description: How to use sweeps in a new project
---

# Sweeps Quickstart

Note: If you're already using W&B and want to add sweeps to an existing project, see the [next section](add-to-existing.md).

### Setup W&B Logging

In the repository or folder for your existing training code, create a W&B account \(or log in to an existing account\) from the command line. This links experiment runs from the current directory to your account and makes the visualizations accessible from your browser.

```python
pip install wandb # if you haven't already
wandb login
```

### Create a sweep configuration

Specify your training script, parameter ranges, search strategy, and stopping criteria in a YAML file. W&B will pass these parameters and their values as command line arguments to your training script. Make sure your script can parse these arguments correctly—directly from the command line for use in your script.

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

This configuration will use the Bayes optimization method to choose sets of hyperparameter values with which to call your program. It will launch experiments with the following syntax:

```text
python train.py --learning-rate=0.005 --optimizer=adam
python train.py --learning-rate=0.03 --optimizer=sgd
...
```

#### Log your result metric

If you're specifying a metric to optimize, your script must log it to W&B:

```python
# [model training code that outpus validation loss as, e.g., valid_loss]
wandb.log({"val_loss" : valid_loss})
```

### Create sweep

Set up the sweep from the command line. This returns a unique identifier for the sweep \(SWEEP\_ID\) and a URL to track all your runs.

```python
wandb sweep sweep.yaml # prints out SWEEP_ID and tracking URL
```

### Launch agent\(s\) to run sweep

Launch one or more W&B agents with the given SWEEP\_ID. Multiple agents can be launched in parallel. Each agent will request parameters from our parameter server and use these to launch your training script in your environment. Once an agent is launched, go to the tracking URL to see live updates of the configuration settings and the results of different runs in your sweep. You can optionally stop the agent from the browser.

```text
wandb agent SWEEP_ID
```

{% hint style="info" %}
The agent will execute the script specified by the `program` field in the sweep configuration.  The script will need to be accessible from the path where the agent command is run.  The agent will always run the latest version of the script so be careful not to modify the script while the sweep is running.
{% endhint %}

{% hint style="info" %}
The agent can be configured to launch a limited number of runs by specifying the count parameter.   For example, to limit the agent to only launching a single run use the command line:  
`wandb agent --count 1 SWEEP_ID`
{% endhint %}

