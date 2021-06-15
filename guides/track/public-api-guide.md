---
description: >-
  Best practices and common use cases for our public API to export data and
  update existing runs
---

# Import & Export Data

Use the Public API to export or update data that you have saved to W&B. Before using this API, you'll want to log data from your script — check the [Quickstart](../../quickstart.md) for more details.

**Use Cases for the Public API**

* **Export Data**: Pull down a dataframe for custom analysis in a Jupyter Notebook. Once you have explored the data, you can sync your findings by creating a new analysis run and logging results, for example: `wandb.init(job_type="analysis")`
* **Update Existing Runs**: You can update the data logged in association with a W&B run. For example, you might want to update the config of a set of runs to include additional information, like the architecture or a hyperparameter that wasn't originally logged.

See the [Generated Reference Docs](https://docs.wandb.ai/ref/python/public-api) for details on available functions.

### Authentication

Authenticate your machine with your [API key](https://wandb.ai/authorize) in one of two ways:

1. Run `wandb login`  on the command line and paste in your API key.
2. Set the **WANDB\_API\_KEY** environment variable to your API key.

### Export Run Data

Download data from a finished or active run. Common usage includes downloading a dataframe for custom analysis in a Jupyter notebook, or using custom logic in an automated environment.

```python
import wandb
api = wandb.Api()
run = api.run("<entity>/<project>/<run_id>")
```

The most commonly used attributes of a run object are:

| Attribute | Meaning |
| :--- | :--- |
| run.config | A dictionary for model inputs, such as hyperparameters |
| run.history\(\) | A list of dictionaries meant to store values that change while the model is training such as loss.  The command wandb.log\(\) appends to this object. |
| run.summary | A dictionary of outputs. This can be scalars like accuracy and loss, or large files. By default, wandb.log\(\) sets the summary to the final value of a logged timeseries. This can also be set directly. |

You can also modify or update the data of past runs. By default a single instance of an api object will cache all network requests. If your use case requires real time information in a running script, call api.flush\(\) to get updated values.

### Sampling

The default history method samples the metrics to a fixed number of samples \(the default is 500, you can change this with the _samples_ argument\). If you want to export all of the data on a large run, you can use the run.scan\_history\(\) method. For more details see the [API Reference](https://docs.wandb.ai/ref/python/public-api).

### Querying Multiple Runs

{% tabs %}
{% tab title="Dataframes and CSVs" %}
This example script finds a project and outputs a CSV of runs with name, configs and summary stats.

```python
import wandb
api = wandb.Api()

# Change oreilly-class/cifar to <entity/project-name>
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
{% endtab %}

{% tab title="MongoDB Style" %}
The W&B API also provides a way for you to query across runs in a project with api.runs\(\). The most common use case is exporting runs data for custom analysis.  The query interface is the same as the one [MongoDB uses](https://docs.mongodb.com/manual/reference/operator/query).

```python
runs = api.runs("username/project", {"$or": [{"config.experiment_name": "foo"}, {"config.experiment_name": "bar"}]})
print("Found %i" % len(runs))
```
{% endtab %}
{% endtabs %}

Calling `api.runs(...)` returns a **Runs** object that is iterable and acts like a list. The object loads 50 runs at a time in sequence as required, you can change the number loaded per page with the **per\_page** keyword argument.

`api.runs(...)` also accepts an **order** keyword argument. The default order is `-created_at`, specify `+created_at` to get results in ascending order. You can also sort by config or summary values i.e. `summary.val_acc` or `config.experiment_name`

### Error Handling

If errors occur while talking to W&B servers a `wandb.CommError` will be raised. The original exception can be introspected via the **exc** attribute.

### Get the latest git commit through the API

In the UI, click on a run and then click the Overview tab on the run page to see the latest git commit. It's also in the file `wandb-metadata.json` . Using the public API, you can get the git hash with **run.commit**.

## Common Questions

### Export data to visualize in matplotlib or seaborn

Check out our [API examples](https://docs.wandb.ai/library/public-api-guide#public-api-examples) for some common export patterns. You can also click the download button on a custom plot or on the expanded runs table to download a CSV from your browser.

### Get the random run ID and run name from your script

After calling `wandb.init()`  you can access the random run ID or the human readable run name from your script like this:

* Unique run ID \(8 character hash\): `wandb.run.id`
* Random run name \(human readable\): `wandb.run.name`

 If you're thinking about ways to set useful identifiers for your runs, here's what we recommend:

* **Run ID**: leave it as the generated hash. This needs to be unique across runs in your project.
* **Run name**: This should be something short, readable, and preferably unique so that you can tell the difference between different lines on your charts.
* **Run notes**: This is a great place to put a quick description of what you're doing in your run. You can set this with `wandb.init(notes="your notes here")` 
* **Run tags**: Track things dynamically in run tags, and use filters in the UI to filter your table down to just the runs you care about. You can set tags from your script and then edit them in the UI, both in the runs table and the overview tab of the run page.

## Public API Examples

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

### Read specific metrics from a run

To pull specific metrics from a run, use the `keys` argument. The default number of samples when using `run.history()` is 500. Logged steps that do not include a specific metric will appear in the output dataframe as `NaN`. The `keys` argument will cause the api to sample steps that include the listed metric keys more frequently.

```python
import wandb
api = wandb.Api()

run = api.run("<entity>/<project>/<run_id>")
if run.state == "finished":
   for i, row in run.history(keys=["accuracy"]).iterrows():
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

### Update metrics for a run, after the run has finished

This example sets the accuracy of a previous run to 0.9. It also modifies the accuracy histogram of a previous run to be the histogram of numpy\_array.

```python
import wandb
api = wandb.Api()

run = api.run("<entity>/<project>/<run_id>")
run.summary["accuracy"] = 0.9
run.summary["accuracy_histogram"] = wandb.Histogram(numpy_array)
run.summary.update()
```

### Rename a metric in a run, after the run has finished

This example renames a summary column in your tables.

```python
import wandb

api = wandb.Api()
run = api.run("<entity>/<project>/<run_id>")
run.summary['new_name'] = run.summary['old_name']
del run.summary['old_name']
run.summary.update()
```

{% hint style="warning" %}
Renaming a column only applies to tables. Charts will still refer to metrics by their original names.
{% endhint %}

### Update config for an existing run

This examples updates one of your configuration settings.

```python
import wandb
api = wandb.Api()
run = api.run("<entity>/<project>/<run_id>")
run.config["key"] = updated_value
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

### Upload files to a finished run

The code snippet below uploads a selected file to a finished run.

```python
import wandb
api = wandb.Api()
run = api.run("entity/project/run_id")
run.upload_file("file_name.extension")
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

### Delete all files with a given extension from a run

```python
import wandb

api = wandb.Api()
run = api.run("<entity>/<project>/<run_id>")

files = run.files()
for file in files:
	if file.name.endswith(".png"):
		file.delete()
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
summary.update({"key": val})
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

