---
description: Use W&B Sweeps to manage hyperparameter searches
---

# Sweeps Overview

Hyperparameter sweeps can help you optimally tune an existing model or efficiently sample a model configuration space for promising regions and ideas. 

Instead of manually tracking variables, launch commands, and results, write a short config for the hyperparameter ranges of interest \(as a YAML file or Python dictionary\). Pick a search strategy \(grid, random, or Bayes\) and start the sweep in two commands, with optional early stopping. Our Python API will automatically schedule, launch, and store all the runs in your sweep. As the sweep runs, we'll visualize the relative importance of different hyperparameters and organize all the details of your experiments in your browser.

Ways to run a sweep:

![](../../.gitbook/assets/central-sweep-server-3%20%281%29.png)

#### 1\) Configure with YAML, run agent on multiple machines

### Step 1: Launch a new sweep

This approach makes it easy to run distributed sweeps on multiple machines.

Our central server coordinates between all agents executing the sweep.  Set up a sweep config file and run this command to get started:

Setup a configuration file, either in a text editor or using our editor.

```text
wandb sweep sweep.yaml
```

```text
program: train.py
method: grid
metric:
  name: loss
  goal: minimize
parameters:
  learning_rate:
    distribution: categorical
    values:
      - 0.1
      - 0.2
      - 0.3
```

This command will print out a sweep ID. Copy that to use in the next step!

Register your yaml file

### Step 2: Launch agents

```text
wandb sweep sweep.yaml
```

On each machine that you'd like to execute the sweep, start an agent with the sweep ID. You'll want to use the same sweep ID for all agents who are performing the same sweep.

In a shell on your own machine, run the wandb agent command which will ask the server for commands to run:

```text
wandb agent your-sweep-id
```

```text
wandb agent
```

### Step 3: View results

You can run wandb agent on multiple machines or in multiple processes on the same machine and each agent will poll the central wandb server for the next set of hyperparameters to run.

Open your project in our web interface to see your live results in the sweep dashboard.

### 2\) The python approach

![](../../.gitbook/assets/image%20%2823%29.png)

This way is good for running inside a notebook.



Setup a sweep as a python object:

## Links

```text
sweep_config = {
    'method': 'grid'
    'metric': {
      'name': 'accuracy',
      'goal': 'minimize'   
    },
    'parameters': {
        'learn_rate': {
            'values': [0.1, 0.2, 0.3]
        }
    }
}
    
sweep_id = wandb.sweep(sweep_config, entity="sweep", project="simpsons")
```

Setup a training function

```text
def train():
    # Default values for hyper-parameters we're going to sweep over
    config_defaults = {
        'learning_rate': 0.1,
    }

    # Initilize a new wandb run
    wandb.init(config=config_defaults)
    
    # Config is a variable that holds and saves hyperparameters and inputs
    config = wandb.config
    
    # training code goes here 
```

Call wandb agent in your python code or jupyter notebook, which will repeatedly ask the server for configuration hyper-parameters and call your training function.

```text
wandb.agent(sweep_id, train)
```

{% page-ref page="quickstart.md" %}

{% page-ref page="add-to-existing.md" %}

{% page-ref page="../configuration.md" %}

{% page-ref page="../local-controller.md" %}

{% page-ref page="../python-api.md" %}

