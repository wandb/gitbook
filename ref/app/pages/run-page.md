---
description: >-
  Each training run of your model gets a dedicated page, organized within the
  larger project
---

# Run Page

Use the run page to explore detailed information about a single version of your model.

## Overview Tab

* Run name, description, and tags
* Run state
  * **finished**: script ended and fully synced data, or called `wandb.finish()` 
  * **failed**: script ended with a non-zero exit status
  * **crashed**: script stopped sending heartbeats in the internal process, which can happen if the machine crashes
  * **running**: script is still running and has recently sent a heartbeat
* Host name, operating system, Python version, and command that launched the run
* List of config parameters saved with [wandb.config](../../../guides/track/config.md)
* List of summary parameters saved with [wandb.log\(\)](../../../guides/track/log.md), by default set to the last value logged

[View a live example →](https://app.wandb.ai/carey/pytorch-cnn-fashion/runs/munu5vvg/overview?workspace=user-carey)

![W&amp;B Dashboard run overview tab](../../../.gitbook/assets/wandb-run-overview-page.png)

The Python details are private, even if you make the page itself public. Here is an example of my run page in incognito on the left and my account on the right.

![](../../../.gitbook/assets/screen-shot-2020-04-07-at-7.46.39-am.png)

## Charts Tab

* Search, group, and arrange visualizations
* Click the pencil icon ✏️ on a graph to edit
  * change x-axis, metrics, and ranges
  * edit legends, titles, and colors of charts
* View examples predictions from your validation set
* To get these charts, log data with [wandb.log\(\)](../../../guides/track/log.md)

[View a live example →](https://app.wandb.ai/wandb/examples-keras-cnn-fashion/runs/wec25l0q?workspace=user-carey)

![](../../../.gitbook/assets/wandb-run-page-workspace-tab%20%281%29.png)

## System Tab

* Visualize CPU utilization, system memory, disk I/O, network traffic, GPU utilization, GPU temperature, GPU time spent accessing memory, GPU memory allocated, and GPU power usage
* Lambda Labs highlighted how to use W&B system metrics in a[ blog post →](https://lambdalabs.com/blog/weights-and-bias-gpu-cpu-utilization/)

[View a live example →](https://wandb.ai/stacey/deep-drive/runs/ki2biuqy/system?workspace=user-carey)

![](../../../.gitbook/assets/wandb-system-utilization.png)

## Model Tab

* See the layers of your model, the number of parameters, and the output shape of each layer

[View a live example →](https://app.wandb.ai/stacey/deep-drive/runs/pr0os44x/model)

![](../../../.gitbook/assets/wandb-run-page-model-tab.png)

## Logs Tab

* Output printed on the command line, the stdout and stderr from the machine training the model
* We show the last 1000 lines. After the run has finished, if you'd like to download the full log file, click the download button in the upper right corner.

[View a live example →](https://app.wandb.ai/stacey/deep-drive/runs/pr0os44x/logs)

![](../../../.gitbook/assets/wandb-run-page-log-tab.png)

## Files Tab

* Save files to sync with the run using [`wandb.save()`](../../../guides/track/advanced/save-restore.md)
* Keep model checkpoints, validation set examples, and more
* Use the `diff.patch` to [restore](../../../guides/track/advanced/save-restore.md) the exact version of your code [View a live example →](https://app.wandb.ai/stacey/deep-drive/runs/pr0os44x/files/media/images)

{% hint style="info" %}
The W&B [Artifacts](../../../guides/artifacts/) system adds extra features for handling, versioning, and deduplicating large files like datasets and models. We recommend you use Artifacts for tracking inputs and outputs of runs, rather than `wandb.save`. Check out the Artifacts quickstart [here](../../../guides/artifacts/).
{% endhint %}

![](../../../.gitbook/assets/wandb-run-page-files-tab.png)

## Artifacts Tab

* Provides a searchable list of the input and output [Artifacts](../../../guides/artifacts/) for this run
* Click a row to see information about a particular artifact used or produced by this run
* See the reference for the [project](project-page.md)-level [Artifacts Tab](project-page.md#artifacts-tab) for more on navigating and using the artifacts viewers in the web app

![](../../../.gitbook/assets/image%20%28141%29.png)

