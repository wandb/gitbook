# Security

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

Here's a simple example:

```python
import wandb
for x in range(10):
	wandb.init(project="runs-from-for-loop", reinit=True)
	for y in range (100):
		wandb.log({"metric": x+y})
	wandb.join()
```

If you're launching many runs, you might also be interested in our Sweeps tool for hyperparameter optimization, tuning, and model search.

{% page-ref page="../../sweeps/overview/" %}

### Switching between accounts

If you have two W&B accounts working from the same machine, you'll need a nice way to switch between your different API keys. You can store both API keys in a file on your machine then add code like the following to your repos. This is to avoid checking your secret key into a source control system, which is potentially dangerous.

```text
if os.path.exists("~/keys.json"):
   os.environ["WANDB_API_KEY"] = json.loads("~/keys.json")["work_account"]
```

