---
description: How to integrate a TensorFlow script to log metrics to W&B
---

# TensorFlow

If you're already using TensorBoard, it's easy to integrate with wandb.

```python
import tensorflow as tf
import wandb
wandb.init(config=tf.flags.FLAGS, sync_tensorboard=True)
```

See our [example projects](../example-projects/) for a complete script example.

## Custom Metrics

If you need to log additional custom metrics that aren't being logged to TensorBoard, you can call `wandb.log` in your code with the same step argument that TensorBoard is using: ie. `wandb.log({"custom": 0.8}, step=global_step)`

## TensorFlow Hook

If you want more control over what get's logged, wandb also provides a hook for TensorFlow estimators. It will log all `tf.summary` values in the graph.

```python
import tensorflow as tf
import wandb

wandb.init(config=tf.FLAGS)

estimator.train(hooks=[wandb.tensorflow.WandbHook(steps_per_log=1000)])
```

## Manual Logging

The simplest way to log metrics in TensorFlow is by logging `tf.summary` with the TensorFlow logger:

```python
import wandb

with tf.Session() as sess:
    # ...
    wandb.tensorflow.log(tf.summary.merge_all())
```

## How is W&B different from TensorBoard?

We were inspired to improve experiment tracking tools for everyone. When the cofounders started working on W&B, they were inspired to build a tool for the frustrated TensorBoard users at OpenAI. Here are a few things we focused on improving:

1. **Reproduce models**: Weights & Biases is good for experimentation, exploration, and reproducing models later. We capture not just the metrics, but also the hyperparameters and version of the code, and we can save your model checkpoints for you so your project is reproducible. 
2. **Automatic organization**: If you hand off a project to a collaborator or take a vacation, W&B makes it easy to see all the models you've tried so you're not wasting hours re-running old experiments.
3. **Fast, flexible integration**: Add W&B to your project in 5 minutes. Install our free open-source Python package and add a couple of lines to your code, and every time you run your model you'll have nice logged metrics and records.
4. **Persistent, centralized dashboard**: Anywhere you train your models, whether on your local machine, your lab cluster, or spot instances in the cloud, we give you the same centralized dashboard. You don't need to spend your time copying and organizing TensorBoard files from different machines.
5. **Powerful table**: Search, filter, sort, and group results from different models. It's easy to look over thousands of model versions and find the best performing models for different tasks. TensorBoard isn't built to work well on large projects.
6. **Tools for collaboration**: Use W&B to organize complex machine learning projects. It's easy to share a link to W&B, and you can use private teams to have everyone sending results to a shared project. We also support collaboration via reports— add interactive visualizations and describe your work in markdown. This is a great way to keep a work log, share findings with your supervisor, or present findings to your lab.

Get started with a [free personal account →](http://app.wandb.ai/)

