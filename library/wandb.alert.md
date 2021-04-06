---
description: Pythonでトリガーされるスクリプト可能なアラート。Slackまたはメールで送信。
---

# wandb.alert\(\)

 Slackまたはメールでアラートを送信します。Pythonスクリプトでトリガーされます。

1. [アカウントでアラートのセットアップ→](file:////app/features/alerts)​
2. ​[コードを試す→](http://tiny.cc/wb-alerts)​
3. Slackまたはメールのスクリプトアラートを確認する

### **関数** <a id="arguments"></a>

`wandb.alert(title="Low Acc", text="Accuracy is below the expected threshold")`

* **title \(文字列\)**: 「低精度」などのアラートについての簡単な説明
* **text \(文字列\)**: 何が起こったためアラートがトリガーされたかについてのより長く詳細な説明
* **level \(オプション\):** アラートの重要性—`INFO`、`WARN`、または`ERROR`のいずれかである必要があります
* **wait\_duration \(オプション\):** 同じ**タイトル**の別のアラートを送信するまでに待機する秒数。これにより、アラートスパムが減ります。

###  **例** <a id="example"></a>

このシンプルなアラートは、精度がしきい値を下回ると警告を送信するようにします。スパムを回避するために、少なくとも5分間隔でアラートを送信します。

​ [コードを試行→ ](https://colab.research.google.com/drive/1zhll1i1usBPra5CmGuPONKheFBnr6Jc4)​​

```text
from datetime import timedeltaimport wandbfrom wandb import AlertLevel​if acc < threshold:    wandb.alert(        title='Low accuracy',         text=f'Accuracy {acc} is below the acceptable theshold {threshold}',        level=AlertLevel.WARN,        wait_duration=timedelta(minutes=5)    )
```

[  
](https://docs.wandb.ai/library/log)

