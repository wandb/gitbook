---
description: Pythonからトリガーされ、Slackまたはメールで送信されるスクリプト可能なアラート
---

# wandb.alert\(\)

PythonスクリプトからトリガーされたSlackまたはEメールアラートを送信します。

1. アカウントにアラートを設定する→

2. コードを試す→

3. SlackまたはEメールをチェックして、スクリプト可能なアラートを確認する。

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

