# Sweeps Quickstart

Start from any machine learning model and get a parallel hyperparameter sweep running in minutes.

{% hint style="info" %}
Want to see a working example? Here's an [example code](https://github.com/wandb/examples/tree/master/examples/pytorch/pytorch-cnn-fashion) and an [example dashboard](https://wandb.ai/anmolmann/pytorch-cnn-fashion/sweeps/pmqye6u3).
{% endhint %}

![Draw insights from large hyperparameter tuning experiments with interactive dashboards.](<../../.gitbook/assets/image (114).png>)

{% hint style="info" %}
Trying to quickly generate a sweep based on runs you've already logged? Check out [this guide](existing-project.md).
{% endhint %}

## 1. Set up `wandb`

### **Set up your account**

1. Start with a W\&B account. [Create one now →](http://app.wandb.ai)
2. Go to your project folder in your terminal and install our library: `pip install wandb`
3. Inside your project folder, log in to W\&B: `wandb login`

### **Set up your Python training script**

{% hint style="info" %}
Trying to run a hyperparameter sweep from a Jupyter notebook? You want [these instructions](python-api.md).
{% endhint %}

1. Import our library `wandb`.
2. Pass the hyperparameter values to `wandb.init` to populate `wandb.config`.
3. Use the values in the config to build your model and execute training.
4. Log metrics to see them in the live dashboard.

See the code snippets below for several ways to set hyperparameter values so that training scripts can be run stand-alone or as part of a sweep.

{% tabs %}
{% tab title="Command Line Arguments" %}
{% code title="train.py" %}
```python
import argparse
import wandb

# Build your ArgumentParser however you like
parser = setup_parser()

# Get the hyperparameters
args = parser.parse_args()

# Pass them to wandb.init
wandb.init(config=args)
# Access all hyperparameter values through wandb.config
config = wandb.config

# Set up your model
model = make_model(config)

# Log metrics inside your training loop
for epoch in range(config["epochs"]):
    val_acc, val_loss = model.fit()
    metrics = {"validation_accuracy": val_acc,
               "validation_loss": val_loss}
    wandb.log(metrics)
```
{% endcode %}
{% endtab %}

{% tab title="In-line Dictionary" %}
{% code title="train.py" %}
```python
import wandb

# Set up your default hyperparameters
hyperparameter_defaults = dict(
    channels=[16, 32],
    batch_size=100,
    learning_rate=0.001,
    optimizer="adam",
    epochs=2,
    )

# Pass your defaults to wandb.init
wandb.init(config=hyperparameter_defaults)
# Access all hyperparameter values through wandb.config
config = wandb.config

# Set up your model
model = make_model(config)

# Log metrics inside your training loop
for epoch in range(config["epochs"]):
    val_acc, val_loss = model.fit()
    metrics = {"validation_accuracy": val_acc,
               "validation_loss": val_loss}
    wandb.log(metrics)
```
{% endcode %}
{% endtab %}

{% tab title="config.py File" %}
{% code title="train.py" %}
```python
import wandb

import config  # python file with default hyperparameters

# Set up your default hyperparameters
hyperparameters = config.config

# Pass them wandb.init
wandb.init(config=hyperparameters)
# Access all hyperparameter values through wandb.config
config = wandb.config

# Set up your model
model = make_model(config)

# Log metrics inside your training loop
for epoch in range(config["epochs"]):
    val_acc, val_loss = model.fit()
    metrics = {"validation_accuracy": val_acc,
               "validation_loss": val_loss}
    wandb.log(metrics)
```
{% endcode %}
{% endtab %}
{% endtabs %}

[See a full code example →](https://github.com/wandb/examples/tree/master/examples/pytorch/pytorch-cnn-fashion)

## 2. Configure your sweep

Set up a YAML file to specify the hyperparameters you wish to sweep over, along with the structure of the sweep like the training script to call, the search strategy and stopping criteria to use, etcetera.

Here are some resources on configuring sweeps:

1. [Example YAML files](https://github.com/wandb/examples/tree/master/examples/keras/keras-cnn-fashion): an example script and several different YAML files
2. [Configuration](configuration.md): full specs to set up your sweep config
   * top-level configurations such as `entity` or `project` for your `wandb` account
   * low-level configurations like command-line arguments for your sweeps program
3. [Jupyter Notebook](python-api.md): set up your sweep config with a Python dictionary instead of a YAML file
4. [Generate config from UI](existing-project.md): take an existing W\&B project and generate a config file
5. [Feed in prior runs](https://docs.wandb.com/sweeps/existing-project#seed-a-new-sweep-with-existing-runs): take previous runs and add them to a new sweep

Here's an example sweep config file called `sweep.yaml`:

{% tabs %}
{% tab title="sweep.yaml" %}
```yaml
program: train.py
method: bayes
metric:
  name: validation_loss
  goal: minimize
parameters:
  learning_rate:
    min: 0.0001
    max: 0.1
  optimizer:
    values: ["adam", "sgd"]
```
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
If you specify a metric to optimize, make sure you're logging it. In this example, I have `validation_loss` in my config file, so I have to log that exact metric name in my script:

`wandb.log({"validation_loss": val_loss})`
{% endhint %}

{% hint style="info" %}
If you want to optimize multiple metrics, consider using a [weighted optimization metric](faq.md#optimizing-multiple-metrics).
{% endhint %}

This example configuration will use a Bayesian search method to choose sets of hyperparameter values to pass as command line arguments to the `train.py` script on each step. The hyperparameters are also accessible via `wandb.config` after `wandb.init` is called.

{% hint style="info" %}
If you're using `argparse` in your script, we recommend that you use underscores in your variable names instead of hyphens.
{% endhint %}

## 3. Initialize a sweep

After you've set up a sweep config file at `sweep.yaml`, run this command to get started:

```python
wandb sweep sweep.yaml
```

This command will print out a _sweep ID_, which includes the entity name and project name. Copy that to use in the next step!

## 4. Launch agent(s)

On each machine or within each process that you'd like to contribute to the sweep, start an "agent". Each agent will poll the central W\&B sweep server you launched with `wandb sweep` for the next set of hyperparameters to run. You'll want to use the same sweep ID for all agents who are participating in the same sweep.

In a shell on your own machine, run the `wandb agent` command:

```python
wandb agent <USERNAME/PROJECTNAME/SWEEPID>
```

## 5. Visualize results

Open your project to see your live results in the sweep dashboard. With just a few clicks, construct rich, interactive charts like [parallel coordinates plots](../../ref/app/features/panels/parallel-coordinates.md),[ parameter importance analyses](../../ref/app/features/panels/parameter-importance.md), and [more](../../ref/app/features/panels/).

[Example dashboard →](https://wandb.ai/anmolmann/pytorch-cnn-fashion/sweeps/pmqye6u3)

![](<../../.gitbook/assets/image (88) (2) (3) (3) (3) (3) (3) (1) (3) (1) (1) (1) (1) (1) (1) (1) (5) (1) (1) (1) (1) (1) (1) (1) (5).png>)

## 6. Stop the agent

From the terminal, hit `Ctrl+c` to stop the run that the sweep agent is currently running. To kill the agent, hit `Ctrl+c` again after the run is stopped.
