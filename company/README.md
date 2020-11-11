# Company

**什么是wandb？**

Wandb是用于机器学习的实验跟踪工具。 我们使从事机器学习的任何人都可以轻松跟踪实验，与同事及未来的我们分享结果。

这是1分钟的概述视频。 [查看示例项目→](https://app.wandb.ai/stacey/estuary)

{% embed url="https://www.youtube.com/watch?v=icy3XkZ5jBk" caption="" %}

**它是如何工作的？**

当您使用wandb测试训练代码时，我们的后台进程将收集有关训练模型时正在发生的事情的有用数据。 例如，我们可以跟踪模型性能指标，超参数，渐变，系统指标，输出文件以及您最近的git commit。

{% page-ref page="../ref/export-api/examples.md" %}

###  设置wanb难吗？

我们知道大多数人会使用emacs和Google表格等工具跟踪他们的训练，因此我们将wandb设计为尽可能轻巧。 集成过程需要5到10分钟，而wandb不会减慢速度或使您的训练脚本崩溃。

##  **使用wandb的好处**

我们的用户告诉我们，他们从wandb中获得了三种好处：

### **1.可视化训练**

我们的一些用户将wandb视为“persistent TensorBoard”。默认情况下，我们收集模型性能指标，例如准确性和损失。我们还可以收集并显示matplotlib对象，模型文件，系统指标（如GPU使用情况）以及您最近的git commit SHA +补丁文件，该文件包含自上次提交以来的所有更改。

您还可以记下要保存的个人训练记录以及训练数据。这是我们给彭博Bloomberg的一堂课中的一个相关[示例项目](https://app.wandb.ai/bloomberg-class/imdb-classifier/runs/2tc2fm99/overview)。

###  2.组织和比较大量的训练

大多数训练机器学习模型的人都在尝试他们模型的很多版本，我们的目标是帮助人们保持组织有序。

您可以创建项目以将所有运行都放在一个位置。您可以跨多个运行可视化性能指标，并根据需要过滤，分组和标记它们。

一个很好的例子项目是Stacey的[河口项目](https://wandb.ai/stacey/estuary)。在边栏中，您可以打开和关闭训练以显示在图表上，或者单击一个训练以进行更深入的研究。您所有的运行都将保存并组织在一个统一的工作区中。

![](../.gitbook/assets/image%20%2884%29.png)

### **3.分享您的结果**

一旦完成了许多运行，通常就需要组织它们以显示某种结果。我们在Latent Space的朋友写了一篇很好的文章，名为《 ML最佳实践：测试驱动开发》\([ML Best Practices: Test Driven Development](https://www.wandb.com/articles/ml-best-practices-test-driven-development)\)，讨论了他们如何使用W＆B报告来提高团队的生产力。

用户Boris Dayma撰写了有关语义细分\([Semantic Segmentation](https://wandb.ai/borisd13/semantic-segmentation/reports?view=borisd13%2FSemantic%20Segmentation%20Report)\)的公开示例报告。他介绍了他尝试过的各种方法以及它们的工作效果。

我们真的希望wandb鼓励机器学习团队更有效地协作。

如果您想了解有关团队如何使用wandb的更多信息，我们已经记录了对[OpenAI](https://www.wandb.com/articles/why-experiment-tracking-is-crucial-to-openai)和[Toyota Research](https://www.youtube.com/watch?v=CaQCw-DKiO8)的技术用户的采访。

## **团队**

如果您正在与合作者一起进行机器学习项目，那么我们可以轻松共享结果。

* [企业团队](https://www.wandb.com/pricing)：我们支持小型创业公司和大型企业团队，例如OpenAI和Toyota Research Institute。我们提供灵活的定价选项来满足您团队的需求，并且我们支持托管云，私有云和本地安装。
* [学术团队](https://www.wandb.com/academic)：我们致力于支持学术透明和合作研究。如果您是学者，我们将授予您访问免费小组的机会，以在私人项目中分享您的研究成果。

如果您想与团队以外的人共享项目，请在导航栏中单击项目隐私设置，然后将项目设置为“公开”。与您共享链接的任何人都可以看到您的公共项目结果。

