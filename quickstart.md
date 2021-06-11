---
description: 轻松编写一段脚本，就可以在你自己的项目中查看我们的实验跟踪和可视化功能。
---

# Quickstart \(快速上手\)

简单三步即可开始记录机器学习实验。

## 1. **安装库**

在使用Python3的环境中安装我们的库。

```bash
pip install wandb
```

{% hint style="info" %}
 如果你在不方便运行shell命令的自动化环境中训练模型，比如谷歌的CloudML，你应当查看一下我们的[环境变量](https://docs.wandb.ai/v/zh-hans/library/environment-variables)。
{% endhint %}

## 2. **创建账号**

在你的shell命令行中或到我们的注册页注册一个[免费账号](https://wandb.ai/login?signup=true)。

```bash
wandb login
```

## 3. **修改你的训练脚本**

在你的脚本中插入几行代码，用以记录超参数和指标（Metric）。

{% hint style="info" %}
权重（Weights）和偏差\(Biases\)是框架无关的，但如果你用的是常见机器学习框架，你可能会发现特定框架的例子更容易上手。我们针对特定框架开发了对应的钩子（hook）,以简化集成，这些框架包括 [Keras](https://docs.wandb.ai/v/zh-hans/integrations/keras), [TensorFlow](https://docs.wandb.ai/v/zh-hans/integrations/tensorflow), [PyTorch](https://docs.wandb.ai/v/zh-hans/integrations/pytorch), [Fast.ai](https://docs.wandb.ai/v/zh-hans/integrations/fast.ai), [Scikit-learn](https://docs.wandb.ai/v/zh-hans/integrations/scikit), [XGBoost](https://docs.wandb.ai/v/zh-hans/integrations/xgboost), [Catalyst](https://docs.wandb.ai/v/zh-hans/integrations/catalyst), 和 [Jax](https://docs.wandb.ai/v/zh-hans/integrations/jax-example).
{% endhint %}

**初始化W&B**

开始记录之前，在你的脚本开始处初始化`wandb。有些集成，比如我们的 Hugging Face 集成，内部包含wandb.init()。`

```python
# Inside my model training code
import wandb
wandb.init(project="my-project")
```

如果项目不存在，我们会自动为你创建项目。上方训练脚本的运行会同步到一个名称为“my-project”的项目中。要了解更多初始化选项，请查看[wandb.init](https://docs.wandb.ai/v/zh-hans/library/wandb.init)文档。

**声明超参数**

用对象[wandb.config](https://docs.wandb.ai/v/zh-hans/library/wandb.config)保存超参数很容易。

```python
wandb.config.dropout = 0.2
wandb.config.hidden_layer_size = 128
```

**记录指标（Metric）**

在训练模型过程中记录指标（Metric），如损失（Loss）和准确率（Accuracy）（很多情况下，我们会提供特定框架的默认值）。用[wandb.log](https://docs.wandb.ai/v/zh-hans/library/wandb.log)记录更加复杂的输出和结果，如直方图、图形和图像。

```python
def my_train_loop():
    for epoch in range(10):
        loss = 0 # change as appropriate :)
        wandb.log({'epoch': epoch, 'loss': loss})
```

**保存文件**

保存在路径`wandb.run.d`中的全部内容都会被上传到W&B，并在运行结束后与你的运行项保存在一起。这对于保存你模型中的文字权重（Weight）和偏差（Bias）特别方便：

```python
# by default, this will save to a new subfolder for files associated
# with your run, created in wandb.run.dir (which is ./wandb by default)
wandb.save("mymodel.h5")

# you can pass the full path to the Keras model API
model.save(os.path.join(wandb.run.dir, "mymodel.h5"))
```

很好！现在正常运行你的脚本，我们会在一个后台进程中同步那些记录。你的终端输出、指标（Metric）和文件将被同步到云端，如果你从一个git 库中运行的话，还会同步你的git状态记录。

{% hint style="info" %}
 如果你是在做测试，想禁用wandb同步，可以设置[环境变量](https://docs.wandb.ai/v/zh-hans/library/environment-variables)WANDB\_MODE=dryrun
{% endhint %}

## **下一步**

现在你已经让这个仪表运行起来了，下面是一些很酷的功能概述：

1.  **项目页**：在一个项目仪表盘上比较很多不同的实验。每次运行项目中的一个模型，都会在图形和表格中添加一行。在左侧栏中点击表格图标，即可展开表格并能看到所有超参数和指标（Metric）。可以创建多个项目来组织你的运行，并用表格为你的运行添加标签和注释。
2.      **自定义可视化**：为方便探究结果，可添加平行坐标图、散点图及其他高级可视化工具。
3. **报告**：在你的实时图形和表格旁边添加一个Markdown面板，用以描述自己的研究成果。报告让你可以便捷地把项目快照分享给伙伴、教授或者老板
4. **集成**：对于流行框架如PyTorch、Keras和XGBoost，我们有专用集成。
5. **展示成果**：想要分享你的研究成果？我们一直在发布博客来展示我们社区的杰出成果。请发消息至[contact@wandb.com](mailto:contact@wandb.com)​

### \*\*\*\*[**有疑问要联系我们→**](https://docs.wandb.ai/v/zh-hans/company/getting-help)\*\*\*\*

### [**参见OpenAI案例研究→**](https://wandb.ai/openai/published-work/Learning-Dexterity-End-to-End--VmlldzoxMTUyMDQ)\*\*\*\*

![](.gitbook/assets/image%20%2891%29.png)

