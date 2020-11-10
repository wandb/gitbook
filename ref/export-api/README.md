# Data Import/Export API

Export a dataframe for custom analysis, or asynchronously add data to a completed run. For more details see the [API Reference](../api.md).

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

The default history method samples the metrics to a fixed number of samples \(the default is 500, you can change this with the _samples_ argument\). If you want to export all of the data on a large run, you can use the run.scan\_history\(\) method. For more details see the [API Reference](../api.md).

### Querying Multiple Runs

{% tabs %}
{% tab title="MongoDB Style" %}
The W&B API also provides a way for you to query across runs in a project with api.runs\(\). The most common use case is exporting runs data for custom analysis.  The query interface is the same as the one [MongoDB uses](https://docs.mongodb.com/manual/reference/operator/query).

```python
runs = api.runs("username/project", {"$or": [{"config.experiment_name": "foo"}, {"config.experiment_name": "bar"}]})
print("Found %i" % len(runs))
```
{% endtab %}

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
{% endtabs %}

Calling `api.runs(...)` returns a **Runs** object that is iterable and acts like a list. The object loads 50 runs at a time in sequence as required, you can change the number loaded per page with the **per\_page** keyword argument.

`api.runs(...)` also accepts an **order** keyword argument. The default order is `-created_at`, specify `+created_at` to get results in ascending order. You can also sort by config or summary values i.e. `summary.val_acc` or `config.experiment_name`

### Error Handling

If errors occur while talking to W&B servers a `wandb.CommError` will be raised. The original exception can be introspected via the **exc** attribute.

### Get the latest git commit through the API

In the UI, click on a run and then click the Overview tab on the run page to see the latest git commit. It's also in the file `wandb-metadata.json` . Using the public API, you can get the git hash with **run.commit**.

## Common Questions

### Export data to visualize in matplotlib or seaborn

Check out our [API examples](examples.md) for some common export patterns. You can also click the download button on a custom plot or on the expanded runs table to download a CSV from your browser.

### Get the random run ID and run name from your script

After calling `wandb.init()`  you can access the random run ID or the human readable run name from your script like this:

* Unique run ID \(8 character hash\): `wandb.run.id`
* Random run name \(human readable\): `wandb.run.name`

 If you're thinking about ways to set useful identifiers for your runs, here's what we recommend:

* **Run ID**: leave it as the generated hash. This needs to be unique across runs in your project.
* **Run name**: This should be something short, readable, and preferably unique so that you can tell the difference between different lines on your charts.
* **Run notes**: This is a great place to put a quick description of what you're doing in your run. You can set this with `wandb.init(notes="your notes here")` 
* **Run tags**: Track things dynamically in run tags, and use filters in the UI to filter your table down to just the runs you care about. You can set tags from your script and then edit them in the UI, both in the runs table and the overview tab of the run page.

