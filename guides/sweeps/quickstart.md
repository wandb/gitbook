# Sweeps Quickstart

Start from any machine learning model and get a hyperparameter sweep running in minutes. Want to see a working example? Here's [example code](https://github.com/wandb/examples/tree/master/examples/pytorch/pytorch-cnn-fashion) and an [example dashboard](https://app.wandb.ai/carey/pytorch-cnn-fashion/sweeps/v8dil26q).

![](../../.gitbook/assets/image%20%2847%29%20%282%29%20%283%29%20%284%29%20%283%29.png)

{% hint style="info" %}
Already have a Weights & Biases project? [Skip to our next Sweeps tutorial →](existing-project.md)
{% endhint %}

## 1. Add wandb

### **Set up your account**

1. Start with a W&B account.  [Create one now →](http://app.wandb.ai/)
2. Go to your project folder in your terminal and install our library: `pip install wandb`
3. Inside your project folder, log in to W&B: `wandb login`

### **Set up your Python training script**

1. Import our library `wandb`.
2. Make sure your hyperparameters can be properly set by the sweep. Define them in a dictionary at the top of your script and pass them into `wandb.init`.
3. Use the values in `wandb.config` to build you model.
4. Log metrics to see them in the live dashboard. 

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
# Access all hyperparameter values through wandb.config
config = wandb.config

# Set up your model
model = make_model(config)

# Log metrics inside your training loop
for epoch in range(config["epochs"]):
    accuracy, loss = model.fit()
    metrics = {'accuracy': accuracy, 'loss': loss}
    wandb.log(metrics)
```

[See a full code example →](https://github.com/wandb/examples/tree/master/examples/pytorch/pytorch-cnn-fashion)

## 2. Sweep Config

Set up a YAML file to specify your training script, parameter ranges, search strategy, and stopping criteria. W&B will pass these parameters and their values as command line arguments to your training script, and we'll automatically parse them with the config object you set up in [Step 1](quickstart.md#set-up-your-python-training-script).

Here are some resources on configuring sweeps:

1. [Example YAML files](https://github.com/wandb/examples/tree/master/examples/keras/keras-cnn-fashion) : an example script and several different YAML files
2. [Configuration](configuration.md): full specs to set up your sweep config
3. [Jupyter Notebook](python-api.md): set up your sweep config with a Python dictionary instead of a YAML file
4. [Generate config from UI](existing-project.md): take an existing W&B project and generate a config file
5. [Feed in prior runs](https://docs.wandb.com/sweeps/existing-project#seed-a-new-sweep-with-existing-runs): take previous runs and add them to a new sweep

Here's an example sweep config YAML file called `sweep.yaml`:

{% tabs %}
{% tab title="sweep.yaml" %}
```yaml
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
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
If you specify a metric to optimize, make sure you're logging it. In this example, I have `val_loss` in my config file, so I have to log that exact metric name in my script:

`wandb.log({"val_loss": validation_loss})`
{% endhint %}

Under the hood, this example configuration will use the Bayes optimization method to choose sets of hyperparameter values with which to call your program. It will launch experiments with the following syntax:

```python
python train.py --learning_rate=0.005 --optimizer=adam
python train.py --learning_rate=0.03 --optimizer=sgd
```

{% hint style="info" %}
If you're using `argparse` in your script, we recommend that you use underscores in your variable names instead of hyphens.
{% endhint %}

## 3. Initialize a sweep

Our central server coordinates between all agents executing the sweep. Set up a sweep config file and run this command to get started:

```python
wandb sweep sweep.yaml
```

This command will print out a **sweep ID**, which includes the entity name and project name. Copy that to use in the next step!

## 4. Launch agent\(s\)

On each machine that you'd like to execute the sweep, start an agent with the sweep ID. You'll want to use the same sweep ID for all agents who are performing the same sweep.

In a shell on your own machine, run the wandb agent command which will ask the server for commands to run:

```python
wandb agent your-sweep-id
```

You can run `wandb agent` on multiple machines or in multiple processes on the same machine, and each agent will poll the central W&B sweep server for the next set of hyperparameters to run.

## 5. Visualize results

Open your project to see your live results in the sweep dashboard.

[Example dashboard →](https://app.wandb.ai/carey/pytorch-cnn-fashion)

![](../../.gitbook/assets/image%20%2888%29%20%282%29%20%283%29%20%283%29%20%283%29%20%283%29%20%283%29%20%284%29.png)

## 6. Stop the agent

From the terminal, we can hit `Ctrl+c` to stop the run that the sweep agent is currently running. If we want to kill the agent, we need to hit `Ctrl+c` again after the run is stopped.

