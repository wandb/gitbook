---
description: Common questions about logging data with W&B
---

# Logging FAQs

### How can I organize my logged charts and media in the W\&B UI?

We treat `/` as a separator for organizing logged panels in the W\&B UI. By default, the component of the logged item's name before a `/` is used to define a group of panel called a "Panel Section".

```python
wandb.log({'val/loss': 1.1, 'val/acc': 0.3})  # charts in val/ Panel Section
wandb.log({'train/loss': 0.1, 'train/acc': 0.94})  # charts in train/ Panel Section
```

In the [Workspace](../../../ref/app/pages/workspaces.md) settings, you can change whether panels are grouped by just the first component or by all components separated by `/`.

### **How can I compare images or media across epochs or steps?**

Each time you log images from a step, we save them to show in the UI. Expand the image panel, and use the step slider to look at images from different steps. This makes it easy to compare how a model's output changes during training.

### What if I want to log some metrics on batches and some metrics only on epochs?

If you'd like to log certain metrics in every batch and standardize plots, you can log x axis values that you want to plot with your metrics. Then in the custom plots, click edit and select a custom x-axis.

```python
wandb.log({'batch': batch_idx, 'loss': 0.3})
wandb.log({'epoch': epoch, 'val_acc': 0.94}) 
```

### How do I log a list of values?

Logging lists directly is not supported. Instead, list-like collections of numerical data are converted to [histograms](../../../ref/python/data-types/histogram.md). To log all of the entries in a list, give a name to each entry in the list and use those names as keys in a dictionary, as below.

{% tabs %}
{% tab title="Using a dictionary" %}
```python
wandb.log({f"losses/loss-{ii}": loss for ii, loss in enumerate(losses)})
```
{% endtab %}

{% tab title="As a histogram" %}
```python
wandb.log({"losses": wandb.Histogram(losses)})  # converts losses to a histogram
```
{% endtab %}
{% endtabs %}

### How do I plot multiple lines on a plot with a legend?

Multi-line custom chart can be created by using `wandb.plot.lineseries()`. You'll need to navigate to the [project page](https://docs.wandb.ai/ref/app/pages/project-page) to see the line chart. To add a legend to the plot, pass the keys argument within `wandb.plot.lineseries()`. For example:

```python
wandb.log({"my_plot" : wandb.plot.lineseries(
                         xs = x_data, 
                         ys = y_data, 
                         keys = ["metric_A", "metric_B"])}] 
```

You can find more information about Multi-line plots [here](https://docs.wandb.ai/guides/track/log/plots#basic-charts) under the Multi-line tab.

### How do I use custom x-axes?

By default, we increment the global step every time you call `wandb.log`. If you'd like, you can log your own monotonically increasing step and then select it as a custom x-axis on your graphs.

For example, if you have training and validation steps you'd like to align, pass us your own step counter: `wandb.log({"acc": 0.1, "global_step": 1})`. Then in the graphs choose `"global_step"` as the x-axis.

`wandb.log({"acc": 0.1, "batch": 10})` would enable you to choose `"batch"` as an x-axis in addition to the default step axis.

### Why is nothing showing up in my graphs?

If you're seeing "No visualization data logged yet" that means that we haven't gotten the first `wandb.log` call from your script yet. This could be because your run takes a long time to finish a step. If you're logging at the end of each epoch, you could log a few times per epoch to see data stream in more quickly.

### **Why is the same metric appearing more than once?**

If you're logging different types of data under the same key, we have to split them out in our database. This means you'll see multiple entries of the same metric name in a dropdown in the UI. The types we group by are `number`, `string`, `bool`, `other` (mostly arrays), and any `wandb` data type (`Histogram`, `Image`, etc). Send only one type to each key to avoid this behavior.

We store metrics in a case-insensitive fashion, so make sure you don't have two metrics with the same name like `"My-Metric"` and `"my-metric"`.

### How can I access the data logged to my runs directly and programmatically?

The history object is used to track metrics logged by `wandb.log`. Using [our API](../public-api-guide.md), you can access the history object via `run.history()`.

```python
api = wandb.Api()
run = api.run("username/project/run_id")
print(run.history())
```

### What happens when I log millions of steps to W\&B? How is that rendered in the browser?

The more points you send us, the longer it will take to load your graphs in the UI. If you have more than 1000 points on a line, we sample down to 1000 points on the backend before we send your browser the data. This sampling is nondeterministic, so if you refresh the page you'll see a different set of sampled points.

If you'd like all the original data, you can use our [data API](https://docs.wandb.com/library/api) to pull down unsampled data.

**Guidelines**

We recommend that you try to log less than 10,000 points per metric. If you log more than 1 million points in a line, it will take us while to load the page. For more on strategies for reducing logging footprint without sacrificing accuracy, check out [this Colab](http://wandb.me/log-hf-colab). If you have more than 500 columns of config and summary metrics, we'll only show 500 in the table.

### What if I want to integrate W\&B into my project, but I don't want to upload any images or media?

W\&B can be used even for projects that only log scalars — you specify any files or data you'd like to upload explicitly. Here's [a quick example in PyTorch](http://wandb.me/pytorch-colab) that does not log images.
