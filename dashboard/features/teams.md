---
description: 与你的同事合作，分享结果，并跟踪整个团队的所有实验。
---

# Teams

使用Weights & Biases作为你的机器学习团队的中央资源库。

* **追踪**你的团队尝试过的**所有实验**，这样你就不会重复工作了。仅仅几行监控代码就能让你快速、可靠地跟踪模型性能指标、预测、GPU使用情况以及训练模型的代码版本。
* **保存、恢复和重现**之前训练的模型。
* 与你的老板和同事**分享进度**和结果。
* **捕捉回归**，并在性能下降时立即得到警告。
* **对模型性能进行基准测试**，并自定义查询以比较你的模型版本。

## **常见问题** <a id="common-questions"></a>

###  **获取拥有私有团队的权限** <a id="get-access-to-private-teams"></a>

如果你是在一个公司，我们提供了企业计划。查看[价格页面](https://www.wandb.com/pricing)了解更多细节。我们为从事开源项目的学术工作提供免费的私有团队。查看[学术页面](https://www.wandb.com/academic)，申请升级。

### **创建一个新的团队** <a id="create-a-new-team"></a>

一旦您启用了该功能，请在应用程序中的[设置](https://app.wandb.ai/settings)页面上创建一个新的团队。这个名称将会被用于你所有团队项目的URL中，所以请确保你选择一些简短和描述性的名称，因为你以后将无法更改它。

###  **移动运行到一个团队** <a id="move-runs-to-a-team"></a>

很容易在你可以访问的项目之间移动运行。在项目页面上:

1. 点击表格选项卡，来展开运行表格
2. 单击复选框来选择所有运行
3. **点击移动：目标项目可以在你的个人账户中，也可以是你所在的任何团队。**

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-M2ofrgw0jCj-1YRNd-w%2F-M2ogdOkKqoviE2QbEd4%2Fdemo%20-%20move%20runs.gif?alt=media&token=3d46cac0-fb1c-4a0e-97ff-b39b55fba1b8)

###  **向团队发送新的运行** <a id="send-new-runs-to-a-team"></a>

在你的脚本中，将实体（entity）设置为你的团队。实体（Entity）只是指你的用户名或团队名称。在发送运行到那里之前，在web应用中创建一个实体（个人账户或团队账户）。

```text
wandb.init(entity="example-team")
```

当你加入一个团队时，你的**默认实体**会被更新。这意味着在你的[设置页面](https://app.wandb.ai/settings)上，你会看到创建新项目的默认位置现在是你刚刚加入的团队。下面是该[设置页面](https://app.wandb.ai/settings)部分的一个例子看起来的样子：

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-MEvNn4Nyo-sORDTdtfO%2F-MEvOAeNnTPayM_tZUJM%2FScreen%20Shot%202020-08-17%20at%2012.48.57%20AM.png?alt=media&token=61d925e2-309b-4ada-a7c4-9ab3bbd2962a)

### **邀请团队成员** <a id="invite-team-members"></a>

您可以在您的团队设置页面上邀请新成员加入您的团队：app.wandb.ai/teams/。

###  **查看隐私设置** <a id="see-privacy-settings"></a>

您可以在团队设置页面查看所有团队项目的隐私设置：app.wandb.ai/teams/&lt;your-team-here&gt;。

这是团队设置页面看起来的样子。在这张截图中，隐私开关是打开的，这意味着团队中的所有项目只对该团队可见。

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-M2ofrgw0jCj-1YRNd-w%2F-M2ol5d9Jg85Qscsf8H3%2Fdemo%20-%20team%20settings.png?alt=media&token=cfb08fef-a557-47a5-ae90-ff28529aeae7)

###  **将成员从团队中移除** <a id="removing-members-from-teams"></a>

当团队成员离开时，要移除他们很容易。团队管理员可以打开团队设置页面，点击离队成员名字旁边的删除按钮。用户被删除后，他们记录到团队的任何运行都会保留。

### **账户类型** <a id="account-types"></a>

邀请同事加入团队，从这些选项中选择：

*  **成员:** 你的团队中的一名正式成员，通过电子邮件邀请。
* **管理员:** 一个可以添加和删除其他管理员和成员的团队成员。
* **服务：** 一个Service Worker，通过你的运行自动化工具使用W&B会使用到的一个API密钥。如果你从你的团队的服务账户中使用API密钥，请确保设置环境变量**WANDB\_USERNAME**来将运行归属给正确的用户。

[  
](https://docs.wandb.ai/app/features/alerts)

