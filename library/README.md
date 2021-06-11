---
description: Weights&Biases用于实验跟踪、数据集版本化和模型管理
---

# Library \(库\)

使用`wandb` Python库跟踪机器学习实验，只需几行代码。如果你用的是[PyTorch](../integrations/pytorch.md) 或 [Keras](../integrations/keras.md) 等流行框架，我们有轻量级[集成](https://docs.wandb.ai/v/zh-hans/integrations)

##  **在你的脚本中集成W&B**

以下是使用 W&B 跟踪实验的简单构件。我们还为[PyTorch](../integrations/pytorch.md), [Keras](../integrations/keras.md), [Scikit](../integrations/scikit.md)等提供了大量特殊集成。参见 [集成](https://docs.wandb.ai/v/zh-hans/integrations)。

1.  [wandb.init\(\)](https://docs.wandb.ai/v/zh-hans/library/wandb.init): 在你的脚本顶部初始化一个新的运行。这将返回一个Run 对象，并创建一个本地目录，所有的日志和文件都保存在该目录下，然后异步流式地传输到 W&B服务器。如果你想要使用私人服务器而不是我们的托管云服务器，我们提供了[自托管（Self-Hosting）](https://docs.wandb.ai/v/zh-hans/self-hosted)选项。
2. [ wandb.config](https://docs.wandb.ai/v/zh-hans/library/wandb.config): 保存超参数字典，如，学习率或模型类型。你在配置（config）中捕获的模型设置在以后组织和查询结果时非常有用。
3. [wandb.log\(\)](https://docs.wandb.ai/v/zh-hans/library/wandb.log): 在训练循环中随时间记录指标（Metric）,如准确率（Accuracy）和损失（Loss）。默认情况下，当你调用wandb.log\(\),它会添加一个新步（Step）到历史对象中并更新总结（Summary）对象。
   *  **历史（history）**:一个类似字典的对象数组，用于随时间推移跟踪指标（Metric）。这些时间序列值在UI中显示为默认的线图。
   * **总结（summary）**:默认情况下，用wandb.log\(\)记录指标（Metric）的最终值。你可以手动为一个指标（Metric）设置总结（Summary）,以捕获最高准确率（Accuracy）或最低损失（Loss）而不是最终值。这些值在比较运行的表格和图中会使用--例如，你可以可视化你项目中所有运行的最终准确率（Accuracy）。
4.   ****[制品（Artifacts）](https://docs.wandb.ai/v/zh-hans/artifacts):保存运行的输出，如模型权重（Weight）或预测表。这让你不仅可以跟踪模型训练，还可以跟踪影响最终模型的所有流水线步骤。

##  **最佳实践**

 `wandb` 库非常灵活，下面是一些建议指南。

1. **配置（Config）**:  跟踪超参数、架构、数据集和任何其他你想要用来重现你的模型的东西。这些将显示在列中—使用配置列在应用中动态地分组、排序和过滤运行。
2.   **项目（Project）**: 一个项目是你可以一起比较的一组实验集合。每个项目都有独立的仪表盘，你可以轻松地打开或关闭不同的运行组来比较不同的模型版本。
3.  **注释（Notes）**: 给你自己的一个快速注解消息，该注释可以从你脚本中设置，并且可以在表格中编辑。
4. **标签（Tags）**: 识别基准运行和收藏夹运行。你可以使用标签过滤运行，他们在表中是可编辑的。

```python
import wandb

config = dict (
  learning_rate = 0.01,
  momentum = 0.2,
  architecture = "CNN",
  dataset_id = "peds-0192",
  infra = "AWS",
)

wandb.init(
  project="detect-pedestrians",
  notes="tweak baseline",
  tags=["baseline", "paper1"],
  config=config,
)
```

##  **什么数据会被记录？**

 你的脚本记录的全部数据都被保存在本地机器的一个**wandb**路径下，然后同步到云端或你的[私人服务器](https://docs.wandb.ai/v/zh-hans/self-hosted)。

###  **自动记录**

* **系统指标（Metric）**：CPU和GPU使用率、网络等。由命令[nvidia-smi](https://developer.nvidia.com/nvidia-system-management-interface)获取，显示在运行页“系统（System）”选项卡中。
* **命令行**：提取标准输出和标准错误，并显示于运行页“日志（logs）”选项卡中。

 在你的[设置页面](https://wandb.ai/settings)中打开代码保存功能以获取:

* **Git commit**: 提取最近的git提交，并显示于运行页“概览（overview）”选项卡以及如果有未提交的修改还会显示一个 diff.patch 文件。
* **文件（Files）**: requirements.txt文件，以及用于运行你保存在**wandb**路径的全部文件，将被上传并显示于运行页“文件（Files）”选项卡。

###  **通过特定调用记录**

当涉及数据和模型指标时，你可以决定要记录哪些东西。

* **数据集**：你必须明确记录图像或其它数据集样本，这样才能保存到W&B。
* **PyTorch梯度**：添加wandb.watch\(model\)，即可在界面中看到权值（Weight）的梯度直方图。
* **配置（config）**：记录超参数、数据集链接或所使用的架构名称，并作为配置（config）参数，传值方式如下：wandb.init\(config=your\_config\_dictionary\)
* **指标（Metrics）**：用wandb.log\(\)记录你的模型指标。如果你从你的训练循环内部记录如准确率（Accuracy）、损失（Loss）等指标，，就可以在界面中看到实时更新图。

### 

