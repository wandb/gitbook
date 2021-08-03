# Advanced Features

The guides in this section go beyond core Weights & Biases experiment-tracking features like[ logging data and media](../log/), [building rich dashboards](../app.md), and [seamlessly integrating with popular frameworks and tools](../../integrations/) to cover advanced use cases and power-user features.

{% hint style="info" %}
Looking for the gory details on how the `wandb` library, CLI, and UI tools work? You want the [Reference](../../../ref/) documentation.
{% endhint %}

Need to **track large-scale ML experiments distributed across multiple GPUs** and multiple nodes? Then check out our guide to [Distributed Training](distributed-training.md). For some approaches to distributed training and [cross-validation](https://github.com/wandb/examples/tree/master/examples/wandb-sweeps/sweeps-cross-validation), you also need to **combine multiple runs together into a single experiment**, as described in our guide on how to [Group Runs](grouping.md).

At Weights & Biases, we're all about preventing you from losing any of your work. If you're **using** [**pre-emptible compute**](https://cloud.google.com/preemptible-vms) **or your machine crashes**, we'll help you [Resume Runs](resuming.md) where you left off. If you're **in danger of losing valuable data**, `wandb` can even [Save & Restore Files](save-restore.md).

Tired of **wondering whether training has finished or, worse, crashed**? Set up [Alerts](alert.md) to Slack or your e-mail, with configurable triggers right in your Python code.

The behavior of the tool is **controllable from the command line**, as described in our guide to [Environment Variables](environment-variables.md).

{% page-ref page="distributed-training.md" %}

{% page-ref page="grouping.md" %}

{% page-ref page="resuming.md" %}

{% page-ref page="save-restore.md" %}

{% page-ref page="alert.md" %}

{% page-ref page="environment-variables.md" %}

