# PyTorch Lightning

 PyTorch Lightning提供了一个轻量级的封装器，用来组织你的PyTorch代码以及轻松地添加高级功能，如[分布式训练](https://pytorch-lightning.readthedocs.io/en/latest/multi_gpu.html)、[16位精确度](https://pytorch-lightning.readthedocs.io/en/latest/amp.html)。W&B也提供了一个轻量级的封装器，用来记录你的机器学习（ML）实验。我们直接合并到了PyTorch Lightning库，所以大家可随时查看[他们的文档](https://pytorch-lightning.readthedocs.io/en/latest/loggers.html#weights-and-biases)。

## ⚡**仅需两行代码即可快速上手**

```python
from pytorch_lightning.loggers import WandbLogger
from pytorch_lightning import Trainer

wandb_logger = WandbLogger()
trainer = Trainer(logger=wandb_logger)
```

## ✅ **查看实例！**

我们已经为你创建了一些示例，以帮助你了解集成的工作原理：

*  ​ [Google Colab中的](https://colab.research.google.com/drive/16d1uctGaw2y9KhGBlINNTsWpmlXdJwRW?usp=sharing)一个使用超参数优化演示示例。
*  [教程](https://colab.research.google.com/github/wandb/examples/blob/master/colabs/pytorch-lightning/Supercharge_your_Training_with_Pytorch_Lightning_%2B_Weights_%26_Biases.ipynb): 使用Pytorch Lightning + Weights&Biases增强你的训练
*  ​[用Lightning做语义分割](https://wandb.ai/borisd13/lightning-kitti/reports/Lightning-Kitti--Vmlldzo3MTcyMw)：优化自动驾驶汽车的神经网络。
*  跟踪Lightning模型性能的[步骤指南](https://app.wandb.ai/cayush/pytorchlightning/reports/Use-Pytorch-Lightning-with-Weights-%26-Biases--Vmlldzo2NjQ1Mw) ​

## **💻 API 참조**

### `WandbLogger`

可选参数:

*   **name**（字符串型）——显示运行的名称。
*  **save\_dir**（字符串型）——数据保存的路径（默认为wandb目录）。
* **offline**（布尔型）——离线运行（数据可以稍后流式传输到wandb服务器）。
*  **id**（字符串型）——设置版本，主要用来做断点续训。
* **version**（字符串型）——与版本相同（遗留）
* **anonymous**（布尔型）——启用或显式禁用匿名记录。
* **project**（字符串型）——本运行所属的项目名称。
*  **log\_model**（布尔型）—— 将检查点保存在wandb目录中，并上传到W&B服务器。
*  **prefix**（字符串型）——放在指标（metric）键开头的字符串。
* **sync\_step**（布尔型）——将训练步（step）与wandb步（step）同步（默认为True）。
* **\*\*kwargs**—— wandb.init使用的其他参数（如,entity,group,tags等），可作为关键字参数传入到此记录器中。

### **`WandbLogger.watch`**

记录模型拓扑以及可选的梯度和权重。

```python
wandb_logger.watch(model, log='gradients', log_freq=100)
```

参数：

* **model** \(_nn.Module_\) – 要记录的模型。
* **log**（字符串型）——"parameters"、"all"或None，默认值为"gradients"。
* **log\_freq**（整型）——隔多少步数记录一次梯度和参数（默认为100）。

### **`WandbLogger.log_hyperparams`**

记录超参数配置。

注意：当使用`LightningModule.save_hyperparameters()时此函数会被自动调用`

```python
wandb_logger.log_hyperparams(params)
```

 参数：

* **params（字典型）——字典型数据，超参数名称作为“键”，配置值作为“值”。**

### `WandbLogger.log_metrics`

 记录训练指标。

注意：此函数由`LightningModule.log('metric', value)`自动调用。

```python
wandb_logger.log_metrics(metrics, step=None)
```

 **参数:**

* **metric**（数字型）——字典型数据，指标名称作为“键”，测试量作为“值”。
* **step**（整数型或None）——要在哪个步（step）记录指标。

\*\*\*\*

