---
description: Retain the flexibility and interactivity of Jupyter and add robust logging.
---

# Tracking Jupyter Notebooks

Use Weights & Biases with Jupyter to get interactive visualizations without leaving your notebook. Combine custom analysis, experiments, and prototypes, all fully logged!

## **Use Cases for W&B with Jupyter notebooks**

1. **Iterative experimentation**: Run and re-run experiments, tweaking parameters, and have all the runs you do saved automatically to W&B without having to take manual notes along the way.
2. **Code saving**: When reproducing a model, it's hard to know which cells in a notebook ran, and in which order. Turn on code saving on your [settings page ](https://app.wandb.ai/settings)to save a record of cell execution for each experiment.
3. **Custom analysis**: Once runs are logged to W&B, it's easy to get a dataframe from the API and do custom analysis, then log those results to W&B to save and share in reports.

## Getting started in a notebook

Start your notebook with the following code to install W&B and link your account:

```python
!pip install wandb -qqq
import wandb
wandb.login()
```

Next, set up your experiment and save hyperparameters:

```python
wandb.init(project="jupyter-projo",
           config={
               "batch_size": 128,
               "learning_rate": 0.01,
               "dataset": "CIFAR-100",
           })
```

After running `wandb.init()` , start a new cell with `%%wandb` to see live graphs in the notebook. If you run this cell multiple times, data will be appended to the run.

```python
%%wandb

# Your training loop here
```

Try it for yourself in this [quick example notebook →](http://wandb.me/jupyter-interact-colab)

![](../../.gitbook/assets/jupyter-widget.png)

As an alternative to the `%%wandb` magic, after running `wandb.init()` you can end any cell with `wandb.run` to show in-line graphs:

```python
# Initialize wandb.run first
wandb.init()

# If cell outputs wandb.run, you'll see live graphs
wandb.run
```

{% hint style="info" %}
Want to know more about what you can do with W&B? Check out our [guide to logging data and media](log/), learn [how to integrate us with your favorite ML toolkits](../integrations/), or just dive straight into the [reference docs](../../ref/python/) or our [repo of examples](https://github.com/wandb/examples).
{% endhint %}

## Additional Jupyter features in W&B

1. **Easy authentication in Colab**: When you call `wandb.init` for the first time in a Colab, we automatically authenticate your runtime if you're currently logged in to W&B in your browser. On the overview tab of your run page, you'll see a link to the Colab.
2. **Launch dockerized Jupyter**: Call `wandb docker --jupyter` to launch a docker container, mount your code in it, ensure Jupyter is installed, and launch on port 8888.
3. **Run cells in arbitrary order without fear**: By default, we wait until the next time `wandb.init` is called to mark a run as "finished". That allows you to run multiple cells \(say, one to set up data, one to train, one to test\) in whatever order you like and have them all log to the same run. If you turn on code saving in [settings](https://app.wandb.ai/settings), you'll also log the cells that were executed, in order and in the state in which they were run, enabling you to reproduce even the most non-linear of pipelines. To mark a run as complete manually in a Jupyter notebook, call `run.finish`.

```python
import wandb

run = wandb.init()

# training script and logging goes here

run.finish()
```

## Common questions

### How do I silence W&B info messages?

To disable standard wandb logging and info messages \(e.g. project info at the start of a run\), run the following in a notebook cell _before_ running `wandb.login`:

{% tabs %}
{% tab title="Jupyter Magic" %}
```python
%env WANDB_SILENT=True
```
{% endtab %}

{% tab title="Python" %}
```python
import os

os.environ["WANDB_SILENT"] = "True"
```
{% endtab %}
{% endtabs %}

If you see log messages like `INFO    SenderThread:11484 [sender.py:finish():979]`  in your notebook, you can disable those with the following:

```python
import logging

logger = logging.getLogger("wandb")
logger.setLevel(logging.ERROR)
```

### How do I set the `WANDB_NOTEBOOK_NAME`?

If you're seeing the error message `"Failed to query for notebook name, you can set it manually with the WANDB_NOTEBOOK_NAME environment variable,"` you can resolve it by setting the environment variable. There's multiple ways to do so:

{% tabs %}
{% tab title="Jupyter Magic" %}
```python
%env "WANDB_NOTEBOOK_NAME" "notebook name here"
```
{% endtab %}

{% tab title="Pure Python" %}
```python
import os

os.environ["WANDB_NOTEBOOK_NAME"] = "notebook name here"
```
{% endtab %}
{% endtabs %}

