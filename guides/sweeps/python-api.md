---
description: Run Sweeps from Jupyter notebooks
---

# Sweep from Jupyter Notebook

{% hint style="info" %}
 You can try out running Sweeps in Jupyter notebooks now, no install required, using Colab! We've got examples in [PyTorch](%20http://wandb.me/sweeps-colab) and [Keras](http://wandb.me/tf-sweeps-colab).
{% endhint %}

## Initialize a sweep

```python
import wandb

sweep_config = {
  "name": "My Sweep",
  "method": "grid",
  "parameters": {
        "param1": {
            "values": [1, 2, 3]
        }
    }
}

sweep_id = wandb.sweep(sweep_config)
```

You can also choose the project and entity to log the sweep under, using [`wandb.sweep`](../../ref/python/sweep.md) or by setting [Environment Variables](../track/advanced/environment-variables.md).

{% tabs %}
{% tab title="wandb.sweep" %}
```python
sweep_id = wandb.sweep(sweep_config, entity="ghinton", project="capsules")
```
{% endtab %}

{% tab title="Environment Variables" %}
```python
%env WANDB_ENTITY=ghinton
%env WANDB_PROJECT=capsules

sweep_id = wandb.sweep(sweep_config)
```
{% endtab %}
{% endtabs %}

## Run an agent

When running an agent from python, the agent runs a specified function instead of using the `program` key from the sweep configuration file.

```python
import wandb
import time

def train():
    run = wandb.init()
    print("config:", dict(run.config))
    for epoch in range(35):
        print("running", epoch)
        wandb.log({"metric": run.config.param1, "epoch": epoch})
        time.sleep(1)

wandb.agent(sweep_id, function=train)
```

For more details, check out the reference docs for `wandb.agent` here:

{% page-ref page="../../ref/python/agent.md" %}

## Run a local controller

If you want to develop your own parameter search algorithms you can run your controller from python.

The simplest way to run a controller:

```python
sweep = wandb.controller(sweep_id)
sweep.run()
```

If you want more control of the controller loop:

```python
import wandb
sweep = wandb.controller(sweep_id)
while not sweep.done():
    sweep.print_status()
    sweep.step()
    time.sleep(5)
```

Or even more control over the parameters being served:

```python
import wandb
sweep = wandb.controller(sweep_id)
while not sweep.done():
    params = sweep.search()
    sweep.schedule(params)
    sweep.print_status()
```

If you want to specify your sweep entirely with code you can do something like this:

```python
import wandb
from wandb.sweeps import GridSearch,RandomSearch,BayesianSearch

sweep = wandb.controller()
sweep.configure_search(GridSearch)
sweep.configure_program('train-dummy.py')
sweep.configure_controller(type="local")
sweep.configure_parameter('param1', value=3)
sweep.create()
sweep.run()
```

