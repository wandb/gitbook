---
description: 如果您已经在项目中使用wandb.init，wandb.config和wandb.log，请从这里开始！
---

# Sweep from an existing project

如果您有现成的W＆B项目，则可以轻松地通过超参数扫描来优化模型。我将通过一个工作示例逐步介绍步骤——您可以打开 [W&B Dashboard](https://app.wandb.ai/carey/pytorch-cnn-fashion)。我正在使用此示例存储[库中的代码](https://github.com/wandb/examples/tree/master/examples/pytorch/pytorch-cnn-fashion)，该代码训练了PyTorch卷积神经网络来对[Fashion MNIST数据集](https://github.com/zalandoresearch/fashion-mnist)中的图像进行分类。

## 1. **创建一个项目**

手动运行您的第一个基准运行，以检查W＆B日志是否正常运行。您将下载此简单的示例模型，对其进行训练几分钟，然后在Web仪表板中看到该示例。

* • 克隆此库`git clone https://github.com/wandb/examples.git`
* • 打开此示例
* `cd examples/pytorch/pytorch-cnn-fashion`
* • 手动运行 `python train.py`

  [查看示例项目页面→](https://wandb.ai/carey/pytorch-cnn-fashion)

## 2. **创建扫描**

在项目页面中，打开侧栏中的“扫描”选项卡，然后单击“创建扫描”。

![](../../.gitbook/assets/sweep1.png)

自动生成的配置会根据您已经完成的运行猜测要扫描的值。编辑配置以指定要尝试的超参数范围。当您启动扫描时，它将在我们的W＆B扫描服务器上启动一个新进程。这种集中式服务可以协调代理程序——您正在运行训练作业的机器。

![](../../.gitbook/assets/sweep2.png)

### **3.启动代理**

接下来，在本地启动代理。如果您想分发工作并更快地完成扫描，则可以在不同的计算机上并行启动数十个代理。该代理将打印出下一个要尝试的参数集。

![](../../.gitbook/assets/sweep3.png)

就是这样简单！现在，您正在进行一次扫描。这是我的示例扫描开始时的仪表板。[查看示例项目页面→](https://wandb.ai/carey/pytorch-cnn-fashion)

![](https://paper-attachments.dropbox.com/s_5D8914551A6C0AABCD5718091305DD3B64FFBA192205DD7B3C90EC93F4002090_1579066494222_image.png)

### **使用现有运行开始新的扫描**

使用先前记录的已有运行启动新扫描。

1. 打开您的项目表。
2. 选择要与表格左侧复选框一起使用的运行。
3. 单击下拉列表以创建新的扫描。

现在，您的扫描将在我们的服务器上进行设置。您需要做的就是启动一个或多个代理以开始运行。

![](../../.gitbook/assets/create-sweep-from-table%20%281%29.png)

