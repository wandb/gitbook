---
description: >-
  Each training run of your model gets a dedicated page, organized within the
  larger project
---

# Run Page

Use the run page to explore detailed information about a single version of your model.

## Overview Tab

* Run name, description, and tags
* Host name, operating system, python version, and command that launched the run
* List of config parameters saved with [wandb.config](../../library/config.md)
* List of summary parameters saved with [wandb.log\(\)](../../library/log.md), by default set to the last value logged

[View a live example →](https://app.wandb.ai/wandb/examples-keras-cnn-fashion/runs/wec25l0q/overview)

![](../../.gitbook/assets/image%20%2810%29.png)

![](../../.gitbook/assets/image%20%285%29.png)

## Graphs Tab

* Search, group, and arrange visualizations
* Click the pencil icon ✏️ on a graph to edit
  * change x-axis, metrics, and ranges
  * edit legends, titles, and colors of charts
* View examples predictions from your validation set

[View a live example →](https://app.wandb.ai/wandb/examples-keras-cnn-fashion/runs/wec25l0q?workspace=user-carey)

![](../../.gitbook/assets/image%20%2820%29.png)

## System Tab

* Visualize CPU utilization, system memory, disk I/O, network traffic, GPU utilization, GPU temperature, GPU time spent accessing memory, GPU memory allocated, and GPU power usage
* Lambda Labs wrote about using our system metrics. [Read the blog post →](https://lambdalabs.com/blog/weights-and-bias-gpu-cpu-utilization/)

[View a live example →](https://app.wandb.ai/wandb/feb8-emotion/runs/toxllrmm/system)

![](../../.gitbook/assets/image%20%2845%29.png)



