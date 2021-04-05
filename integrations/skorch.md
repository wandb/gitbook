# Skorch

你可以通过Skorch使用Weights & Biases以自动记录具有最佳性能的模型– 以及每个周期后的所有模型性能指标、模型拓扑和计算资源。保存在wandb\_run.dir中的每个文件都会被自动记录到W&B服务器。

 查看[示例运行](https://app.wandb.ai/borisd13/skorch/runs/s20or4ct?workspace=user-borisd13)

##  **参数**

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>&#x53C2;&#x6570;</b>
      </th>
      <th style="text-align:left">&#x8BF4;&#x660E;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">
        <p><b>wandb_run</b>:</p>
        <p>wandb.wandb_run.Run</p>
      </td>
      <td style="text-align:left">wandb run &#x7528;&#x4E8E;&#x8BB0;&#x5F55;&#x6570;&#x636E;&#x3002;</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>save_model<br /></b>bool (default=True)</td>
      <td style="text-align:left">&#x662F;&#x5426;&#x4FDD;&#x5B58;&#x6700;&#x4F73;&#x6A21;&#x578B;&#x7684;&#x4E00;&#x4E2A;&#x68C0;&#x67E5;&#x70B9;&#x5E76;&#x5C06;&#x5176;&#x4E0A;&#x4F20;&#x5230;&#x4F60;&#x5728;W&amp;B&#x670D;&#x52A1;&#x5668;&#x4E0A;&#x7684;&#x8FD0;&#x884C;&#x4E2D;&#x3002;</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>keys_ignored </b>&#x5B57;&#x7B26;&#x4E32;&#x6216;&#x5B57;&#x7B26;&#x4E32;&#x5217;&#x8868;(&#x9ED8;&#x8BA4;=None)</td>
      <td
      style="text-align:left">&#x4E0D;&#x5E94;&#x8BE5;&#x88AB;&#x8BB0;&#x5F55;&#x5230;tensorboard&#x7684;&#x952E;&#x6216;&#x952E;&#x5217;&#x8868;&#x3002;&#x8BF7;&#x6CE8;&#x610F;&#xFF0C;&#x9664;&#x4E86;&#x7528;&#x6237;&#x63D0;&#x4F9B;&#x7684;&#x952E;&#x5916;&#xFF0C;&#x4EE5;event_&#x2019;&#x5F00;&#x5934;&#x6216;&#x4EE5;&#x2018;_best&#x2019;&#x7ED3;&#x5C3E;&#x7684;&#x952E;&#x9ED8;&#x8BA4;&#x4F1A;&#x88AB;&#x5FFD;&#x7565;&#x3002;</td>
    </tr>
  </tbody>
</table>

## **示例代码**

我们已经为你创建了一些示例，以了解集成的工作原理：

*  [Colab](https://colab.research.google.com/drive/1Bo8SqN1wNPMKv5Bn9NjwGecBxzFlaNZn?usp=sharing): 一个尝试集成的简单演示
* [一步一步指导 ](https://app.wandb.ai/cayush/uncategorized/reports/Automate-Kaggle-model-training-with-Skorch-and-W&B--Vmlldzo4NTQ1NQ): 跟踪你的Skorch模型性能

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

## **方法**

| 方法 | 说明 |
| :--- | :--- |
| `initialize`\(\) | \(重新\)设置回调的初始状态 |
| `on_batch_begin`\(net\[, X, y, training\]\) | 在每个批次开始时被调用 |
| `on_batch_end`\(net\[, X, y, training\]\) | 在每个批次结束时被调用 |
| `on_epoch_begin`\(net\[, dataset\_train, …\]\) | 在每个周期（epoch）开始时被调用 |
| `on_epoch_end`\(net, \*\*kwargs\) | 对上一个历史步（step）的数值进行记录，并保存最佳模型。 |
| `on_grad_computed`\(net, named\_parameters\[, X, …\]\) | 在计算完梯度后，但在执行更新步（step）之前，每个批次调用一次。 |
| `on_train_begin`\(net, \*\*kwargs\) | 记录模型拓扑，并为梯度添加一个钩子。 |
| `on_train_end`\(net\[, X, y\]\) | 在训练结束时被调用。 |

