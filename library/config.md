---
description: Dictionary-like object to save your experiment configuration
---

# wandb.config

## Overview

Use `wandb.config` to save your training config:  hyperparameters, input settings like dataset name or model type, and any other independent variables for your experiments. This is useful for analyzing your experiments and reproducing your work in the future. You'll be able to group by config values in the web interface, comparing the settings of different runs and seeing how these affect the output. Note that output metrics or dependent variables \(like loss and accuracy\) should be saved with `wandb.log`instead. Config should be set just once at the beginning of your training experiment.

You can send us a nested dictionary in config, and we'll flatten the names using dots in our backend. We recommend that you avoid using dots in your config variable names, and use a dash or underscore instead. Once you've created your wandb config dictionary, if your script accesses wandb.config keys below the root, use `[ ]` syntax instead of `.` syntax.

## Simple Example

```python
wandb.config.epochs = 4
wandb.config.batch_size = 32
# you can also initialize your run with a config
wandb.init(config={"epochs": 4})
```

## Efficient Initialization

You can treat `wandb.config` as a dictionary, updating multiple values at a time.

```python
wandb.init(config={"epochs": 4, "batch_size": 32})
# or
wandb.config.update({"epochs": 4, "batch_size": 32})
```

## TensorFlow Flags

You can pass TensorFlow flags into the config object.

```python
wandb.init()
wandb.config.epochs = 4

flags = tf.app.flags
flags.DEFINE_string('data_dir', '/tmp/data')
flags.DEFINE_integer('batch_size', 128, 'Batch size.')
wandb.config.update(flags.FLAGS)  # adds all of the tensorflow flags as config
```

## Argparse Flags

You can pass in the arguments dictionary from argparse. This is convenient for quickly testing different hyperparameter values from the command line.

```python
wandb.init()
wandb.config.epochs = 4

parser = argparse.ArgumentParser()
parser.add_argument('-b', '--batch-size', type=int, default=8, metavar='N',
                     help='input batch size for training (default: 8)')
args = parser.parse_args()
wandb.config.update(args) # adds all of the arguments as config variables
```

## File-Based Configs

You can create a file called **config-defaults.yaml,** __and it will automatically be loaded into `wandb.config`

```yaml
# sample config defaults file
epochs:
  desc: Number of epochs to train over
  value: 100
batch_size:
  desc: Size of each mini-batch
  value: 32
```

You can tell wandb to load different config files with the command line argument `--configs special-configs.yaml` which will load parameters from the file special-configs.yaml.

One example use case: you have a YAML file with some metadata for the run, and then a dictionary of hyperparameters in your Python script. You can save both in the nested config object:

```python
hyperparameter_defaults = dict(
    dropout = 0.5,
    batch_size = 100,
    learning_rate = 0.001,
    )

config_dictionary = dict(
    yaml=my_yaml_file,
    params=hyperparameter_defaults,
    )
    
wandb.init(config=config_dictionary)
```

## Dataset Identifier 

You can add a unique identifier \(like a hash or other identifier\) in your run's configuration for your dataset by tracking it as input to your experiment using `wandb.config` 

```yaml
wandb.config.update({'dataset':'ab131'}) 
```

### Update Config Files

You can use the public API to update your config file 

```yaml
import wandb
api = wandb.Api()
run = api.run("username/project/run_id")
run.config["foo"] = 32
run.update()
```

### Key Val Pairs 

ou can log any key val pairs into wandb.config.  They will be different for every type of model you are training.  i.e. `wandb.config.update({"my_param": 10, "learning_rate": 0.3, "model_architecture": "B"})`  


