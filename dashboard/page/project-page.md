---
description: 比较你的模型版本，在一个临时工作空间中探究结果，并将结果导出到报告中以保存笔记和可视化
---

# Project Page

 项目工作空间**Workspace** 为你提供了一个个人的沙盒来比较实验。使用项目来组织可以比较的模型，并对针对同一问题使用不同的结构、超参数、数据集、预处理等。

项目页面的标签:

1. ​ [**概览**](https://docs.wandb.ai/app/pages/project-page#overview-tab): 你项目的快照
2. **​**[**工作空间**](https://docs.wandb.ai/app/pages/project-page#workspace-tab): 个人可视化沙盒
3. ​ [**表格**](https://docs.wandb.ai/app/pages/project-page#table-tab)**:** 所有运行的鸟瞰图
4. ​ [**报告**](https://docs.wandb.ai/app/pages/project-page#reports-tab): 保存的笔记、运行和图表的快照
5. ​ [**扫描**](https://docs.wandb.ai/app/pages/project-page#sweeps-tab): 自动探索和优化

##  **概览标签** <a id="overview-tab"></a>

* **项目名称**: 点击可以编辑项目名称
* **项目描述**: 点击可以编辑项目描述并添加注解
* **删除项目**: 点击右下角的点菜单可以删除项目
* **项目隐私**: 编辑谁可以查看运行和报告——点击锁图标
* **最近活跃**: 查看该项目最近记录数据的时间
* **总计算量**: 我们将项目中所有运行的时间相加，得到这个总计算量
* **取消删除运行**: 点击下拉菜单并点击"取消删除所有运行" 来恢复项目中已删除的运行。

​ [查看实战案例 →](https://app.wandb.ai/example-team/sweep-demo/overview)​​

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-M7yi6eYRlYrlAU1WTVH%2F-M7yiLeDdkmlheNEi7l_%2Fundelete.png?alt=media&token=25e402be-f119-4c23-a131-33495befdc40)

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-M0dbpU5aGt2W5bMrqek%2F-M0ddL_OzbS7XWjcLCn2%2Fimage.png?alt=media&token=d80c5cb8-49d4-4444-b817-361bb6cbb9c2)

##  **工作空间标签** <a id="workspace-tab"></a>

 **运行侧边栏**: 你的项目中所有运行的列表

* **点菜单**: 将鼠标悬停在侧边栏的某一行上，可以看到左侧出现的菜单。使用该菜单可以重命名一个运行，删除一个运行，或停止和激活运行。
* **可见性图标**: 点击眼睛图标可以打开和关闭图上的运行。
* **颜色: 将运行颜色更改为我们的预设颜色或自定义颜色。**
* **搜索**: 按名称搜索运行。这也会过滤图中的可见运行。
* **过滤器**: 使用侧边栏过滤器缩小可见的运行集合
* **分组**: 选择一个配置列来动态地对运行进行分组，例如按结构（architecture）进行分组。分组使图上的平均值用一条线显示，图上点的方差用阴影区域显示。 
* **排序**: 选择一个值对你的运行进行排序，例如最低损失和最高精确率。排序会影响图形上显示的运行。
* **展开按钮**: 将侧边栏展开为完整表格。
* **运行计数**: 顶部括号中的数字是项目中运行的总数量。数字 \(N 可见\)是眼睛图标被打开且在每个图中可见的运行数量。在下面的示例中，图形中只显示183个运行中的前10个。编辑图形可以增加可见的最大运行数。

 **面板布局**: 使用这个临时空间来探索结果，添加和删除图表，并基于不同指标来比较你的模型的版本。

​[查看实战案例 →](https://app.wandb.ai/example-team/sweep-demo)​​

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-M0dbpU5aGt2W5bMrqek%2F-M0dgOfTiQ30FwrUxddB%2Fimage.png?alt=media&token=b571e66c-e2a7-4415-bb3c-1d82d96944bc)

### **搜索运行** <a id="search-for-runs"></a>

 在侧边栏中按名称搜索运行。你可以使用正则表达式来过滤你可见的运行。搜索框会影响图形上显示的运行。下面是一个例子：

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-M0dbpU5aGt2W5bMrqek%2F-M0dj4fP6WvwLiU0b1uQ%2F2020-02-21%2013.51.26.gif?alt=media&token=d43131e3-bd8a-4cf9-82a2-3561416f4f2e)

### **添加面板分段（Section）** <a id="add-a-section-of-panels"></a>

单击面板分段（section）下拉菜单，然后点击添加分段“Add section”为面板创建一个新的分段。你可以重新命名分段，拖动他们以重新组织，以及展开和折叠分段。

每个分段右上角有一些选项：

* **切换到自定义布局**: 自定义布局允许你单独调整面板大小。
* **切换到标准布局**: 标准布局允许一次性调整分段中的所有面板的大小，并提供分页功能。
* **添加分段**: 从下拉菜单中在上面或下面添加一个分段，或点击页面底部的按钮添加一个新分段。
* **重命名分段**: 更改分段的标题。
* **导出分段到报告**: 将面板的该分段保存到一个新报告。
* **删除分段**: 删除整个分段和所有图表。该操作可以通过工作区栏中页面底部的撤销按钮来撤销。
* **添加面板**: 点击加号按钮，可向该分段添加面板。

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-M1c7kK381fHgNbr9MRd%2F-M1c8lfbYOb5Q2lb8vCN%2Fadd-section.gif?alt=media&token=282a4058-8bcb-46d2-b181-77d8fbc5111d)

### **在分段之间移动面板** <a id="move-panels-between-sections"></a>

拖放面板可以重新排序和组织分段。你也可以点击面板右上角的移动“Move”按钮，选择要将该面板移至的分段。

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-M1c7kK381fHgNbr9MRd%2F-M1c9Br_1hDUT8rI06iR%2Fmove-panel.gif?alt=media&token=fb76f55a-d919-45f5-a49a-924bcb8bf73e)

### **调整面板大小** <a id="resize-panels"></a>

* **标准布局**: 所有面板都保持相同的大小，并且有面板分页。通过点击并拖动右下角，可以调整面板大小。通过点击并拖动分段的右下角可以调整分段的大小。
* **自定义布局**: 所有面板的大小是独立的，并且没有分页。

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-M1c7kK381fHgNbr9MRd%2F-M1c9EYmNkbAxeXIn69U%2Fresize-panel.gif?alt=media&token=9c1d7eab-c2c2-4165-b5c3-8421b5dad5ff)

###  **搜索指标（metric）** <a id="search-for-metrics"></a>

使用工作区的搜索框来过滤面板。该搜索匹配面板标题，默认是可视化指标的名称。

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-M8TheW9ebvFMzMjp82c%2F-M8TiGj59wKSfTqV72Mt%2Fsearch%20in%20the%20workspace.png?alt=media&token=1f5bb3ff-6c35-4e02-8893-064562c22d77)

##  **表格标签** <a id="table-tab"></a>

使用该表格对结果进行过滤、分组和排序。

​ [查看实战案例 →](https://app.wandb.ai/example-team/sweep-demo/table?workspace=user-carey)​​

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-M2-uo1UQqblEd7E4Ya_%2F-M2-vJHAUHyQXhp3LSse%2Fimage.png?alt=media&token=65a05aad-5995-4622-a448-08d7119a676e)

## **报告标签** <a id="reports-tab"></a>

在一个地方查看所有的结果快照，并与你的团队分享发现结果。

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-M2-uo1UQqblEd7E4Ya_%2F-M2-vi7EQPY08qcfThDa%2Freports-tab.png?alt=media&token=280ada3e-87c6-4438-9ae3-e9617b725b76)

## **扫描标签** <a id="sweeps-tab"></a>

从你的项目开始一个新的扫描。

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-M2-uo1UQqblEd7E4Ya_%2F-M2-vrOHHdfn1uzN5N_l%2Fsweeps-tab.png?alt=media&token=f7db45c7-4f08-4d1d-beef-ed3f407dfb60)

## **常见问题** <a id="common-questions"></a>

###  **重置工作空间** <a id="reset-workspace"></a>

如果你在你的项目页面上看到一个类似下面的错误，下面是如何重置你的工作空间`"objconv: "100000000000" overflows the maximum values of a signed 64 bits integer"`

 在URL结尾添加**?workspace=clear**，然后按回车键。这会让你进入一个已清理的项目页面空间。

###  **删除项目** <a id="delete-projects"></a>

你可以通过点击概览选项卡右侧的三个点来删除你的项目。

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-LzbpqanX3nTzy4gh_Wm%2F-LzbpvMqIdHKlXJLzQ-q%2Fhowto%20-%20delete%20project.gif?alt=media&token=a07d9791-c9f5-4b1f-ab4d-21937664b766)

###  **隐私设置** <a id="privacy-settings"></a>

点击页面顶部导航栏中的锁图标来更改项目隐私设置。你可以编辑谁可以查看或提交运行到你的项目。这些设置包括项目中的所有运行和报告。如果你只想与少数人分享你的结果，你可以创建一个[私有团队](https://docs.wandb.ai/app/features/teams)。

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-M0dk3_G2Z2XFax24mN5%2F-M0dka-KGUSIueb7OQdl%2Fimage.png?alt=media&token=d4922440-323d-4194-b5eb-66cf2d0a8586)

### **删除空项目**  <a id="delete-an-empty-project"></a>

通过点击下拉菜单并选择删除项目“Delete Project”,来删除没有运行的项目。

