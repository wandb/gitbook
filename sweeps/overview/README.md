---
description: Use W&B Sweeps to manage hyperparameter searches
---

# Sweeps Overview

Hyperparameter sweeps can help you optimally tune an existing model or efficiently sample a model configuration space for promising regions and ideas. 

Instead of manually tracking variables, launch commands, and results, write a short config for the hyperparameter ranges of interest \(as a YAML file or Python dictionary\). Pick a search strategy \(grid, random, or Bayes\) and start the sweep in two commands, with optional early stopping. Our Python API will automatically schedule, launch, and store all the runs in your sweep. As the sweep runs, we'll visualize the relative importance of different hyperparameters and organize all the details of your experiments in your browser.

![](../../.gitbook/assets/central-sweep-server-3%20%281%29.png)

### Step 1: Launch a new sweep

Our central server coordinates between all agents executing the sweep.  Set up a sweep config file and run this command to get started:

```text
wandb sweep sweep.yaml
```

This command will print out a sweep ID. Copy that to use in the next step!

### Step 2: Launch agents

On each machine that you'd like to execute the sweep, start an agent with the sweep ID. You'll want to use the same sweep ID for all agents who are performing the same sweep.

```text
wandb agent your-sweep-id
```

### Step 3: View results

Open your project in our web interface to see your live results in the sweep dashboard.

![](../../.gitbook/assets/image%20%2823%29.png)



## Links

{% page-ref page="quickstart.md" %}

{% page-ref page="add-to-existing.md" %}

{% page-ref page="../configuration.md" %}

{% page-ref page="../local-controller.md" %}

{% page-ref page="../python-api.md" %}

