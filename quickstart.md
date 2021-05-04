---
description: Start tracking machine learning experiments in 5 minutes
---

# Quickstart

### 1. Set up wandb

[Sign up](https://app.wandb.ai/login?signup=true) for a free account and install the library `wandb` in a Python 3 environment from the command line. In a notebook, call `wandb.login()` in Python istead.

```bash
pip install wandb
wandb login
```

### 2. Start a new run

Initialize a new run in W&B in your Python script or notebook. `wandb.init()` will start tracking system metrics and console logs, right out of the box. Run your code, put in [your API key](https://wandb.ai/authorize) when prompted, and you'll see the new run appear in W&B. [Learn more →](guides/track/launch.md)

```python
import wandb
wandb.init(project="my-test-project")
```

### 3. Track metrics

Use `wandb.log()` to track metrics, or a framework [integration](guides/integrations/) for easy instrumentation. [Learn more →](guides/track/log.md)

```python
wandb.log({'accuracy': train_acc, 'loss': train_loss})
```

###  4. Track hyperparameters

Save hyperparameters so you can quickly compare experiments. [Learn more →](guides/track/config.md)

```python
wandb.config.dropout = 0.2
```

## Common Questions

**Where do I find my API key?**  
Once you've logged in, it will be on the [Authorize page](https://wandb.ai/authorize).

**How do I use W&B in an automated environment?**  
If you are training models in an automated environment where it's inconvenient to run shell commands, such as Google's CloudML, you should look at our guide to configuration with [Environment Variables](guides/track/advanced/environment-variables.md).

**Do you offer on-prem installs?**  
Yes, you can [self-host W&B Local](guides/self-hosted/) on your own machines or in a private cloud.

**How do I turn off wandb logging temporarily?**  
If you're testing code and want to disable wandb syncing, set the environment variable [`WANDB_MODE=offline`](guides/track/advanced/environment-variables.md).



