# wandb.alert\(\)

 通过Slack或邮箱发送警告信息。

1. [Set up alerts in your account→]()
2. [Try the code →](http://tiny.cc/wb-alerts)
3. Check your Slack or email to see the scriptable alerts.

###  **概述**

`wandb.alert(title="Low Acc", text="Accuracy is below the expected threshold")`

* **title \(string\)**: A short description of the alert, for example "Low accuracy"
* **text \(string\)**: A longer, more detailed description of what happened to trigger the alert
* **level \(optional\):** （等级）警告的重要性，必须为INFO、`WARN`和\``ERROR`其中之一。
* **wait\_duration \(optional\):**   等待期）要等多久才能再次发送标题相同的警告，单位为秒。

### **示例**

我们来设置一个简单的警告，每当准确率低于可接受的分界线，就会每隔5分钟给我们提示一次。

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

