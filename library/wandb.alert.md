---
description: Send an alert notification over Slack or email.
---

# wandb.alert\(\)

### Overview

Calling `wandb.alert(title, text)` will send an alert over Slack or email, depending on which notifications you've opted into on your [Settings Page](../app/features/alerts.md#user-level-alerts). The `title` should be a short description of the alert, and `text` should provide more detailed information.

`wandb.alert` accepts a few optional keyword arguments:

* **level** — the importance of the alert, must be either `INFO`, `WARN`, or `ERROR`
* **wait\_duration** — the time to wait in seconds before sending another alert with the same title

### Examples

Let's set up a simple alert that warns us every 5 minutes whenever our accuracy falls below an acceptable threshold:

```text
from datetime import timedelta
import wandb
from wandb import AlertLevel

if acc < threshold:
    wandb.alert(
        title='Accuracy low', 
        text=f'Accuracy {acc} is below the acceptable theshold {threshold}',
        level=AlertLevel.WARN,
        wait_duration=timedelta(minutes=5)
    )
```

