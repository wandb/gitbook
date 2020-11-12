---
description: 用权阈可视化PyTorch Lightning模型。
---

# PyTorch Lightning

PyTorch Lightning提供了一个轻量级的封装器，用来组织PyTorch代码以及轻松地添加高级功能，如分布式训练、[16位精确度](https://pytorch-lightning.readthedocs.io/en/latest/amp.html)。权阈也提供了一个轻量级的封装器，用来记录机器学习实验。我们直接合并到了PyTorch Lightning库，所以大家可随时查看他们的说明文档。

##  **⚡仅需两行代码就变得疾如闪电**

```python
from pytorch_lightning.loggers import WandbLogger
from pytorch_lightning import Trainer

wandb_logger = WandbLogger()
trainer = Trainer(logger=wandb_logger)
```

##  **✅查看实例！**

我们准备了几个例子，让大家看看集成的效果：

*  [用谷歌的Colab运行](https://colab.research.google.com/github/wandb/examples/blob/master/colabs/pytorch-lightning/Supercharge_your_Training_with_Pytorch_Lightning_%2B_Weights_%26_Biases.ipynb)，在一个简单的笔记本中体验集成效果。
*  [一步步教你](https://wandb.ai/cayush/pytorchlightning/reports/Use-Pytorch-Lightning-with-Weights-&-Biases--Vmlldzo2NjQ1Mw)跟踪闪电（Lightning）模型的表现
* [用闪电做语义分割](https://wandb.ai/borisd13/lightning-kitti/reports/Lightning-Kitti--Vmlldzo3MTcyMw)：优化自动驾驶神经网络。

##  **💻 API参考**

### `WandbLogger`

参数:

*  **name**（字符串型）——显示运行项的名称。
*  **save\_dir**（字符串型）——数据保存的路径。
* **offline**（布尔型）——是否离线运行（以后再把数据上传至权阈服务器）。
*  **version**（id）——设置版本，主要用来做断点续训。
* **anonymous**（布尔型）——启用或显式禁用匿名记录。
* **project**（字符串型）——本运行项所属的项目名称。
* **tags**（字符串列表型）——与本运行项有关的标签。

### **`WandbLogger.watch`**

记录模型拓扑，可选择记录梯度和权值。

```python
wandb_logger.watch(model, log='gradients', log_freq=100)
```

 参数：

* **model**（nn.Module）——要记录的模型。
* **log**（字符串型）——"parameters"、"all"或None，默认值为"gradients"。
* **log\_freq**（整型）——隔多少步数记录一次梯度和参数。

### **`WandbLogger.log_hyperparams`**

 记录超参数配置。

注意：`Trainer`自动调用该函数。

```python
wandb_logger.log_hyperparams(params)
```

参数：

*  **params（**字典型）——字典型数据，超参数名称为“键”，设置值为“值”。

### `WandbLogger.log_metrics`

记录训练指标。

注意：`Trainer`自动调用该函数。

```python
wandb_logger.log_metrics(metrics, step=None)
```

 参数:

* **metric**（数字型）——字典型数据，指标名称为“键”，测试量为“值”。
* **step**（整数型或None）——要在哪个时间步记录指标。

\*\*\*\*

