---
description: Importing and logging data into W&B
---

# Import CSV

## Converting your CSV of Experiments into a W\&B Dashboard

![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)

{% embed url="https://drive.google.com/file/d/1PL4RSdopHEptDR5Gi0DEzECXuoW_5B0f/view?usp=sharing" %}
The below table becomes this Weights & Biases Dashboard after conversion
{% endembed %}

| Experiment   | Model Name       | Notes                                            | Tags          | Num Layers | Final Train Acc | Final Val Acc | Training Losses                       |
| ------------ | ---------------- | ------------------------------------------------ | ------------- | ---------- | --------------- | ------------- | ------------------------------------- |
| Experiment 1 | mnist-300-layers | Overfit way too much on training data            | \[latest]     | 300        | 0.99            | 0.90          | \[0.55, 0.45, 0.44, 0.42, 0.40, 0.39] |
| Experiment 2 | mnist-250-layers | Current best model                               | \[prod, best] | 250        | 0.95            | 0.96          | \[0.55, 0.45, 0.44, 0.42, 0.40, 0.39] |
| Experiment 3 | mnist-200-layers | Did worse than the baseline model. Need to debug | \[debug]      | 200        | 0.76            | 0.70          | \[0.55, 0.45, 0.44, 0.42, 0.40, 0.39] |
| ...          | ...              | ...                                              | ...           | ...        | ...             | ...           |                                       |
| Experiment N | mnist-X-layers   | NOTES                                            | ...           | ...        | ...             | ...           | \[..., ...]                           |

In some cases, you may have your experiment details in a CSV file. W\&B can take this CSV and easily convert it into an experiment run. That may include:

* A name for the experiment run
* Initial [notes](../../../ref/app/features/notes.md)
* [Tags](../../../ref/app/features/tags.md) to differentiate the experiments
* Configurations needed for your experiment (with the added benefit of being able to utilize our [Sweeps Hyperparameter Tuning](../../sweeps/))

You can log this data a [`wandb.init()`](../../../ref/python/init.md) command:

```python
run = wandb.init(project=PROJECT_NAME, name=run_name, tags=tags, notes=notes, config=config)
```

As an experiment runs, you'll want to log every instance of your metrics so they are available to view, query, and analyze with W\&B. This is done via a [`run.log()`](./) command:

```python
run.log({key: val})
```

Once an experiment reaches the end, you may want to log final summary metrics to define the outcome of the run. This can be done with W\&B using  `define_metric`, but in this example case, we will be directly adding the [summary](broken-reference) metrics to our run with [`run.summary.update()`](broken-reference):

```python
run.summary.update(summaries)
```

Below is the full example script that will turn the above table into a W\&B Dashboard:

```python
FILENAME = "experiments.csv"
loaded_experiment_df = pd.read_csv(FILENAME)

PROJECT_NAME = "Converted Experiments"

EXPERIMENT_NAME_COL = "Experiment"
NOTES_COL = "Notes"
TAGS_COL = "Tags"
CONFIG_COLS = ["Num Layers"]
SUMMARY_COLS = ["Final Train Acc", "Final Val Acc"]
METRIC_COLS = ["Training Losses"]
pyth
for i, row in loaded_experiment_df.iterrows():
    
    run_name = row[EXPERIMENT_NAME_COL]
    notes = row[NOTES_COL]
    tags = row[TAGS_COL]

    config = {}
    for config_col in CONFIG_COLS:
        config[config_col] = row[config_col]

    metrics = {}
    for metric_col in METRIC_COLS:
        metrics[metric_col] = row[metric_col]
    
    summaries = {}
    for summary_col in SUMMARY_COLS:
        summaries[summary_col] = row[summary_col]


    run = wandb.init(project=PROJECT_NAME, name=run_name,\
    tags=tags, notes=notes, config=config)

    for key, val in metrics.items():
        if isinstance(val, list):
            for _val in val:
                run.log({key: _val})
        else:
            run.log({key: val})
            
    run.summary.update(summaries)
    run.finish()
```

## Logging your CSV for Visualization and Reuse

![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)

{% embed url="https://drive.google.com/file/d/1jBG3M4VnaMgeclRzowYZEYvFxvwb9SXF/view?usp=sharing" %}

You can also convert CSVs into [interactive `wandb.Table` format](../../data-vis/tables-quickstart.md) fit for a W\&B Dashboard.

Using [Artifacts](broken-reference) we can also directly log our `.csv` file to W\&B for reuse across projects and experiments:

```python
# Read our CSV into a new DataFrame
new_iris_dataframe = pd.read_csv("iris_dataframe.csv")

# Convert the DataFrame into a W&B Table
# NOTE: Tables will have a row limit of 10000 but...
iris_table = wandb.Table(dataframe=new_iris_dataframe)

# Add the table to an Artifact to increase the row limit to 200000 and make it easier to reuse!
iris_table_artifact = wandb.Artifact("iris_artifact", type="dataset")
iris_table_artifact.add(iris_table, "iris_table")

# We will also log the raw csv file within an artifact to preserve our data
iris_table_artifact.add_file("iris_dataframe.csv")

# Start a W&B run to log data
run = wandb.init(project="Tables-Quickstart")

# Log the table to visualize with a run...
run.log({"iris": iris_table})

# and Log as an Artifact to increase the available row limit!
run.log_artifact(iris_table_artifact)

# Finish the run (useful in notebooks)
run.finish()
```
