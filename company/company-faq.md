# Company FAQ

###  定价和账户计划

W＆B对个人和学术团队免费。我们致力于免费提供支持，以支持机器学习的发展，并且我们可以使用[Export API.](https://app.gitbook.com/@weights-and-biases/s/docs/ref/export-api)轻松导出数据。

* [ 定价页面](https://www.wandb.com/pricing)：个人，初创企业，团队和企业的帐户
*  [学术人员](https://www.wandb.com/academic)：申请访问学术团队以与合作者共享结果
*  [本地](https://app.gitbook.com/@weights-and-biases/s/docs/self-hosted)：我们提供本地和私有云安装选项
*  [联系人](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MLvV8HPbd9J_6haqztg/v/ch/company/getting-help)：有任何问题请与我们联系
*  [申请演示](https://www.wandb.com/contact)：我们很高兴与您通话并讨论自定义企业计划

### **权重和偏差与TensorBoard有何不同？**

 我们致力于为所有人改进实验跟踪工具。当联合创始人开始从事W＆B时，他们受到启发为OpenAI的沮丧的TensorBoard用户构建工具。以下是我们重点改进的一些内容：

1. 复制模型：“权阈”非常适合以后进行实验，探索和复制模型。我们不仅捕获指标，还捕获代码的超参数和版本，并且我们可以为您保存模型检查点，以便您的项目可重现。
2. . 自动组织：如果您将项目移交给别的协作者或您将去度假，W＆B可以轻松查看您尝试过的所有模型，因此您不会浪费大量时间来重新运行旧的实验。
3. 快速、灵活的集成：在5分钟内将W＆B添加到您的项目中。安装我们的免费开源Python软件包，然后代码中添加几行，每次运行模型时，您都会获得记录良好的指标和试验记录。
4.  持久的集中式仪表板\(dashboard\)：无论您是在本地计算机，实验室集群还是在云中定位实例的任何地方训练模型，我们都会为您提供相同的集中式仪表板。您无需花费时间从其他机器复制和组织TensorBoard文件。
5. 强大的表格：搜索，过滤，排序和分组来自不同模型的结果。您可以轻松查看数千个模型版本，并为不同任务找到性能最佳的模型。 而TensorBoard不适用于大型项目。
6.  协作工具：使用W＆B来组织复杂的机器学习项目。把链接共享到W＆B很容易，并且您可以使用私人团队让所有人将结果发送到共享项目。我们还支持通过报告进行协作——添加交互式可视化并以markdown描述您的工作。这是保存工作日志，与主管共享发现或向实验室展示发现的好方法。

开始使[用免费的个人帐户→](http://app.wandb.ai/)

**谁拥有这些数据？**

 您可以随时导出和删除数据。我们绝不会共享与私人项目相关的数据。我们希望您可以公开自己的作品，以便其他从业者可以学习。

 我们希望探索、共享高级模式，以推动机器学习领域的发展。例如，我们写了[这篇文章](https://www.wandb.com/articles/monitor-improve-gpu-usage-for-model-training)，介绍人们如何无法充分利用他们的GPU。我们希望以此方式尊重您的隐私。如果您对数据隐私有任何疑问，我们很乐意收到您的来信。请通过contact@wandb.com与我们联系。

###  **为什么要构建这些工具？**

在Weights＆Biases，我们的使命是为机器学习构建最佳工具。我们经验丰富的技术联合创始人创建了图8，我们的工具正在被OpenAI和Toyota等尖端的机器学习团队使用。我们喜欢制作有用的工具，而我们最开心的就是与正在使用我们的产品构建真实模型的人们进行交互。

![](../.gitbook/assets/image%20%2856%29.png)

###  我如何拼读“ wandb”？

您可以将其发音为w-and-b（如我们最初的意图），wand-b（因为它像魔杖一样神奇）或wan-db（因为它可以保存数据库之类的东西）。

###  **如何在论文中引用“权阈”？**

 我们写了一份[白皮书](https://www.dropbox.com/s/0ipub9ewwkml8jf/Experiment%20Tracking%20with%20Weights%20%26%20Biases.pdf?dl=1)，如果您要写一篇有关使用我们工具的项目的文章，那么如果您引用我们，我们将非常高兴。这是为我们的网站生成的BibTeX引文，可帮助人们了解我们的工具。

```text
@misc{wandb, 
title = {Experiment Tracking with Weights and Biases}, 
year = {2020}, 
note = {Software available from wandb.com}, 
url={https://www.wandb.com/}, 
author = {Biewald, Lukas},
}
```

