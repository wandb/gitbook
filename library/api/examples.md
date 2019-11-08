---
description: Useful ways to use the wandb API.
---

# API Examples

## Read metrics from a run

This example outputs timestamp and accuracy saved with `wandb.log({"accuracy": acc})` for a run saved to `<entity>/<project>/<run_id>`.

```python
import wandb
api = wandb.Api()

run = api.run("<entity>/<project>/<run_id>")
if run.state == "finished":
   for i, row in run.history().iterrows():
       print(row["_timestamp"], row["accuracy"])
```

## Compare two runs

This will output the config parameters that are different between run1 and run2.

```python
import wandb
api = wandb.Api()

# replace with your <entity_name>/<project_name>/<run_id>
run1 = api.run("stacey/keras_finetune/d9u2iaok")
run2 = api.run("stacey/keras_finetune/7jdf890a")

import pandas as pd
df = pd.DataFrame([run1.config, run2.config]).transpose()

df.columns = [run1.name, run2.name]
print(df[df[run1.name] != df[run2.name]])
```

Outputs:

```text
              c_10_sgd_0.025_0.01_long_switch base_adam_4_conv_2fc
batch_size                                 32                   16
n_conv_layers                               5                    4
optimizer                             rmsprop                 adam
```

## Update metrics for a run \(after run finished\)

This example sets the accuracy of a previous run to 0.9. It also modifies the accuracy histogram of a previous run to be the histogram of numpy\_arry

```python
import wandb
api = wandb.Api()

run = api.run("<entity>/<project>/<run_id>")
run.summary["accuracy"] = 0.9
run.summary["accuracy_histogram"] = wandb.Histogram(numpy_array)
run.summary.update()
```

## Export metrics from a single run to a CSV file

This script finds all the metrics saved for a single run and saves them to a CSV.

```python
import wandb
api = wandb.Api()

# run is specified by <entity>/<project>/<run id>
run = api.run("oreilly-class/cifar/uxte44z7")

# save the metrics for the run to a csv file
metrics_dataframe = run.history()
metrics_dataframe.to_csv("metrics.csv")
```

## Export metrics from a large single run without sampling

The default history method samples the metrics to a fixed number of samples \(the default is 500, you can change this with the _samples_ argument\). If you want to export all of the data on a large run, you can use the run.scan\_history\(\) method. This script loads all of the loss metrics into a variable losses for a longer run.

```python
import wandb
api = wandb.Api()

run = api.run("l2k2/examples-numpy-boston/i0wt6xua")
history = run.scan_history()
losses = [row["Loss"] for row in history]
```

## Export metrics from all runs in a project to a CSV file

This script finds a project and outputs a CSV of runs with name, configs and summary stats.

```python
import wandb
api = wandb.Api()

# Change oreilly-class/cifar to <entity/project-name>
runs = api.runs("oreilly-class/cifar")
summary_list = [] 
config_list = [] 
name_list = [] 
for run in runs: 
    # run.summary are the output key/values like accuracy.  We call ._json_dict to omit large files 
    summary_list.append(run.summary._json_dict) 

    # run.config is the input metrics.  We remove special values that start with _.
    config_list.append({k:v for k,v in run.config.items() if not k.startswith('_')}) 

    # run.name is the name of the run.
    name_list.append(run.name)       

import pandas as pd 
summary_df = pd.DataFrame.from_records(summary_list) 
config_df = pd.DataFrame.from_records(config_list) 
name_df = pd.DataFrame({'name': name_list}) 
all_df = pd.concat([name_df, config_df,summary_df], axis=1)

all_df.to_csv("project.csv")
```

## Download a file from a run

This finds the file "model-best.h5" associated with my runwith run ID uxte44z7 in the cifar project and saves it locally.

```python
import wandb
api = wandb.Api()
run = api.run("oreilly-class/cifar/uxte44z7")
run.file("model-best.h5").download()
```

## Download all files from a run

This finds all files associated with run ID uxte44z7 and saves them locally.  \(Note: you can also accomplish this by running wandb restore &lt;RUN\_ID&gt; from the command line.\)

```python
import wandb
api = wandb.Api()
run = api.run("oreilly-class/cifar/uxte44z7")
for file in run.files():
    file.download()
```

