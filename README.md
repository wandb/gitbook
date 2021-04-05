---
description: 跟踪机器学习实验，可视化指标(metric)并分享结果。
---

# Weights & Biases

权重（Weights）&偏差（Biases）可以帮助跟踪你的机器学习项目。使用我们的工具记录运行中的超参数和输出指标\(Metric\)，然后对结果进行可视化和比较，并快速与同事分享你的发现。

![](.gitbook/assets/image.jpeg)

我们的工具可以在这些机器学习基础设施上运行：亚马逊AWS、谷歌云、Kubernetes、微软Azure和on-prem机器。

**工具**

1. [**仪表盘**](https://docs.wandb.com/app)：跟踪实验、可视化结果。
2. \*\*\*\*[**报告**](https://docs.wandb.com/reports)：保存和分享可复制的成果/结论。
3. [**扫描**](https://docs.wandb.com/sweeps)（Sweeps）：通过调节超参数来优化模型
4. \*\*\*\*[**制品（Artifacts）**](https://docs.wandb.com/artifacts): 数据集和模型版本化，流水线跟踪。

{% embed url="https://youtu.be/gnD8BFuyVUA" caption="" %}

**入门指南**

简单将我们的Python库`wandb`添加到你的机器学习脚本。

*  [快速上手](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MKaPhwzNIegNuInaekR/quickstart)
* [集成Keras](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MKaPhwzNIegNuInaekR/library/integrations/keras)
* [集成PyTorch ](%20https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MKaPhwzNIegNuInaekR/library/integrations/pytorch)   
*  [集成TensorFlow](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MKaPhwzNIegNuInaekR/library/integrations/tensorflow)
* [集成 Jupyter Notebook ](%20https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MKaPhwzNIegNuInaekR/library/integrations/jupyter)

 下面的截图示例来自W&B中的一个[物种鉴别项目](https://wandb.ai/stacey/curr_learn/reports?view=stacey/Species%20Identification)。

![](.gitbook/assets/screen-shot-2020-08-07-at-1.16.16-pm.png)

**示例**

如果你对示例项目感兴趣，我们有些资源：

*  [应用库](https://wandb.ai/gallery)：我们web应用中的一个功能报告库
*  [示例项目](https://docs.wandb.com/examples)：Github和Colab中的代码和项目

