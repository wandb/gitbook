---
description: Run Sweeps using Jupyter notebooks
---

# Running Sweeps in Jupyter

Sweeps allow you to easily try out a large number of hyperparameters while tracking model performance and logging all the information you need to reproduce experiments. There are two major APIs for Sweeps: one using the command line \([guide here](quickstart.md)\) and the other using pure Python. This guide describes how to use the second API to run a sweep inside a Jupyter notebook.

{% hint style="info" %}
 You can try out running sweeps in Jupyter notebooks now, no install required, using Colab! We've got examples in [PyTorch](%20http://wandb.me/sweeps-colab) and [Keras](http://wandb.me/tf-sweeps-colab), plus a video walkthrough!
{% endhint %}

{% embed url="https://www.youtube.com/watch?v=9zrmUIlScdY" %}

## 0. How do sweeps work?

Two components work together in a sweep: a _controller_ on the central sweep server, which picks out new hyperparameter combinations to try, and _agents_, running in any number of processes on any number of machines, which query the server for hyperparameters, use them to run model training, and then report the results back to the controller.

The benefit of this setup is in parallelization: we handle tricky aspects like tracking state over time and communicating between machines, so you can focus on the machine learning. That means it's as easy as, say, opening two copies of the same Google Colab notebook in two tabs to get a 2x speed-up in your hyperparameter search!

![](../../.gitbook/assets/image%20%2873%29.png)

## 1. Initialize the sweep

To get started, we need to configure the sweep: decide which hyperparameters we will search over, which `method` we will use to pick new hyperparameters, and so on. We store all of this information in a dictionary \(or dictionary-like\) Python data structure, the `sweep_config`. For details, see [this guide](https://docs.wandb.ai/guides/sweeps/configuration).

We then initialize our sweep by calling `wandb.sweep`. Note that no experiments are launched yet! We still need to define a training function and pass it to an agent.

```python
import wandb

sweep_config = {
  "name" : "my-sweep",
  "method" : "random",
  "parameters" : {
    "epochs" : {
      "values" : [10, 20, 50]
    },
    "learning_rate" :{
      "min": 0.0001,
      "max": 0.1
    }
  }
}

sweep_id = wandb.sweep(sweep_config)
```

`wandb.sweep` returns an identifier for the sweep. Agents need that identifier to join in.

For more details, check out the reference docs for `wandb.sweep` here:

{% page-ref page="../../ref/python/sweep.md" %}

## 2. Run an agent

Now, in any process on any machine \(including the one we just used to initialize the sweep\), we just

1. define a function to run training based on those hyperparameters, and
2. pass that function, plus the `sweep_id`, to `wandb.agent`. 

That's all we need to do! If we want to search more quickly, we can repeat this procedure on more machines.

```python
def train():
    with wandb.init() as run:
        config = wandb.config
        model = make_model(config)
        for epoch in range(config["epochs"]):
            loss = model.fit()  # your model training code here
            wandb.log({"loss": loss, "epoch": epoch})

count = 5 # number of runs to execute
wandb.agent(sweep_id, function=train, count=count)
```

For more details, check out the reference docs for `wandb.agent` here:

{% page-ref page="../../ref/python/agent.md" %}

