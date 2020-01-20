---
description: >-
  Tools for distributed training, launching jobs in parallel, and automating
  jobs
---

# Advanced

## Topics

For distributed training or launching automated jobs, we have a few features to help you log to wandb in an organized way.

{% page-ref page="environment-variables.md" %}

{% page-ref page="grouping.md" %}

{% page-ref page="resuming.md" %}

For sensitive and large-scale projects, here are details about how we manage security and limits. If you'd like more information or have concerns, [we'd love to talk to you](../../overview/getting-help.md)!

{% page-ref page="security.md" %}

{% page-ref page="limits.md" %}

## Common Questions

### How do I launch multiple runs from one script?

If you're trying to start multiple runs from one script, add two things to your code:

1. wandb.init\(**reinit=True**\): Use this setting to allow reinitializing runs
2. **wandb.join\(\)**: Use this at the end of your run to finish logging for that run

```python
import wandb
for x in range(10):
	wandb.init(project="runs-from-for-loop", reinit=True)
	for y in range (100):
		wandb.log({"metric": x+y})
	wandb.join()
```

