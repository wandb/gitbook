---
description: Sending data to W&B Enterprise installations
---

# Sending Data

For the most part, you can use W&B Enterprise the same way as our Software-as-a-Service offering. There are a few tweaks to keep in mind.

## Command Line Usage

You'll need to fetch your API key from your W&B Server instead of `wandb.ai` with one of the following methods. W&B Enterprise Server API keys have a dedicated prefix to avoid confusion between the SaaS system and your private installation.

```bash
# Save a wandb settings file in the `wandb` subfolder of your python path.
mkdir -p ~/.config/wandb
echo -e "[default]\nbase_url = http://your-server-ip-or-host" > ~/.config/wandb/settings
wandb login
python your-training-script.py

# Or, configure the CLI target with environment variables.
WANDB_BASE_URL=http://your-server-ip-or-host wandb login
WANDB_BASE_URL=http://your-server-ip-or-host python your-training-script.py
```

## Jupyter Notebooks

In jupyter notebooks, simply set the environment variable `WANDB_BASE_URL` to the host or IP address of the server running W&B enterprise. For example:

```python
import os
os.environ['WANDB_BASE_URL'] = 'http://your-server-ip-or-host'

# At this point you will be asked for an API KEY to your server.
import wandb
wandb.init()
```

