# Quickstart

The proceeding Quickstart demonstrates how to define, initialize, and run a sweep. There are four main steps:

1. Set up your training code
2. Define the sweep configuration
3. Initialize the sweep
4. Start the sweep agent
5. Visualize results (optional)
6. Stop the agent (optional)

{% hint style="info" %}
Copy and paste the following code snippets into a Jupyter Notebook.&#x20;
{% endhint %}

Before you get started, make sure you log in to Weights & Biases:

```python
import wandb
wandb.login()
```

### Set up your training code

Define a training function that takes in hyperparameter values from `wandb.config` and uses them to train a model and return metrics.

```python
import numpy as np 
import random

# 🐝 Step 1: Define training function that takes in hyperparameter 
# values from `wandb.config` and uses them to train a model and return metric
def train_one_epoch(epoch, lr, bs): 
  acc = 0.25 + ((epoch/30) +  (random.random()/10))
  loss = 0.2 + (1 - ((epoch-1)/10 +  random.random()/5))
  return acc, loss

def evaluate_one_epoch(epoch): 
  acc = 0.1 + ((epoch/20) +  (random.random()/10))
  loss = 0.25 + (1 - ((epoch-1)/10 +  random.random()/6))
  return acc, loss

def main():
    # Use the wandb.init() API to generate a background process 
    # to sync and log data as a Weights and Biases run.
    # Optionally provide the name of the project. 
    run = wandb.init(project='my-first-sweep')

    # note that we define values from `wandb.config` instead of 
    # defining hard values
    lr  =  wandb.config.lr
    bs = wandb.config.batch_size
    epochs = wandb.config.epochs

    for epoch in np.arange(1, epochs):
      train_acc, train_loss = train_one_epoch(epoch, lr, bs)
      val_acc, val_loss = evaluate_one_epoch(epoch)

      wandb.log({
        'epoch': epoch, 
        'train_acc': train_acc,
        'train_loss': train_loss, 
        'val_acc': val_acc, 
        'val_loss': val_loss
      })
```

Optionally provide the name of the project where you want the output of the W\&B Run to be stored (`project` parameter in [`wandb.init`](https://docs.wandb.ai/ref/python/init)). If the project is not specified, the run is put in an "Uncategorized" project.

{% hint style="warning" %}
Both the W\&B Sweep and the W\&B Run must be in the same project. Therefore, the name you provide when you initialize Weights & Biases must match the name of the project you provide when you initialize a W\&B Sweep.
{% endhint %}

For more information on how to add Weights & Biases SDK to your code, see [Add W\&B to your code](https://docs.wandb.ai/guides/sweeps/add-w-and-b-to-your-code).&#x20;

### Define the sweep configuration

Within a YAML file, specify what hyperparameters you want to sweep over and. For more information about configuration options, see [Define sweep configuration](https://docs.wandb.ai/guides/sweeps/define-sweep-configuration).

The proceeding example demonstrates a sweep configuration that uses a random search (`'method':'random'`). The sweep will randomly select a random set of values listed in the configuration for the batch size, epoch, and the learning rate.

Throughout the sweeps, Weights & Biases will maximize the metric specified in the metric key (`metric`). In the following example, W\&B will maximize (`'goal':'maximize'`) the validation accuracy (`'val_acc'`).

```python
# 🐝 Step 2: Define sweep config
sweep_configuration = {
    'method': 'random',
    'name': 'sweep',
    'metric': {'goal': 'maximize', 'name': 'val_acc'},
    'parameters': 
    {
        'batch_size': {'values': [16, 32, 64]},
        'epochs': {'values': [5, 10, 15]},
        'lr': {'max': 0.1, 'min': 0.0001}
     }
}
```

### Initialize the Sweep

Weights & Biases uses a _Sweep Controller_ to manage sweeps on the cloud (standard), locally (local) across one or more machines. For more information about Sweep Controllers, see [Search and stop algorithms locally](https://docs.wandb.ai/guides/sweeps/local-controller).&#x20;

A sweep identification number is returned when you initialize a sweep:

```python
# 🐝 Step 3: Initialize sweep by passing in config
sweep_id = wandb.sweep(sweep=sweep_configuration, project='my-first-sweep')
```

Optionally provide the name of the project where you want the output of the sweep to be stored. If the project is not specified, the run is put in an "Uncategorized" project.

For more information about initializing sweeps, see [Initialize sweeps](https://docs.wandb.ai/guides/sweeps/initialize-sweeps).

### Start the sweep agent

Use the [`wandb.agent`](https://docs.wandb.ai/ref/python/agent) API call to start a Weights & Biases sweep.

```python
# 🐝 Step 4: Call to `wandb.agent` to start a sweep
wandb.agent(sweep_id, function=main, count=4)
```

### Visualize results (optional)

Open your project to see your live results in the W\&B Sweep dashboard. With just a few clicks, construct rich, interactive charts like [parallel coordinates plots](../../ref/app/features/panels/parallel-coordinates.md),[ parameter importance analyses](../../ref/app/features/panels/parameter-importance.md), and [more](../../ref/app/features/panels/).

[Example dashboard →](https://wandb.ai/anmolmann/pytorch-cnn-fashion/sweeps/pmqye6u3)

<figure><img src="../../.gitbook/assets/Screen Shot 2022-09-22 at 7.18.38 PM.png" alt=""><figcaption></figcaption></figure>

For more information about how to visualize results, see [Visualize sweep results](https://docs.wandb.ai/guides/sweeps/visualize-sweep-results).

### Stop the agent (optional)

From the terminal, hit `Ctrl+c` to stop the run that the Sweep agent is currently running. To kill the agent, hit `Ctrl+c` again after the run is stopped.
