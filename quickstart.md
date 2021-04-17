---
description: Start tracking machine learning experiments in 5 minutes
---

# Quickstart

### 1. Set up wandb

[Sign up](https://app.wandb.ai/login?signup=true) for a free account and install the library `wandb` in a Python 3 environment from the command line. 

```bash
pip install wandb
wandb login
```

### 2. Start a new run

Initialize a new run in W&B in your Python script or notebook. `wandb.init()` will start tracking system metrics and console logs, right out of the box. Run your code to see the new run appear in W&B. [Learn more →](guides/track/launch.md)

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

Save hyperparameters so you can quickly compare and reproduce experiments. [Learn more →](guides/track/config.md)

```python
wandb.config.dropout = 0.2
```

## Quickstart FAQ

**How do I use wandb in an automated environment?**  
If you are training models in an automated environment where it's inconvenient to run shell commands, such as Google's CloudML, you should look at our [Environment Variables](guides/track/advanced/environment-variables.md).

**Do you offer on-prem or private cloud installations?**  
Yes, learn more about[ W&B Local installations](guides/self-hosted/).

**How do I turn off wandb logging temporarily**?  
If you're testing later and want to disable wandb syncing, set the environment variable [WANDB\_MODE=dryrun](guides/track/advanced/environment-variables.md).



