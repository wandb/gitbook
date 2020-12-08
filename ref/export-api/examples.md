---
description: >-
  Here are some common use cases for pulling down data from W&B using our Python
  API.
---

# Data Export API Examples

### Find the run path

To use the public API, you'll often need the **Run Path** which is `"<entity>/<project>/<run_id>"`  In the app, open a run and click on the **Overview** tab to see the run path for any run.

### Read metrics from a run

This example outputs timestamp and accuracy saved with `wandb.log({"accuracy": acc})` for a run saved to `<entity>/<project>/<run_id>`.

```python
import wandb
api = wandb.Api()

run = api.run("<entity>/<project>/<run_id>")
if run.state == "finished":
   for i, row in run.history().iterrows():
      print(row["_timestamp"], row["accuracy"])
```

### Compare two runs

This will output the config parameters that are different between run1 and run2.

```python
import wandb
api = wandb.Api()

# replace with your <entity_name>/<project_name>/<run_id>
run1 = api.run("<entity>/<project>/<run_id>")
run2 = api.run("<entity>/<project>/<run_id>")

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

### Update metrics for a run \(after run finished\)

This example sets the accuracy of a previous run to 0.9. It also modifies the accuracy histogram of a previous run to be the histogram of numpy\_array

```python
import wandb
api = wandb.Api()

run = api.run("<entity>/<project>/<run_id>")
run.summary["accuracy"] = 0.9
run.summary["accuracy_histogram"] = wandb.Histogram(numpy_array)
run.summary.update()
```

### Update config in a run

This examples updates one of your configuration settings

```python
import wandb
api = wandb.Api()
run = api.run("<entity>/<project>/<run_id>")
run.config["key"] = 10
run.update()
```

### Export metrics from a single run to a CSV file

This script finds all the metrics saved for a single run and saves them to a CSV.

```python
import wandb
api = wandb.Api()

# run is specified by <entity>/<project>/<run id>
run = api.run("<entity>/<project>/<run_id>")

# save the metrics for the run to a csv file
metrics_dataframe = run.history()
metrics_dataframe.to_csv("metrics.csv")
```

### Export metrics from a large single run without sampling

The default history method samples the metrics to a fixed number of samples \(the default is 500, you can change this with the _samples_ argument\). If you want to export all of the data on a large run, you can use the run.scan\_history\(\) method. This script loads all of the loss metrics into a variable losses for a longer run.

```python
import wandb
api = wandb.Api()

run = api.run("<entity>/<project>/<run_id>")
history = run.scan_history()
losses = [row["Loss"] for row in history]
```

### Export metrics from all runs in a project to a CSV file

This script finds a project and outputs a CSV of runs with name, configs and summary stats.

```python
import wandb
api = wandb.Api()

runs = api.runs("<entity>/<project>")
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

### Download a file from a run

This finds the file "model-best.h5" associated with with run ID uxte44z7 in the cifar project and saves it locally.

```python
import wandb
api = wandb.Api()
run = api.run("<entity>/<project>/<run_id>")
run.file("model-best.h5").download()
```

### Download all files from a run

This finds all files associated with run ID uxte44z7 and saves them locally.  \(Note: you can also accomplish this by running wandb restore &lt;RUN\_ID&gt; from the command line.\)

```python
import wandb
api = wandb.Api()
run = api.run("<entity>/<project>/<run_id>")
for file in run.files():
    file.download()
```

### Download the best model file

```python
import wandb
api = wandb.Api()
sweep = api.sweep("<entity>/<project>/<sweep_id>")
runs = sorted(sweep.runs, key=lambda run: run.summary.get("val_acc", 0), reverse=True)
val_acc = runs[0].summary.get("val_acc", 0)
print(f"Best run {runs[0].name} with {val_acc}% validation accuracy")
runs[0].file("model-best.h5").download(replace=True)
print("Best model saved to model-best.h5")
```

### Get runs from a specific sweep

```python
import wandb
api = wandb.Api()
sweep = api.sweep("<entity>/<project>/<sweep_id>")
print(sweep.runs)
```

### Download system metrics data

This gives you a dataframe with all your system metrics for a run.

```python
import wandb
api = wandb.Api()
run = api.run("<entity>/<project>/<run_id>")
system_metrics = run.history(stream = 'events')
```

### Update summary metrics

You can pass your dictionary to update summary metrics.

```python
summary.update({“key”: val})
```

### Get the command that ran the run

Each run captures the command that launched it on the run overview page. To pull this command down from the API, you can run:

```python
api = wandb.Api()
run = api.run("username/project/run_id")
meta = json.load(run.file("wandb-metadata.json").download())
program = ["python"] + [meta["program"]] + meta["args"]
```

### Get paginated data from history

If metrics are being fetched slowly on our backend or API requests are timing out, you can try lowering the page size in `scan_history` so that individual requests don't time out. The default page size is 1000, so you can experiment with different sizes to see what works best:

```python
api = wandb.Api()
run = api.run("username/project/run_id")
run.scan_history(keys=sorted(cols), page_size=100)
```



