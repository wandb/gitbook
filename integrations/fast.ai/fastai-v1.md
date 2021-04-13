# fastai v1

提示：这篇文档专用于Fastai v1。如果你用的是Fastai最新版，请参考[Fastai页面](https://docs.wandb.ai/v/zh-hans/integrations/fast.ai)。

对于使用Fastai v1的脚本，我们有一个回调函数，可以自动记录模型拓扑、损失、指标、权重、梯度、样本预测以及最佳训练模型。

```text
import wandbfrom wandb.fastai import WandbCallback​wandb.init()​learn = cnn_learner(data,                    model,                    callback_fns=WandbCallback)learn.fit(epochs)
```

需要记录的数据可通过回调构造函数进行配置。

```text
from functools import partial​learn = cnn_learner(data, model, callback_fns=partial(WandbCallback, input_type='images'))
```

也可以只在开始训练时使用WandbCallback。在这种情况下，它必须被实例化。

```text
learn.fit(epochs, callbacks=WandbCallback(learn))
```

自定义参数也可在这一阶段给定。

```text
learn.fit(epochs, callbacks=WandbCallback(learn, input_type='images'))
```

## **示例代码** <a id="example-code"></a>

我们已经为你创建了一些示例，以了解集成的工作原理：

**Fastai v1**

*  ​ [对辛普森一家的人物进行分类](https://github.com/borisdayma/simpsons-fastai)：一个简单跟踪和比较Fastai模型的演示。
* ​ [用Fastai做语义分割](https://github.com/borisdayma/semantic-segmentation)：优化自动驾驶汽车的神经网络。‌

##  **选项** <a id="options"></a>

 `WandbCallback()`‌类支持许多选项：

| 关键字参数 | 默认值 | 说明 |
| :--- | :--- | :--- |
| learn | N/A | 要勾住的fast.ai learner。 |
| save\_model | True | 如果模型在每一步都有改进，就保存该模型。还会在训练结束后加载最优模型。 |
| mode | auto | 'min'、'max'或'auto'：如何在不同步（step）之间比较`monitor`指定的训练指标 |
| monitor | None | 用于评估性能的训练指标，以便于保存最佳模型。None默认为验证损失。 |
| log | gradients | gradients"、"parameters"、"all"或者None。损失和指标总是会被记录。 |
| input\_type | None | "images"或者None。用于展示样本预测。 |
| validation\_data | None | 若设置了input\_type， 则指定用于样本预测的数据 |
| predictions | 36 | 若设置了input\_type并且validation\_data为None，指定要进行预测的数量 |
| seed | 12345 | 若设置了input\_type并且validation\_data为None，则初始化样本预测的随机生成器。 |

[  
](https://docs.wandb.ai/integrations/fastai)

