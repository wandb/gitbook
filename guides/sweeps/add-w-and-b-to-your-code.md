# Add W\&B to your code

There are numerous ways to add the Weights & Biases Python SDK to your script or Jupyter Notebook. Outlined below is a "best practice" example of how to integrate the W\&B Python SDK into your own code.

### Original training script

Suppose you have the following code in a Jupyter Notebook cell or Python script. We define a function called `main` that mimics a typical training loop; for each epoch, the accuracy and loss is computed on the training and validation data sets. The values are randomly generated for the purpose of this example.

We defined a dictionary called `config` where we store hyperparameters values (line 15). At the end of the cell, we call the `main` function to execute the mock training code.

{% code lineNumbers="true" %}
```python
#train.py
import random
import numpy as np

def train_one_epoch(epoch, lr, bs): 
  acc = 0.25 + ((epoch/30) +  (random.random()/10))
  loss = 0.2 + (1 - ((epoch-1)/10 +  random.random()/5))
  return acc, loss

def evaluate_one_epoch(epoch): 
  acc = 0.1 + ((epoch/20) +  (random.random()/10))
  loss = 0.25 + (1 - ((epoch-1)/10 +  random.random()/6))
  return acc, loss
  
config = {
    'lr' : 0.0001,
    'bs' : 16,
    'epochs': 5
}

def main():
    # Note that we define values from `wandb.config` instead of 
    # defining hard values
    lr = config['lr']
    bs = config['bs']
    epochs = config['epochs']

    for epoch in np.arange(1, epochs):
      train_acc, train_loss = train_one_epoch(epoch, lr, bs)
      val_acc, val_loss = evaluate_one_epoch(epoch)
      
      print('epoch: ', epoch)
      print('training accuracy:', train_acc,'training loss:', train_loss)
      print('validation accuracy:', val_acc,'training loss:', val_loss)

# Call the main function.       
main()
```
{% endcode %}

### Training script with W\&B Python SDK&#x20;

The following code examples demonstrate how to add the W\&B Python SDK into your code. If you start W\&B Sweep jobs in the CLI, you will want to explore the CLI tab. If you start W\&B Sweep jobs within a Jupyter notebook or Python script, explore the Python SDK tab.&#x20;

{% tabs %}
{% tab title="Python script or Jupyter Notebook" %}
To create a W\&B Sweep, we added the following to the code example:

1. Line 1: Import the Wights & Biases Python SDK.
2. Line 6: Create a dictionary object where the key-value pairs define the sweep configuration. In the proceeding example, the batch size (`batch_size`), epochs (`epochs`), and the learning rate (`lr`) hyperparameters are varied during each sweep. For more information on how to create a sweep configuration, see [Define sweep configuration](https://docs.wandb.ai/guides/sweeps/define-sweep-configuration).&#x20;
3. Line 19: Pass the sweep configuration dictionary to [`wandb.sweep`](https://docs.wandb.ai/ref/python/sweep). This initializes the sweep. This returns a sweep ID (`sweep_id`). For more information on how to initialize sweeps, see [Initialize sweeps](https://docs.wandb.ai/guides/sweeps/initialize-sweeps).&#x20;
4. Line 33: Use the [`wandb.init()`](https://docs.wandb.ai/ref/python/init) API to generate a background process to sync and log data as a [W\&B Run](https://docs.wandb.ai/ref/python/run).&#x20;
5. Line 37-39: (Optional) define values from `wandb.config` instead of defining hard coding values.
6. Line 45: Log the metric we want to optimize with [`wandb.log`](https://docs.wandb.ai/ref/python/log). You must log the metric defined in your configuration. Within the configuration dictionary (`sweep_configuration` in this example) we defined the sweep to maximize the `val_acc` value).
7. Line 54: Start the sweep with the [`wandb.agent`](https://docs.wandb.ai/ref/python/agent) API call. Provide the sweep ID (line 19), the name of the function the sweep will execute (`function=main`), and set the maximum number of runs to try to four (`count=4`). For more information on how to start W\&B Sweep, see [Start sweep agents](https://docs.wandb.ai/guides/sweeps/start-sweep-agents).&#x20;

{% code lineNumbers="true" %}
```python
import wandb
import numpy as np 
import random

# Define sweep config
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

# Initialize sweep by passing in config. (Optional) Provide a name of the project.
sweep_id = wandb.sweep(sweep=sweep_configuration, project='my-first-sweep')

# Define training function that takes in hyperparameter values from `wandb.config` and uses them to train a model and return metric
def train_one_epoch(epoch, lr, bs): 
  acc = 0.25 + ((epoch/30) +  (random.random()/10))
  loss = 0.2 + (1 - ((epoch-1)/10 +  random.random()/5))
  return acc, loss

def evaluate_one_epoch(epoch): 
  acc = 0.1 + ((epoch/20) +  (random.random()/10))
  loss = 0.25 + (1 - ((epoch-1)/10 +  random.random()/6))
  return acc, loss

def main():
    run = wandb.init()

    # note that we define values from `wandb.config` instead 
    # of defining hard values
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

# Start sweep job.
wandb.agent(sweep_id, function=main, count=4)
```
{% endcode %}
{% endtab %}

{% tab title="CLI" %}
To create a W\&B Sweep, we first create a YAML configuration file. The configuration file contains he hyperparameters we want the sweep to explore.  In the proceeding example, the batch size (`batch_size`), epochs (`epochs`), and the learning rate (`lr`) hyperparameters are varied during each sweep.

```yaml
# config.yaml
program: train.py
method: random
name: sweep
metric:
  goal: maximize
  name: val_acc
parameters:
  batch_size: 
    values: [16,32,64]
  lr:
    min: 0.0001
    max: 0.1
  epochs:
    values: [5, 10, 15]
```

For more information on how to create a W\&B Sweep configuration, see [Define sweep configuration](https://docs.wandb.ai/guides/sweeps/define-sweep-configuration).&#x20;

Note that you must provide the name of your Python script for the `program` key in your YAML file.

Next, we add the following to the code example:

1. Line 1-2: Import the Wights & Biases Python SDK (`wandb`) and PyYAML (`yaml`). PyYAML is used to read in our YAML configuration file.
2. Line 18: Read in the configuration file.
3. Line 21: Use the [`wandb.init()`](https://docs.wandb.ai/ref/python/init) API to generate a background process to sync and log data as a [W\&B Run](https://docs.wandb.ai/ref/python/run). We pass the config object to the config parameter.&#x20;
4. Line 25 - 27: Define hyperparameter values from `wandb.config` instead of using hard coded values.
5. Line 33-39: Log the metric we want to optimize with [`wandb.log`](https://docs.wandb.ai/ref/python/log). You must log the metric defined in your configuration. Within the configuration dictionary (`sweep_configuration` in this example) we defined the sweep to maximize the `val_acc` value.

{% code lineNumbers="true" %}
```python
import wandb
import yaml
import random
import numpy as np

def train_one_epoch(epoch, lr, bs): 
  acc = 0.25 + ((epoch/30) +  (random.random()/10))
  loss = 0.2 + (1 - ((epoch-1)/10 +  random.random()/5))
  return acc, loss

def evaluate_one_epoch(epoch): 
  acc = 0.1 + ((epoch/20) +  (random.random()/10))
  loss = 0.25 + (1 - ((epoch-1)/10 +  random.random()/6))
  return acc, loss  

def main():
    # Set up your default hyperparameters
    with open('./config.yaml') as file:
        config = yaml.load(file, Loader=yaml.FullLoader)
    
    run = wandb.init(config=config)

    # Note that we define values from `wandb.config` instead of 
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

# Call the main function.       
main()
```
{% endcode %}

Navigate to your CLI. Within your CLI, set a maximum number of runs the sweep agent should try. This is step optional. In the following example we set the maximum number to five.

```bash
NUM=5
```

Next, initialize the sweep with the [`wandb sweep`](https://docs.wandb.ai/ref/cli/wandb-sweep) command. Provide the name of the YAML file. Optionally provide the name of the project for the project flag (`--project`):

```bash
wandb sweep --project sweep-demo-cli config.yaml
```

This returns a sweep ID. For more information on how to initialize sweeps, see [Initialize sweeps](https://docs.wandb.ai/guides/sweeps/initialize-sweeps).

Copy the sweep ID and replace `sweepID` in the proceeding code snippet to start the sweep job with the [`wandb agent`](https://docs.wandb.ai/ref/cli/wandb-agent) command:

```bash
wandb agent --count $NUM noahluna/sweep-demo-cli/sweepID
```

For more information on how to start sweep jobs, see [Start sweep jobs](https://docs.wandb.ai/guides/sweeps/start-sweep-job).&#x20;
{% endtab %}
{% endtabs %}
