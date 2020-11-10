---
description: 轻松地编写一段脚本，在你自己的项目中看看我们的实验跟踪功能和可视化功能。
---

# Quickstart

简单三步即可开始记录机器学习实验。

## 1. **安装库**

在Python3环境中安装我们的库。

```bash
pip install wandb
```

{% hint style="info" %}
如果你在自动环境中训练模型，应当看看文档《[在自动环境中运行](https://docs.wandb.com/library/environment-variables)》。自动的环境方便运行shell命令，比如谷歌的CloudML。
{% endhint %}

## 2. **创建账号**

在shell命令行或我们的注册页注册一个[免费账号](https://wandb.ai/login?signup=true)。

```bash
wandb login
```

## 3. **修改训练脚本**

在脚本中插入几行代码，用以记录超参数和指标。

{% hint style="info" %}
权阈是与框架无关的，但如果你用的是常见的机器学习框架，或许能找到对应框架的例子，就更容易上手。我们针对各种框架开发了对应的钩子（hook）,简化了集成过程，这些框架包括 [Keras](https://docs.wandb.com/frameworks/keras), [TensorFlow](https://docs.wandb.com/frameworks/tensorflow), [PyTorch](https://docs.wandb.com/frameworks/pytorch), [Fast.ai](https://docs.wandb.com/frameworks/fastai), [Scikit-learn](https://docs.wandb.com/frameworks/scikit), [XGBoost](https://docs.wandb.com/frameworks/xgboost), [Catalyst](https://docs.wandb.com/frameworks/catalyst), 和 [Jax](https://docs.wandb.com/frameworks/jax-example).
{% endhint %}

**初始化Wandb**

在脚本开头，紧接着导入（import）要初始化`wandb`

```python
# Inside my model training code
import wandb
wandb.init(project="my-project")
```

如果项目不存在，我们就自动为你创建项目。运行上方的训练脚本就会同步到名称为“my-project”的项目。要了解更多初始化选项，请查看[wandb.init](https://docs.wandb.com/library/init)文档。

**声明超参数**

用对象[wandb.config](https://docs.wandb.com/library/config)很容易保存超参数。

```python
wandb.config.dropout = 0.2
wandb.config.hidden_layer_size = 128
```

**记录指标**

在训练模型过程中记录指标，如损失（loss）和准确率（很多情况下，我们会提供特定框架的默认值）。用[wandb.log](https://docs.wandb.com/library/log)记录更加复杂的输出和结果，如直方图、图形和图像。

```python
def my_train_loop():
    for epoch in range(10):
        loss = 0 # change as appropriate :)
        wandb.log({'epoch': epoch, 'loss': loss})
```

**保存文件**

保存在路径`wandb.run.dir`的全部东西都会被上传到权阈，并在运行结束后与运行项保存在一起。这特别方便逐字地保存模型中的权值和阈值：

```python
# by default, this will save to a new subfolder for files associated
# with your run, created in wandb.run.dir (which is ./wandb by default)
wandb.save("mymodel.h5")

# you can pass the full path to the Keras model API
model.save(os.path.join(wandb.run.dir, "mymodel.h5"))
```

不错！现在正常运行脚本，我们会在一个后台进程中同步那些记录。你的最终输出、指标和文件将被同步到云端，如果你从git repo运行的话，还会同步你的git状态记录。

{% hint style="info" %}
如果你要做测试，想关闭wandb同步，就设置[环境变量](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MKaPhwzNIegNuInaekR/library/environment-variables)WANDB\_MODE=dryrun
{% endhint %}

## **下一步**

现在你让这个仪表运行起来了，下面简单概括了一些好功能：

1. **项目页**：可在项目指示板中比较多个不同实验。每次运行项目中的模型，都会在图表和表格中添加一行。在左侧栏中点击表格图标，即可展开表格并能看到所有超参数和指标。可以创建多个项目来组织运行项，并可以用表格向运行项添加标签和注释。
2. **自定义可视化**：为方便浏览结果，可添加平行坐标图、散点图及其他高级可视化工具。
3. **报告**：在动态图表和表格旁边添加一个Markdown面板，用以描述自己的研究成果。报告让你便捷地把项目简况分享给合作伙伴、教授或者老板。
4. **框架**：对于常见[框架](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MKaPhwzNIegNuInaekR/library/integrations)，我们有专门的集成，这些框架如PyTorch、Keras和XGBoost
5. **展示成果**：想要分享研究成果？我们一直在写论坛文章来展示我们社区的杰出成果。请发消息至[contact@wandb.com](mailto:contact@wandb.com)

### [**有疑问要联系我们→**](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MKaPhwzNIegNuInaekR/company/getting-help)\*\*\*\*

### [**参见OpenAI案例研究→**](https://wandb.ai/openai/published-work/Learning-Dexterity-End-to-End--VmlldzoxMTUyMDQ)\*\*\*\*

![](../.gitbook/assets/image%20%2891%29.png)

