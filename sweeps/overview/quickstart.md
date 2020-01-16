# Sweeps Quickstart

Start from any machine learning model and get a hyperparameter sweep running in minutes. Want to see a working example? Here's [example code](https://github.com/wandb/examples/tree/master/pytorch-cnn-fashion) and an [example dashboard](https://app.wandb.ai/carey/pytorch-cnn-fashion/sweeps/v8dil26q).

![](../../.gitbook/assets/image%20%2816%29.png)

{% hint style="info" %}
Already have a Weights & Biases project? [Skip to our next Sweeps tutorial →](add-to-existing.md)
{% endhint %}

## 1. Add wandb

### **Set up your account**

1. Start with a W&B account.  [Create one now →](http://app.wandb.ai/)
2. Go to your project folder in your terminal and install our library: `pip install wandb`
3. Inside your project folder, log in to W&B: `wandb login`

### **Set up your Python training script**

1. Import our library `wandb`  
2. Make sure your hyperparameters can be properly set by the sweep. Define them in a dictionary at the top of your script and pass them into wandb.init.
3. Log metrics to see them in the live dashboard. 

```python
import wandb

# Set up your default hyperparameters before wandb.init
# so they get properly set in the sweep
hyperparameter_defaults = dict(
    dropout = 0.5,
    channels_one = 16,
    channels_two = 32,
    batch_size = 100,
    learning_rate = 0.001,
    epochs = 2,
    )

# Pass your defaults to wandb.init
wandb.init(config=hyperparameter_defaults)
config = wandb.config

# Your model here ...

# Log metrics inside your training loop
metrics = {'accuracy': accuracy, 'loss': loss}
wandb.log(metrics)
```

## 2. Sweep Config

Specify your training script, parameter ranges, search strategy, and stopping criteria in a YAML file. W&B will pass these parameters and their values as command line arguments to your training script. Make sure your script can parse these arguments correctly—directly from the command line for use in your script.

Here are some config resources:

1. [Example YAML](https://github.com/wandb/examples/blob/master/pytorch-cnn-fashion/sweep-grid-hyperband.yaml): a code example of a script and YAML file to do a sweep
2. [Configuration](../configuration.md): full specs to set up your sweep config
3. [Jupyter Notebook](../python-api.md): set up your sweep config with a Python dictionary instead of a YAML file
4. Generate config from UI: next page link
5. Feed in prior runs: next page link

Here's an example sweep config YAML file called **sweep.yaml**:

```text
program: train.py
method: bayes
metric:
  name: val_loss
  goal: minimize
parameters:
  learning_rate:
    min: 0.001
    max: 0.1
  optimizer:
    values: ["adam", "sgd"]
```

{% hint style="warning" %}
If you specify a metric to optimize, make sure you're logging it. In this example, I have **val\_loss** in my config file, so I have to log that exact metric name in my script:

`wandb.log({"val_loss": validation_loss})`
{% endhint %}

This example configuration will use the Bayes optimization method to choose sets of hyperparameter values with which to call your program. It will launch experiments with the following syntax:

```text
python train.py --learning_rate=0.005 --optimizer=adam
python train.py --learning_rate=0.03 --optimizer=sgd
```

{% hint style="info" %}
If you're using argparse, we recommend that you use underscores in your variable names instead of hyphens.
{% endhint %}

## 3. Initialize a sweep

Our central server coordinates between all agents executing the sweep.  Set up a sweep config file and run this command to get started:

```text
wandb sweep sweep.yaml
```

This command will print out a **sweep ID**. Copy that to use in the next step!

## 4. Launch agent\(s\)

On each machine that you'd like to execute the sweep, start an agent with the sweep ID. You'll want to use the same sweep ID for all agents who are performing the same sweep.

In a shell on your own machine, run the wandb agent command which will ask the server for commands to run:

```text
wandb agent your-sweep-id
```

You can run wandb agent on multiple machines or in multiple processes on the same machine, and each agent will poll the central W&B Sweep server for the next set of hyperparameters to run.

## 5. Visualize results

Open your project to see your live results in the sweep dashboard.

[Example dashboard →](https://app.wandb.ai/carey/pytorch-cnn-fashion)

![](../../.gitbook/assets/image%20%2825%29.png)

## 

