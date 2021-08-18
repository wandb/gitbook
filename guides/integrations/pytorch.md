---
description: How to integrate a PyTorch script to log metrics to W&B
---

# PyTorch

{% hint style="info" %}
Try our integration out in a [colab notebook](https://colab.research.google.com/github/wandb/examples/blob/master/colabs/pytorch/Simple_PyTorch_Integration.ipynb) \(with video walkthrough below\) or see our [example repo](https://github.com/wandb/examples) for scripts, including one on hyperparameter optimization using [Hyperband](https://arxiv.org/abs/1603.06560) on [Fashion MNIST](https://github.com/wandb/examples/tree/master/examples/pytorch/pytorch-cnn-fashion), plus the [W&B Dashboard](https://wandb.ai/wandb/keras-fashion-mnist/runs/5z1d85qs) it generates.
{% endhint %}

{% embed url="https://www.youtube.com/watch?v=G7GH0SeNBMA" caption="Follow along with a video tutorial!" %}

## Logging gradients with `wandb.watch`

W&B provides first class support for PyTorch. To automatically log gradients, you can call [`wandb.watch`](../../ref/python/watch.md) and pass in your PyTorch model.

```python
import wandb
wandb.init(config=args)

model = ... # set up your model

# Magic
wandb.watch(model, log_freq=100)

model.train()
for batch_idx, (data, target) in enumerate(train_loader):
    output = model(data)
    loss = F.nll_loss(output, target)
    loss.backward()
    optimizer.step()
    if batch_idx % args.log_interval == 0:
        wandb.log({"loss": loss})
```

If you need to track multiple models in the same script, you can call `wandb.watch` on each model separately. Reference documentation for this function is [here](../../ref/python/watch.md).

{% hint style="warning" %}
Gradients, metrics and the graph won't be logged until `wandb.log` is called after a forward _and_ backward pass.
{% endhint %}

## Logging images and media

You can pass PyTorch `Tensors` with image data into [`wandb.Image`](../../ref/python/data-types/image.md) and utilities from [`torchvision`](https://pytorch.org/vision/stable/index.html) will be used to convert them to images automatically:

```python
images_t = ...  # generate or load images as PyTorch Tensors
wandb.log({"examples" : [wandb.Image(im) for im in images_t]})
```

For more on logging rich media to W&B in PyTorch and other frameworks, check out our [media logging guide](../track/log/media.md).

## Profiling PyTorch code

![View detailed traces of PyTorch code execution inside W&amp;B dashboards.](../../.gitbook/assets/image%20%28136%29.png)

W&B integrates directly with [PyTorch Kineto](https://github.com/pytorch/kineto)'s [Tensorboard plugin](https://github.com/pytorch/kineto/blob/master/tb_plugin/README.md) to provide tools for profiling PyTorch code, inspecting the details of CPU and GPU communication, and identifying bottlenecks and optimizations.

```python
profiler = torch.profiler.profile(
    schedule=schedule,  # see the profiler docs for details on scheduling
    on_trace_ready=torch.profiler.tensorboard_trace_handler("path/to/run/tbprofile")
    with_stack=True)

with profiler:
    ...  # run the code you want to profile here
    # see the profiler docs for detailed usage information

# create a wandb Artifact
profile_art = wandb.Artifact("trace", type="profile")
# 
profile_art.add_file("path/to/run/tbprofile/*.pt.trace.json")
run.log_artifact(profile_art) 
```

See and run working example code in [this Colab](http://wandb.me/trace-colab).

{% hint style="warning" %}
The interactive trace viewing tool is based on the Chrome Trace Viewer, which only works with the Chrome browser.
{% endhint %}

## Data visualization with W&B Tables

Use [W&B Tables](https://docs.wandb.ai/guides/data-vis) to log, query, and analyze your data. You can think of a W&B Table as a `DataFrame` that you can interact with inside W&B. Tables support rich media types, primitive and numeric types, as well as nested lists and dictionaries. 

This pseudo-code shows you how to log images, along with their ground truth and predicted class, to W&B Tables:

```python
# Create a new W&B Run
wandb.init(project="mnist")

# Create a W&B Table
my_table = wandb.Table(columns=["id", "image", "labels", "prediction"])

# Get your image data and make predictions
image_tensors, labels = get_mnist_data()
predictions = model(image_tensors)

# Add your image data and predictions to the W&B Table
for idx, im in enumerate(image_tensors): 
  my_table.add_data(idx, wandb.Image(im), labels[idx], predictions[id])

# Log your Table to W&B
wandb.log({"mnist_predictions": my_table})
```

This is will produce a Table like this:

![](../../.gitbook/assets/screenshot-2021-07-14-at-20.18.39.png)

For more examples of data visualization with W&B Tables, please see [the documentation](https://docs.wandb.ai/guides/data-vis).

