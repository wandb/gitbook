---
description: Pythonからトリガーされ、Slackまたはメールで送信されるスクリプト可能なアラート
---

# wandb.alert\(\)

### Arguments

`wandb.alert(title="Low Acc", text="Accuracy is below the expected threshold")`

* **title \(string\)**: 「低精度」などのアラートについての簡単な説明
* **text \(string\)**: アラートをトリガーするために起こったことについてのより長く、より詳細な説明
* **level \(optional\):** アラートの重要性—`INFO`、`WARN`、または`ERROR`のいずれかである必要があります
* **wait\_duration \(optional\):** 同じ**タイトル**の別のアラートを送信するまでに待機する秒数。これにより、アラートスパムが減ります。

###  例

この単純なアラートは、精度がしきい値を下回るとアラートを送信します。スパムを回避するために、少なくとも5分間隔でアラートを送信します。

 [コードを試行→ ](https://colab.research.google.com/drive/1zhll1i1usBPra5CmGuPONKheFBnr6Jc4)

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

