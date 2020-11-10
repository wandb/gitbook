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

[View a live example →](https://app.wandb.ai/carey/pytorch-cnn-fashion/runs/munu5vvg/overview?workspace=user-carey)

![](../../.gitbook/assets/run-page-overview-tab.png)

The Python details are private, even if you make the page itself public. Here is an example of my run page in incognito on the left and my account on the right.

![](../../.gitbook/assets/screen-shot-2020-04-07-at-7.46.39-am.png)

## Graphs Tab

* Search, group, and arrange visualizations
* Click the pencil icon ✏️ on a graph to edit
  * change x-axis, metrics, and ranges
  * edit legends, titles, and colors of charts
* View examples predictions from your validation set

[View a live example →](https://app.wandb.ai/wandb/examples-keras-cnn-fashion/runs/wec25l0q?workspace=user-carey)

![](../../.gitbook/assets/image%20%2837%29.png)

## System Tab

* Visualize CPU utilization, system memory, disk I/O, network traffic, GPU utilization, GPU temperature, GPU time spent accessing memory, GPU memory allocated, and GPU power usage
* Lambda Labs wrote about using our system metrics. [Read the blog post →](https://lambdalabs.com/blog/weights-and-bias-gpu-cpu-utilization/)

[View a live example →](https://app.wandb.ai/wandb/feb8-emotion/runs/toxllrmm/system)

![](../../.gitbook/assets/image%20%2888%29.png)

## Model Tab

* See the layers of your model, the number of parameters, and the output shape of each layer

[View a live example →](https://app.wandb.ai/stacey/deep-drive/runs/pr0os44x/model)

![](../../.gitbook/assets/image%20%2834%29.png)

## Logs Tab

* Output printed on the command line, the stdout and stderr from the machine training the model
* We show the last 1000 lines. After the run has finished, if you'd like to download the full log file, click the download button in the upper right corner.

[View a live example →](https://app.wandb.ai/stacey/deep-drive/runs/pr0os44x/logs)

![](../../.gitbook/assets/image%20%2874%29.png)

## Files Tab

* Save files to sync with the run using [wandb.save\(\)](../../library/save.md) — _we're Dropbox for AI_
* Keep model checkpoints, validation set examples, and more
* Use the diff.patch to [restore](../../library/restore.md) the exact version of your code

[View a live example →](https://app.wandb.ai/stacey/deep-drive/runs/pr0os44x/files/media/images)

![](../../.gitbook/assets/image%20%283%29.png)

