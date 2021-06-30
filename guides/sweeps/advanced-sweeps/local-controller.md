---
description: >-
  Run search and stopping algorithms locally, instead of using our cloud-hosted
  service
---

# Local Controller

By default the hyper-parameter controller is hosted by W&B as a cloud service. W&B agents communicate with the controller to determine the next set of parameters to use for training. The controller is also responsible for running early stopping algorithms to determine which runs can be stopped.

The local controller feature allows the user to run search and stopping algorithms locally. The local controller gives the user the ability to inspect and instrument the code in order to debug issues as well as develop new features which can be incorporated into the cloud service.

{% hint style="info" %}
The local controller is currently limited to running a single agent.
{% endhint %}

## Running the local controller

The simplest method is to indicate you want to use the local controller when starting your sweep:

```python
wandb sweep --controller sweep-config.yaml
```

Alternatively, you can get more control by initializing a sweep separately from starting the controller.  You'll need to add the following to your sweep's configuration file:

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

