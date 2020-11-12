# fastai v1

提示：这篇文档专用于Fastai v1。如果你用的是Fastai最新版，请参考[Fastai页面](https://app.gitbook.com/@weights-and-biases/s/docs/library/integrations/fastai)。

 对于Fastai v1的脚本，我们有一个回调函数自动记录模型的拓扑、损失、指标、权值、梯度、样本预测以及最佳训练模型。

```text
import wandbfrom wandb.fastai import WandbCallback​wandb.init()​learn = cnn_learner(data,                    model,                    callback_fns=WandbCallback)learn.fit(epochs)
```

所需的记录数据可通过回调构造函数进行配置。

```text
from functools import partial​learn = cnn_learner(data, model, callback_fns=partial(WandbCallback, input_type='images'))
```

也可以只在开始训练时使用WandbCallback。在这种情况下，它必须实例化。

```text
learn.fit(epochs, callbacks=WandbCallback(learn))
```

 自定义参数还可在这一阶段给定。

```text
learn.fit(epochs, callbacks=WandbCallback(learn, input_type='images'))
```

‌

##  **示例代码** <a id="example-code"></a>

我们准备了几个例子，让大家看看集成的效果：

**Fastai v1**‌

* ​ [辛普森字符分类](https://github.com/borisdayma/simpsons-fastai)：简单演示如何跟踪Fastai模型并加以比较。
* ​ [用Fastai做语义分割](https://github.com/borisdayma/semantic-segmentation)：优化自动驾驶神经网络。

‌

## **选项** <a id="options"></a>

‌类`WandbCallback()`支持多个选项：

| 关键字参数 | 默认值 | 说明 |
| :--- | :--- | :--- |
| learn | N/A | 要勾住的fast.ai learner。 |
| save\_model | True | 如果模型在每一步都被批准了，就保存该模型。还会在训练末尾加载最优模型。 |
| mode | auto | 'min'、'max'或'auto'：如何在不同时间步之间比较`monitor`指定的训练指标。 |
| monitor | None | 用于评估性能的训练指标，以便于保存最佳模型。None默认为验证损失。 |
| log | gradients | gradients"、"parameters"、"all"或者None。始终记录损失和指标。 |
| input\_type | None | "images"或者None。用于展示样本预测。 |
| validation\_data | None | 若设置了input\_type，指定样本预测所用的数据。 |
| predictions | 36 | 若设置了input\_type并且validation\_data为None，指定要做多少次预测。 |
| seed | 12345 |  若设置了input\_type并且validation\_data为None，为样本预测初始化随机生成器。 |

