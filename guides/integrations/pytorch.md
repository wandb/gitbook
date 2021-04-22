---
description: How to integrate a PyTorch script to log metrics to W&B
---

# PyTorch

## Usage Examples

{% hint style="info" %}
Try our integration out in a [colab notebook](https://colab.research.google.com/github/wandb/examples/blob/master/colabs/pytorch/Simple_PyTorch_Integration.ipynb) \(with video walkthrough below\) or see our [example repo](https://github.com/wandb/examples) for scripts, including one on hyperparameter optimization using [Hyperband](https://arxiv.org/abs/1603.06560) on [Fashion MNIST](https://github.com/wandb/examples/tree/master/examples/pytorch/pytorch-cnn-fashion), plus the [W&B Dashboard](https://wandb.ai/wandb/keras-fashion-mnist/runs/5z1d85qs) it generates.
{% endhint %}

{% embed url="https://www.youtube.com/watch?v=G7GH0SeNBMA" caption="Follow along with a video tutorial!" %}

## Using `wandb.watch`

W&B provides first class support for PyTorch. To automatically log gradients and store the network topology, you can call `watch` and pass in your PyTorch model.

```python
import wandb
wandb.init(config=args)

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

{% hint style="warning" %}
Gradients, metrics and the graph won't be logged until `wandb.log` is called after a forward _and_ backward pass.
{% endhint %}

### Options

By default the hook only logs gradients.

<table>
  <thead>
    <tr>
      <th style="text-align:left">Arguments</th>
      <th style="text-align:left">Options</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>log</code>
      </td>
      <td style="text-align:left">
        <ul>
          <li><code>all</code>: log histograms of both gradients and parameters</li>
          <li><code>gradients </code>: log histograms of gradients (default)</li>
          <li><code>parameters </code>: log histograms of parameters</li>
          <li><code>None</code>
          </li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>log_freq</code>
      </td>
      <td style="text-align:left">integer (default <code>1000</code>): The number of steps between logging
        gradients/parameters</td>
    </tr>
  </tbody>
</table>

## Images

You can pass PyTorch tensors with image data into [`wandb.Image`](../../ref/python/data-types/image.md) and [`torchvision`](https://pytorch.org/vision/stable/index.html) utils will be used to log them automatically.

To log images and view them in the [Media panel](../track/log.md#media), you can use the following syntax:

```python
wandb.log({"examples" : [wandb.Image(i) for i in images]})
```

## Multiple Models

If you need to track multiple models in the same script, you can call `wandb.watch` on each model separately.

