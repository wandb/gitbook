---
description: 概述我们的客户端库
---

# Python Library

利用我们的Python库训练你的机器学习模型以及跟踪实验过程。搭建过程仅需几行代码。如果你用的是常见框架，我们有很多专用的集成，让搭建wandb轻而易举。

我们有从代码生成的更详尽文档，见于[参考](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MKaPhwzNIegNuInaekR/library/reference)。

### **训练模型**

* wandb.init — 在训练脚本开头初始化一个新的运行项；
* wandb.config — 跟踪超参数；
* wandb.log — 在训练循环中持续记录变化的指标；
* wandb.save — 保存运行项相关文件，如模型权值；
* wandb.restore — 运行指定运行项时，恢复代码状态。

{% page-ref page="../../library/init.md" %}

{% page-ref page="../../library/log.md" %}

{% page-ref page="../../library/config.md" %}

{% page-ref page="../../library/save.md" %}

{% page-ref page="../../library/restore.md" %}

### **将会上传哪些东西？**

脚本记录的全部数据都保存在本地机器上，位于一个**wandb**路径，然后同步到云端。

**自动记录**

* **系统指标**：处理器和GPU使用率、网络等。由命令[nvidia-smi](https://developer.nvidia.com/nvidia-system-management-interface)得出这些指标，这些指标位于运行页“系统”选项卡。
* **命令行**：记录标准输出和标准错误，并显示于运行页“日志”选项卡。
* **git提交**：记录最近的git提交，并显示于运行页“概况”选项卡。
* **文件**：requirements.txt文件，以及用于运行项并保存在**wandb**路径的全部文件，将被上传并显示于运行页“文件”选项卡。

**有明确调用才记录**

当涉及数据和模型指标时，你可以明确决定要记录哪些东西。

* **数据集**：你必须明确记录图像或其它数据集样本，这样才能保存到权阈。
* **PyTorch梯度**：加入wandb.watch\(模型\)，即可在界面中看到权值的梯度直方图。
* **配置（config）**：记录超参数、数据集链接以及所使用的架构名称，并作为config的参数，传值方式如下：wandb.init\(config=your\_config\_dictionary\)
* **指标**：用wandb.log\(\)记录模型的参数。如果你记录的是训练循环内的指标，如准确率、损失，就可以在界面中看到实时更新的图表。

‌

### **常见问题**

‌

**一台机器上有多个权阈用户**

‌

如果你与别人共用一台机器，机器上还有别的权阈用户，要确保自己的运行项永远都记录到对应账号中，做起来不难。把环境变量WANDB\_API\_KEY设置为“验证”（authenticate）。如果你在自己的环境中设置了，当你登录后会得到正确的密码信息，你还可以在脚本中设置那个环境变量。

‌ 运行该命令`export WANDB_API_KEY=X`，X是你的API密钥。当你登录后，可以在[wandb.ai/authorize](https://app.wandb.ai/authorize)找到自己的API密钥。

**组织最佳实践**

我们提供的工具非常灵活、可定制。你可以随意使用我们的工具，但在使用上有一些指导原则

下面这个例子是建立一个运行项：

```text
import wandb​config = dict (  learning_rate = 0.01,  momentum = 0.2,  architecture = "CNN",  dataset_id = "peds-0192",  infra = "AWS",)​wandb.init(  project="detect-pedestrians",  notes="tweak baseline",  tags=["baseline", "paper1"],  config=config,)
```

‌

**Suggested usage**‌

1. **配置（config）**：跟踪超参数、架构、数据集及其它你要用来重新训练模型的所有东西。这些数据按列呈现——用config列对同一应用中的运行项自动分组、分类和筛选。
2. **项目**：一个项目就是可以放一起比较的一组实验。每个项目有一个专门的指示板页，你可以轻松地按组打开、关闭各个组的运行项，以便于比较不同的模型版本。
3. **注释**：提交给自己的简短消息，可在脚本中设置注释，并可在表格中编辑。我们建议用注释栏，就不必重写生成的运行项名称。
4. **标签**：鉴别基准运行项和最喜欢的运行项。你可以用标签筛选运行项，标签可在表格中编辑。

‌

#### **.log\(\)与.summary\(\)有什么区别？**

summary（总结）是表格中显示的值，而log（记录）会保存后续绘图用到的所有值。

比如说，每当准确率发生变化，你就想调用`wandb.log`。一般情况下你可以只用.log，除非你把指标的summary设置为手动，默认情况下`wandb.log()`也会更新summary的值。

我们同时有这两个，是因为一些人喜欢把summary设置为手动，他们想让summary反映比方说最佳准确率，而不是所记录的最后一个准确率。

## FAQ <a id="faq"></a>

‌

### How do I install the wandb Python library in environments without gcc? <a id="how-do-i-install-the-wandb-python-library-in-environments-without-gcc"></a>

‌

If you try to install `wandb` and see this error:

```text
unable to execute 'gcc': No such file or directoryerror: command 'gcc' failed with exit status 1
```

‌

You can install psutil directly from a pre-built wheel. Find your Python version and OS here: [https://pywharf.github.io/pywharf-pkg-repo/psutil](https://pywharf.github.io/pywharf-pkg-repo/psutil)‌

For example, to install psutil on python 3.8 in linux:

```text
pip install https://github.com/pywharf/pywharf-pkg-repo/releases/download/psutil-5.7.0-cp38-cp38-manylinux2010_x86_64.whl/psutil-5.7.0-cp38-cp38-manylinux2010_x86_64.whl#sha256=adc36dabdff0b9a4c84821ef5ce45848f30b8a01a1d5806316e068b5fd669c6d
```

‌

After psutil has been installed, you can install wandb with `pip install wandb`

