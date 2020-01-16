---
description: Use W&B Sweeps to manage hyperparameter search and hyperparameter optimization
---

# Sweeps Overview

Automate hyperparameter optimization and exploration with Sweeps.

### Benefits of using W&B Sweeps 

1. **Quick setup**: With just a few lines of code you can run W&B sweeps.
2.  **Transparent**: We cite all the algorithms we're using, and [our code is open source](https://github.com/wandb/client/tree/master/wandb/sweeps).
3. **Powerful**: Our sweeps are completely customizable and configurable. You can launch a sweep across dozens of machines, and it's just as easy as starting a sweep on your laptop.

### Common Use Cases

1. **Explore**: Efficiently sample the space of hyperparameter combinations to discover promising regions and build an intuition about your model.
2. **Optimize**:  Use sweeps to achieve the next level of model performance.

### Approach

1. **Add wandb**: In your Python script, add a couple lines of code to log hyperparameters and output metrics from your script. [Read more →](https://docs.wandb.com/quickstart)
2.  **Write config**: Define the variables and ranges to sweep over. Pick a search strategy— we support grid, random, and Bayesian search. Set early stopping to automatically kill off poorly performing runs.
3. **Start sweep**: Launch the sweep server. We host this central controller and coordinate between the agents that execute the sweep.
4. **Start agent\(s\)**: Run this command on each machine you'd like to use to train models in the sweep. The agents ask the central sweep server what hyperparameters to try next, and then they execute the runs.
5. **Visualize results**: Open our live dashboard to see all your results in one central place.

![](../../.gitbook/assets/central-sweep-server-3%20%281%29.png)

## Sweep Commands

### Step 1: Launch a new sweep

Our central server coordinates between all agents executing the sweep.  Set up a sweep config file and run this command to get started:

```text
wandb sweep sweep.yaml
```

This command will print out a **sweep ID**. Copy that to use in the next step!

### Step 2: Launch agents

On each machine that you'd like to execute the sweep, start an agent with the sweep ID. You'll want to use the same sweep ID for all agents who are performing the same sweep.

In a shell on your own machine, run the wandb agent command which will ask the server for commands to run:

```text
wandb agent your-sweep-id
```

You can run wandb agent on multiple machines or in multiple processes on the same machine and each agent will poll the central W&B Sweep server for the next set of hyperparameters to run.

### Step 3: View results

Open your project to see your live results in the sweep dashboard.

![](../../.gitbook/assets/image%20%2823%29.png)

## Sweep Resources

{% page-ref page="quickstart.md" %}

{% page-ref page="add-to-existing.md" %}

{% page-ref page="../configuration.md" %}

{% page-ref page="../local-controller.md" %}

{% page-ref page="../python-api.md" %}

