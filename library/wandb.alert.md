# wandb.alert\(\)

通过Python脚本触发警报（Alert），通过Slack或电子邮件发送给你。

1.  [在你的账户中设置警报→](https://docs.wandb.ai/v/zh-hans/dashboard/features/alerts)
2.   ****[试试代码→](http://tiny.cc/wb-alerts)
3. 检查你的Slack或电子邮件以查看脚本触发的警报。

###  **概述**

`wandb.alert(title="Low Acc", text="Accuracy is below the expected threshold")`

* **title \(string\)**: 警报的简短描述, 例如“Low accuracy”
* **text \(string\)**: 关于触发警报的事件的一个更长更详细的说明
* **level \(可选\):** 等级）警报的重要性，必须为INFO、`WARN`和\``ERROR`其中之一。
* **wait\_duration \(可选\):** 在发送具有相同 title 的另一个警报之前要等待多少秒。这有助于减少垃圾警报。

### **示例**

这个简单的警报会在准确率低于阈值时发出一个警告。为避免垃圾警报，相距至少5分钟它才会发送警报。

[ ](http://tiny.cc/wb-alerts) [运行代码 →](http://tiny.cc/wb-alerts)​​

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

