# Skorch

SkorchでWeights＆Biasesを使用すると、すべてのモデルパフォーマンスメトリック、モデルトポロジ、および各エポック後の計算リソースとともに、最高のパフォーマンスでモデルを自動的にログに記録できます。wandb\_run.dirに保存されたすべてのファイルは、自動的にW＆Bサーバーに記録されます。

[実行例](https://wandb.ai/borisd13/skorch/runs/s20or4ct?workspace=user-borisd13)を参照してください。

##  **パラメータ**

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>&#x30D1;&#x30E9;&#x30E1;&#x30FC;&#x30BF;</b>
      </th>
      <th style="text-align:left"><b>&#x8AAC;&#x660E;</b>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">
        <p><b>wandb_run</b>:</p>
        <p>wandb.wandb_run.Run</p>
      </td>
      <td style="text-align:left">&#x30C7;&#x30FC;&#x30BF;&#x306E;&#x30ED;&#x30B0;&#x306B;&#x4F7F;&#x7528;&#x3055;&#x308C;&#x308B;wandbrun&#x3002;</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>save_model<br /></b>bool (default=True)</td>
      <td style="text-align:left">&#x6700;&#x9069;&#x306A;&#x30E2;&#x30C7;&#x30EB;&#x306E;&#x30C1;&#x30A7;&#x30C3;&#x30AF;&#x30DD;&#x30A4;&#x30F3;&#x30C8;&#x3092;&#x4FDD;&#x5B58;&#x3057;&#x3066;&#x3001;Run
        on W&#xFF06;B&#x30B5;&#x30FC;&#x30D0;&#x30FC;&#x306B;&#x30A2;&#x30C3;&#x30D7;&#x30ED;&#x30FC;&#x30C9;&#x3059;&#x308B;&#x304B;&#x3069;&#x3046;&#x304B;&#x3002;</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>keys_ignored<br /></b>str or list of str (default=None)</td>
      <td style="text-align:left">&#x30C6;&#x30F3;&#x30BD;&#x30EB;&#x30DC;&#x30FC;&#x30C9;&#x306B;&#x8A18;&#x9332;&#x3057;&#x3066;&#x306F;&#x306A;&#x3089;&#x306A;&#x3044;&#x30AD;&#x30FC;&#x307E;&#x305F;&#x306F;&#x30AD;&#x30FC;&#x306E;&#x30EA;&#x30B9;&#x30C8;&#x3002;&#x30E6;&#x30FC;&#x30B6;&#x30FC;&#x304C;&#x63D0;&#x4F9B;&#x3059;&#x308B;&#x30AD;&#x30FC;&#x306B;&#x52A0;&#x3048;&#x3066;&#x3001;&#x300C;event_&#x300D;&#x3067;&#x59CB;&#x307E;&#x308B;&#x30AD;&#x30FC;&#x3084;&#x300C;_best&#x300D;&#x3067;&#x7D42;&#x308F;&#x308B;&#x30AD;&#x30FC;&#x306A;&#x3069;&#x306E;&#x30AD;&#x30FC;&#x306F;&#x30C7;&#x30D5;&#x30A9;&#x30EB;&#x30C8;&#x3067;&#x7121;&#x8996;&#x3055;&#x308C;&#x308B;&#x3053;&#x3068;&#x306B;&#x6CE8;&#x610F;&#x3057;&#x3066;&#x304F;&#x3060;&#x3055;&#x3044;&#x3002;</td>
    </tr>
  </tbody>
</table>

## **サンプルコード**

統合がどのように機能するかを確認するために、いくつかの例を作成しました。

* [Colab](https://colab.research.google.com/drive/1Bo8SqN1wNPMKv5Bn9NjwGecBxzFlaNZn?usp=sharing): 統合を試すための簡単なデモ
*  [ステップバイステップガイド](https://wandb.ai/cayush/uncategorized/reports/Automate-Kaggle-model-training-with-Skorch-and-W&B--Vmlldzo4NTQ1NQ)：Skorchモデルのパフォーマンスを追跡

```python
# Install wandb
... pip install wandb

import wandb
from skorch.callbacks import WandbLogger

# Create a wandb Run
wandb_run = wandb.init()
# Alternative: Create a wandb Run without a W&B account
wandb_run = wandb.init(anonymous="allow")

# Log hyper-parameters (optional)
wandb_run.config.update({"learning rate": 1e-3, "batch size": 32})

net = NeuralNet(..., callbacks=[WandbLogger(wandb_run)])
net.fit(X, y)
```

##  **メソッド**

| **メソッド** |  **説明** |
| :--- | :--- |
| `initialize`\(\) | コールバックの初期状態を（再）設定します。 |
| `on_batch_begin`\(net\[, X, y, training\]\) | 各バッチの開始時に呼び出されます。 |
| `on_batch_end`\(net\[, X, y, training\]\) |  各バッチの最後に呼び出されます。 |
| `on_epoch_begin`\(net\[, dataset\_train, …\]\) |  各エポックの開始時に呼び出されます |
| `on_epoch_end`\(net, \*\*kwargs\) | 最後の履歴ステップからの値をログに記録し、最適なモデルを保存します |
| `on_grad_computed`\(net, named\_parameters\[, X, …\]\) |  グラジエントが計算された後、更新ステップが実行される前に、バッチごとに1回呼び出されます。 |
| `on_train_begin`\(net, \*\*kwargs\) | モデルトポロジをログに記録し、グラデーションのフックを追加します |
| `on_train_end`\(net\[, X, y\]\) | トレーニングの最後に呼び出されます。 |

