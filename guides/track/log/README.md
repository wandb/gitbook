---
description: Keep track of metrics, videos, custom plots, and more
---

# Log Data with wandb.log

Call `wandb.log(dict)` to log a dictionary of metrics, media, or custom objects to a step. Each time you log, we increment the step by default, so you can see how your models and data evolve over time.

### Example Usage

```python
wandb.log({"loss": 0.314, "epoch": 5,
           "inputs": wandb.Image(inputs),
           "logits": wandb.Histogram(ouputs),
           "captions": wandb.Html(captions)})
```

### **Common Workflows**

1. **Compare the best accuracy**: To compare the best value of a metric across runs, set the summary value for that metric. By default, summary is set to the last value you logged for each key. This is useful in the table in the UI, where you can sort and filter runs based on their summary metrics — so you could compare runs in a table or bar chart based on their _best_ accuracy, instead of final accuracy. For example, you could set summary like so: `wandb.run.summary["best_accuracy"] = best_accuracy`
2. **Multiple metrics on one chart**: Log multiple metrics in the same call to `wandb.log`, like this: `wandb.log({"acc'": 0.9, "loss": 0.1})`  and they will both be available to plot against in the UI
3. **Custom x-axis**: Add a custom x-axis to the same log call to visualize your metrics against a different axis in the W\&B dashboard. For example: `wandb.log({'acc': 0.9, 'epoch': 3, 'batch': 117})`. To set the default x-axis for a given metric use [Run.define\_metric()](https://docs.wandb.ai/ref/python/run#define\_metric)
4. **Log rich media and charts**: `wandb.log` supports the logging of a wide variety of data types, from [media like images and videos](media.md) to [tables](../../data-vis/log-tables.md) and [charts](plots.md).

### In-**D**epth Guides

For in-depth information on how to log everything from histograms to 3d molecules, check out the guides below.

{% content-ref url="media.md" %}
[media.md](media.md)
{% endcontent-ref %}

{% content-ref url="plots.md" %}
[plots.md](plots.md)
{% endcontent-ref %}

### **Reference Documentation**

For precise details about the signatures and behavior of logging functions, review the reference docs, generated from the `wandb` Python library.

{% content-ref url="../../../ref/python/log.md" %}
[log.md](../../../ref/python/log.md)
{% endcontent-ref %}

{% content-ref url="../../../ref/python/data-types/" %}
[data-types](../../../ref/python/data-types/)
{% endcontent-ref %}

## Stepwise and Incremental Logging

Information logged to Weights & Biases with `wandb.log` is tracked over time, forming the "history" of a run. By default, each call to `wandb.log` is a new step and all of our charts and panels use the history step as the x-axis.

If you want to plot your metrics against different x-axes, you can log those values like you would any other metric, like `wandb.log({'loss': 0.1, 'epoch': 1, 'batch': 3})`. In the UI you can switch between x-axes in the chart settings.

If you want to log to a single history step from lots of different places in your code you can pass a step index to `wandb.log()` as follows:

```python
wandb.log({'loss': 0.2}, step=step)
```

As long as you keep passing the same value for `step`, W\&B will collect the keys and values from each call in one unified dictionary. As soon you call `wandb.log()` with a different value for `step` than the previous one, W\&B will write all the collected keys and values to the history, and start collection over again. Note that this means you should only use this with consecutive values for `step`: 0, 1, 2, .... This feature doesn't let you write to absolutely any history step that you'd like, only the "current" one and the "next" one.

You can also set `commit=False` in `wandb.log` to accumulate metrics, just be sure to eventually call `wandb.log` with `commit=True` (the default) to persist the metrics.

```python
wandb.log({'loss': 0.2}, commit=False)
# Somewhere else when I'm ready to report this step:
wandb.log({'accuracy': 0.8})
```

## Summary Metrics

In addition to values that change over time during training, it's often important to track a single value that summarizes a model or a preprocessing step, stored in the run's `summary` dictionary. For values that are logged with `wandb.log`, we automatically set summary to the last value added. You can also add metrics or media to the summary directly or overwrite the default values. If a summary metric is modified, the previous value is lost.

```python
wandb.init(config=args)

best_accuracy = 0
for epoch in range(1, args.epochs + 1):
  test_loss, test_accuracy = test()
  if (test_accuracy > best_accuracy):
    wandb.run.summary["best_accuracy"] = test_accuracy
    best_accuracy = test_accuracy
```

You may want to store evaluation metrics in a runs summary after training has completed. Summary can handle numpy arrays, PyTorch tensors or TensorFlow tensors. When a value is one of these types we persist the entire tensor in a binary file and store high level metrics in the summary object such as min, mean, variance, 95th percentile, etc.

```python
api = wandb.Api()
run = api.run("username/project/run_id")
run.summary["tensor"] = np.random.random(1000)
run.summary.update()
```

## Customize axes and summaries with `define_metric`

[Try `define_metric` live in Google Colab →](http://wandb.me/define-metric-colab)

Use `define_metric` to set a **custom x axis** or capture a **custom summary of a metric**.

* **Custom x-axes** are useful in contexts where you need to log to different time steps in the past during training, asynchronously. For example, this can be useful in RL where you may track the per-episode reward and a per-step reward.
* **Custom metric summaries** are useful to capture model performance at the best step, instead of the last step, of training in your `wandb.summary`. For example, you might want to capture the maximum accuracy or the minimum loss value, instead of the final value.

### Customize axes

By default, all metrics are logged against the same x-axis, which is the W\&B internal `step`. Sometimes, you might want to log to a previous step, or use a different x-axis.

Here's an example of setting a custom x-axis metric, instead of the default step.

```python
import wandb

wandb.init()
# define our custom x axis metric
wandb.define_metric("custom_step")
# define which metrics will be plotted against it
wandb.define_metric("validation_loss", step_metric="custom_step")

for i in range(10):
  log_dict = {
      "train_loss": 1/(i+1),
      "custom_step": i**2,
      "validation_loss": 1/(i+1)   
  }
  wandb.log(log_dict)
```

The x axis can be set using globs as well. Currently, only globs that have string prefixes are available. The following example will plot all logged metrics with the prefix `"train/"` to the x-axis `"train/step"`:

```python
import wandb

wandb.init()
# define our custom x axis metric
wandb.define_metric("train/step")
# set all other train/ metrics to use this step
wandb.define_metric("train/*", step_metric="train/step")

for i in range(10):
  log_dict = {
      "train/step": 2 ** i  # grows exponentially with internal wandb step
      "train/loss": 1/(i+1), # x-axis is train/step
      "train/accuracy": 1 -  (1/(1+i)), # x-axis is train/step
      "val/loss": 1/(1+i), # x-axis is internal wandb step
      
  }
  wandb.log(log_dict)
```

### Customize the summary

Summary metrics can be controlled using the `summary` argument in `define_metric` which accepts the following values: `"min"`, `"max"`, `"mean"` ,`"best"`, `"last"` and `"none"`. The `"best"` parameter can only be used in conjunction with the optional `objective` argument which accepts values `"minimize"` and `"maximize"`. Here's an example of capturing the lowest value of loss and the maximum value of accuracy in the summary, instead of the default summary behavior, which uses the final value from history.

```python
import wandb
import random

random.seed(1)
wandb.init()
# define a metric we are interested in the minimum of
wandb.define_metric("loss", summary="min")
# define a metric we are interested in the maximum of
wandb.define_metric("acc", summary="max")
for i in range(10):
  log_dict = {
      "loss": random.uniform(0,1/(i+1)),
      "acc": random.uniform(1/(i+1),1),
  }
  wandb.log(log_dict)
```

Here's what the resulting min and max summary values look like, in pinned columns in the sidebar on the Project Page workspace:

![](<../../../.gitbook/assets/image (144).png>)

## Frequently Asked Questions

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
wandb.log({"losses": np.array(losses)})  # internally converts losses to a histogram
```
{% endtab %}
{% endtabs %}

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

The history object is used to track metrics logged by `wandb.log`. Using [our API](../public-api-guide.md), you can access the history object via `run.history()`.&#x20;

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
