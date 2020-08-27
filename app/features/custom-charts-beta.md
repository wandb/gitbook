---
description: Custom visualizations and custom panels using queries
---

# Custom Charts \[Beta\]

Even the most powerful visualization tool can't encompass all the features you might need to get exactly the right chart. This new feature allows you to fetch any of the data you've logged in a run, and customize how you visualize that in the UI.

### How it works

* **Custom queries**: Pull in your data with a GraphQL query in the UI.
* **Custom charts**: Visualize your data with Vega.

![](../../.gitbook/assets/pr-roc.png)

## Logging data

### **Config and summary data**

* **config**: Settings at the beginning of the run, your independent variables
* **summary**: Results at the end of your training. By default, if you track a metric in history, we set the summary to the final value of that history key.
* **table**: If you need to log a list of multiple values, use a `wandb.Table()` to save that data and then query it in your custom panel. The maximum size of these tables is 10,000 rows.

### **Log a custom table**

A particularly useful format for custom visualizations is `wandb.Table()`, as this lets you log data as a custom 2D array. You can log these tables at multiple time steps throughout your experiment:

```python
# Logging a custom table of data
my_custom_data = [[x1, y1, z1], [x2, y2, z2]]
wandb.log({“custom_data_table”: wandb.Table(data=my_custom_data,
                                columns = ["x", "y", "z"])})
```

## Custom queries

## Custom charts

## Frequently asked questions

### Gotchas

* Not seeing the data you're expecting in the query as you're editing your chart? It might be because the column you're looking for is not logged in the runs you have selected. Save your chart and go back out to the runs table, and select the runs you'd like to visualize with the **eye** icon.

### Common use cases

* Overlay histograms of data from two different models
* Customize bar plots

