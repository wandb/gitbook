# Resuming

`resume=True`を`wandb.init()`に渡すことで、wandbに実行を自動的に再開させることができます。プロセスが正常に終了しない場合、次に実行したときにwandbは最後のステップからロギングを開始します。以下はKerasの簡単な例です。

```python
import keras
import numpy as np
import wandb
from wandb.keras import WandbCallback
wandb.init(project="preemptable", resume=True)

if wandb.run.resumed:
    # restore the best model
    model = keras.models.load_model(wandb.restore("model-best.h5").name)
else:
    a = keras.layers.Input(shape=(32,))
    b = keras.layers.Dense(10)(a)
    model = keras.models.Model(input=a,output=b)

model.compile("adam", loss="mse")
model.fit(np.random.rand(100, 32), np.random.rand(100, 10),
    # set the resumed epoch
    initial_epoch=wandb.run.step, epochs=300,
    # save the best model if it improved each epoch
    callbacks=[WandbCallback(save_model=True, monitor="loss")])
```

自動再開は、失敗したプロセスと同じファイルシステム上でプロセスが再起動された場合にのみ機能します。ファイルシステムを共有できない場合は、**WANDB\_RUN\_ID**を設定できます。これは、スクリプトの1回の実行に対応するグローバルに一意の文字列（プロジェクトごと）です。64文字以内である必要があります。単語以外の文字はすべてダッシュに変換されます。

```python
# store this id to use it later when resuming
id = wandb.util.generate_id()
wandb.init(id=id, resume="allow")
# or via environment variables
os.environ["WANDB_RESUME"] = "allow"
os.environ["WANDB_RUN_ID"] = wandb.util.generate_id()
wandb.init()
```

**WANDB\_RESUME**を「allow」に設定すると、いつでも**WANDB\_RUN\_ID**を一意の文字列に設定でき、プロセスの再起動が自動的に処理されます。**WANDB\_RESUME**を「must」に設定すると、新しい実行を自動作成する代わりに、再開する実行がまだ存在しない場合、wandbはエラーをスローします

| メソッド | シンタクス |  再開しない（デフォルト） | 常に再開する | 実行IDの指定を再開する | 同じディレクトリから再開する |
| :--- | :--- | :--- | :--- | :--- | :--- |
| コマンドライン | wandb run --resume= | 「決して」 | 「必須」 | 「許可」（WANDB\_RUN\_ID = RUN\_IDが必要） | 再開しない（デフォルト） |
| 環境 | WANDB\_RESUME= | 「決して」 | 「必須」 | 「許可」（WANDB\_RUN\_ID = RUN\_IDが必要） | 再開しない（デフォルト） |
| 初期化 | wandb.init\(resume=\) |  | 再開しない（デフォルト） |  | resume=True |

{% hint style="warning" %}
複数のプロセスが同じrun\_idを同時に使用すると、予期しない結果が記録され、レート制限が発生します。
{% endhint %}

{% hint style="info" %}
実行を再開し、wandb.init（）で指定されたメモがある場合、それらのメモはUIで追加したメモを上書きします。
{% endhint %}

