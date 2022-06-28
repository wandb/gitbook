---
description: >-
  Run search and stopping algorithms locally, instead of using our cloud-hosted
  service
---

# Local Controller

By default the hyper-parameter controller is hosted by W\&B as a cloud service. W\&B agents communicate with the controller to determine the next set of parameters to use for training. The controller is also responsible for running early stopping algorithms to determine which runs can be stopped.

The local controller feature allows the user to run search and stopping algorithms locally. The local controller gives the user the ability to inspect and instrument the code in order to debug issues as well as develop new features which can be incorporated into the cloud service.

{% hint style="warning" %}
This feature is offered to support faster development and debugging of new algorithms for the Sweeps tool. It is not intended for actual hyperparameter optimization workloads.
{% endhint %}

## Running the local controller from the command line

For this to work you will need to install a version of wandb by running:&#x20;

```
pip install wandb sweeps 
```

After this the simplest method is to indicate you want to use the local controller when starting your sweep:

```python
wandb sweep --controller sweep-config.yaml
```

Alternatively, you can get more control by initializing a sweep separately from starting the controller. You'll need to add the following to your sweep's configuration file:

{% code title="sweep-config.yaml" %}
```yaml
controller:
  type: local
```
{% endcode %}

Then, after initializing with `wandb sweep`, you can start a controller using `wandb controller`:

```python
wandb sweep sweep-config.yaml
# wandb sweep command will print a sweep_id
wandb controller {user}/{entity}/{sweep_id}
```

## Run a local controller inside Python

When developing and debugging your own parameter search algorithms, you might wish to run the sweep controller from Python.

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

sweep = wandb.controller()
sweep.configure_search("grid")
sweep.configure_program("train-dummy.py")
sweep.configure_controller(type="local")
sweep.configure_parameter("param1", value=3)
sweep.create()
sweep.run()
```
