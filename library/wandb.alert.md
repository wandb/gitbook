---
description: 'Scriptable alerts triggered from Python, sent to you via Slack or email'
---

# wandb.alert\(\)

Send a Slack or email alert, triggered from your Python script.

1. [Set up alerts in your account→](../app/features/alerts.md)
2. [Try the code →](http://tiny.cc/wb-alerts)
3. Check your Slack or email to see the scriptable alerts.

### Arguments

`wandb.alert(title="Low Acc", text="Accuracy is below the expected threshold")`

* **title \(string\)**: A short description of the alert, for example "Low accuracy"
* **text \(string\)**: A longer, more detailed description of what happened to trigger the alert
* **level \(optional\):** How important the alert is — must be either `INFO`, `WARN`, or `ERROR`
* **wait\_duration \(optional\):** How many seconds to wait before sending another alert with the same **title.** This helps reduce alert spam.

### Example

This simple alert sends a warning when accuracy falls below a threshold. To avoid spam, it only sends alerts at least 5 minutes apart.

[Run the code →](http://tiny.cc/wb-alerts)

```python
from datetime import timedelta
import wandb
from wandb import AlertLevel

if acc < threshold:
    wandb.alert(
        title='Low accuracy', 
        text=f'Accuracy {acc} is below the acceptable theshold {threshold}',
        level=AlertLevel.WARN,
        wait_duration=timedelta(minutes=5)
    )
```

