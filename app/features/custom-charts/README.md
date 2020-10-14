---
description: Custom visualizations and custom panels using queries
---

# Custom Charts

Use **Custom Charts** to create charts that aren't possible right now in the default UI. Log arbitrary tables of data and visualize them exactly how you want. Control details of fonts, colors, and tooltips with the power of [Vega](https://vega.github.io/vega/).

* **What's possible**: Read the[ launch announcement →](https://wandb.ai/wandb/posts/reports/Announcing-the-W-B-Machine-Learning-Visualization-IDE--VmlldzoyNjk3Nzg)
* **Code**: Try a live example in a[ hosted notebook →](https://tiny.cc/custom-charts)
* **Video**: Watch a quick [walkthrough video →](https://www.youtube.com/watch?v=3-N9OV6bkSM)
* **Example**: Quick Keras and Sklearn [demo notebook →](https://colab.research.google.com/drive/1g-gNGokPWM2Qbc8p1Gofud0_5AoZdoSD?usp=sharing)

Contact Carey \(c@wandb.com\) with questions or suggestions

![Supported charts from vega.github.io/vega](../../../.gitbook/assets/screen-shot-2020-09-09-at-2.18.17-pm.png)

### How it works

1. **Log data**: From your script, log [config](../../../library/config.md) and summary data as you normally would when running with W&B. To visualize a list of multiple values logged at one specific time, use a custom`wandb.Table`
2. **Customize the chart**: Pull in any of this logged data with a [GraphQL](https://graphql.org/) query. Visualize the results of your query with [Vega](https://vega.github.io/vega/), a powerful visualization grammar.
3. **Log the chart**: Call your own preset from your script with `wandb.plot_table()` or use one of our builtins.

{% page-ref page="walkthrough.md" %}

![](../../../.gitbook/assets/pr-roc.png)

## Log data

Here are the data types you can log from your script and use in a custom chart:

* **Config**: Initial settings of your experiment \(your independent variables\). This includes any named fields you've logged as keys to `wandb.config` at the start of your training \(e.g. `wandb.config.learning_rate = 0.0001)`
* **Summary**: Single values logged during training \(your results or dependent variables\), e.g. `wandb.log({"val_acc" : 0.8})`. If you write to this key multiple times during training via `wandb.log()`, the summary is set to the final value of that key.
* **History**: The full timeseries of the logged scalar is available to the query via the `history` field
* **summaryTable**: If you need to log a list of multiple values, use a `wandb.Table()` to save that data, then query it in your custom panel.

### **How to log a custom table**

Use `wandb.Table()` to log your data as a 2D array. Typically each row of this table represents one data point, and each column denotes the relevant fields/dimensions for each data point which you'd like to plot. As you configure a custom panel, the whole table will be accessible via the named key passed to `wandb.log()`\("custom\_data\_table" below\), and the individual fields will be accessible via the column names \("x", "y", and "z"\). You can log tables at multiple time steps throughout your experiment. The maximum size of each table is 10,000 rows. 

[Try it in a Google Colab →](https://tiny.cc/custom-charts)

```python
# Logging a custom table of data
my_custom_data = [[x1, y1, z1], [x2, y2, z2]]
wandb.log({“custom_data_table”: wandb.Table(data=my_custom_data,
                                columns = ["x", "y", "z"])})
```

## Customize the chart

Add a new custom chart to get started, then edit the query to select data from your visible runs. The query uses [GraphQL](https://graphql.org/) to fetch data from the config, summary, and history fields in your runs.

![Add a new custom chart, then edit the query](../../../.gitbook/assets/2020-08-28-06.42.40.gif)

### Custom visualizations

Select a **Chart** in the upper right corner to start with a default preset. Next, pick **Chart fields** to map the data you're pulling in from the query to the corresponding fields in your chart. Here's an example of selecting a metric to get from the query, then mapping that into the bar chart fields below.

![Creating a custom bar chart showing accuracy across runs in a project](../../../.gitbook/assets/demo-make-a-custom-chart-bar-chart.gif)

### How to edit Vega

Click **Edit** at the top of the panel to go into [Vega](https://vega.github.io/vega/) edit mode. Here you can define a [Vega specification](https://vega.github.io/vega/docs/specification/) that creates an interactive chart in the UI.  You can change any aspect of the chart, from the visual style \(e.g. change the title, pick a different color scheme, show curves as a series of points instead of as connected lines\) to the data itself \(use a Vega transform to bin an array of values into a histogram, etc.\). The panel preview will update interactively, so you can see the effect of your changes as you edit the Vega spec or query. The [Vega documentation and tutorials ](https://vega.github.io/vega/)are an excellent source of inspiration.

**Field references**

To pull data into your chart from W&B, add template strings of the form `"${field:<field-name>}"` anywhere in your Vega spec. This will create a dropdown in the **Chart Fields** area on the right side, which users can use to select a query result column to map into Vega.

To set a default value for a field, use this syntax: `"${field:<field-name>:<placeholder text>}"`

### Saving Charts

Apply any changes to a specific visualization panel, or save the Vega spec to reuse elsewhere in your project. To save the reusable chart definition, click **Save as** at the top of the Vega editor and give your preset a name.

## Log charts from the script

### Builtin presets

{% tabs %}
{% tab title="Line plot" %}
Log a custom line plot—a list of connected and ordered points \(x,y\) on arbitrary axes x and y.

```python
data = [[x, y] for (x, y) in zip(x_values, y_values)]
table = wandb.Table(data=data, columns = ["x", "y"])
wandb.log({"my_custom_plot_id" : wandb.plot.line(table, "x", "y", title="Custom Y vs X Line Plot")
```

You can use this to log curves on any two dimensions. Note that if you're plotting two lists of values against each other, the number of values in the lists must match exactly \(i.e. each point must have an x and a y\).

![](../../../.gitbook/assets/line-plot.png)

[See the interactive report →](https://wandb.ai/wandb/plots/reports/Custom-Line-Plots--VmlldzoyNjk5NTA)

[Try the hosted notebook →](https://tiny.cc/custom-charts)
{% endtab %}

{% tab title="Scatter plot" %}
Log a custom scatter plot—a list of points \(x, y\) on a pair of arbitrary axes x and y.

```python
data = [[x, y] for (x, y) in zip(x_values, y_values)]
table = wandb.Table(data=data, columns = ["x", "y"])
wandb.log({"my_custom_plot_id" : wandb.plot.scatter(table, "x", "y", title="Custom Y vs X Scatter Plot")
```

You can use this to log scatter points on any two dimensions. Note that if you're plotting two lists of values against each other, the number of values in the lists must match exactly \(i.e. each point must have an x and a y\).

![](../../../.gitbook/assets/demo-scatter-plot.png)



[See the interactive report →](https://wandb.ai/wandb/plots/reports/Custom-Line-Plots--VmlldzoyNjk5NTA)

[Try the hosted notebook →](https://tiny.cc/custom-charts)
{% endtab %}

{% tab title="Bar chart" %}
Log a custom bar chart—a list of labeled values as bars—natively in a few lines:

```python
data = [[label, val] for (label, val) in zip(labels, values)]
table = wandb.Table(data=data, columns = ["label", "value"])
wandb.log({"my_bar_chart_id" : wandb.plot.bar(table, "label", "value", title="Custom Bar Chart")
```

You can use this to log arbitrary bar charts. Note that the number of labels and values in the lists must match exactly \(i.e. each data point must have both\).

![](../../../.gitbook/assets/image%20%2896%29.png)

[See the interactive report →](https://wandb.ai/wandb/plots/reports/Custom-Line-Plots--VmlldzoyNjk5NTA)

[Try the hosted notebook →](https://tiny.cc/custom-charts)
{% endtab %}

{% tab title="Histogram" %}




[See the interactive report →](https://wandb.ai/wandb/plots/reports/Custom-Line-Plots--VmlldzoyNjk5NTA)

[Try the hosted notebook →](https://tiny.cc/custom-charts)
{% endtab %}
{% endtabs %}

**wandb.plot.line\(\)**



\*\*\*\*

## Frequently asked questions

### Coming soon

* **Polling**: Auto-refresh of data in the chart
* **Sampling**: Dynamically adjust the total number of points loaded into the panel for efficiency

### Gotchas

* Not seeing the data you're expecting in the query as you're editing your chart? It might be because the column you're looking for is not logged in the runs you have selected. Save your chart and go back out to the runs table, and select the runs you'd like to visualize with the **eye** icon.

### Common use cases

* Customize bar plots with error bars
* Show model validation metrics which require custom x-y coordinates \(like precision-recall curves\)
* Overlay data distributions from two different models/experiments as histograms
* Show changes in a metric via snapshots at multiple points during training
* Create a unique visualization not yet available in W&B \(and hopefully share it with the world\) 

