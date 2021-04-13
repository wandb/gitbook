---
description: 将单独的训练和评估运行分组到更大的实验中。
---

# Grouping

通过向**wandb.init\(\)** 传递唯一的组名，将独立的作业分组到实验中。

###  **使用案例**

1. **分布式训练:** 如果你的实验拆分为有单独的训练，并且评估脚本被视为更大的整体的一部分，应使用分组。
2. **多进程**: 将多个较小的进程分组到一个实验。
3.  **K折交叉验证（K-fold cross-validation）**: 使用不同的随机种子（seed）将运行分组到一起以查看一个更大的实验。这里有一个使用扫描和分组进行K折交叉验证的[例子](https://github.com/wandb/examples/tree/master/examples/wandb-sweeps/sweeps-cross-validation)。 

 设置分组的方式有三种:

### **1. 在你的脚本中设置分组** <a id="1-set-group-in-your-script"></a>

向wandb.init\(\)传递一个可选的group和job\_type 。这样会为你的每个实验提供包含独立运行的一个独立分组页面， 例如:`wandb.init(group="experiment_1", job_type="eval")。`

### **2. 设置一个组环境变量** <a id="2-set-a-group-environment-variable"></a>

使用`WANDB_RUN_GROUP` 为你的运行指定一个组作为环境变量。更多信息，请查看我们的[环境变量](https://docs.wandb.ai/v/zh-hans/library/environment-variables)文档。group应该在你的项目中是唯一的，并且由该组中所有运行共享。你可以使用wandb.util.generate\_id\(\)生成一个长度8个字符的唯一的字符串，在你的所有—进程中是使用。 例如:os.environ\["WANDB\_RUN\_GROUP"\] = "experiment-" + wandb.util.generate\_id\(\)

### **3. 在UI中切换分组** <a id="3-toggle-grouping-in-the-ui"></a>

你可以通过任何配置列来动态分组。例如，如果你使用 `wandb.config` 记录批次大小（Batch Size）或学习速率（Learning Rate）,那么你可以在web应用程序中按这些超参数动态分组。

## **使用分组进行分布式训练** <a id="distributed-training-with-grouping"></a>

 如果你在**wandb.init\(\)**中设置分组，在UI中默认我们会对运行分组。你可以点击表格顶部的**分组**按钮开启或关闭该功能。这里有一个[示例代码](http://wandb.me/grouping)生成的一个[示例项目](https://wandb.ai/carey/group-demo?workspace=user-carey)，在该示例代码中我们设置了分组。你可以点击侧边栏中的每个“分组”行，以获得该实验的一个独立分组页面。

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-MV_-H5y5WWmuiyuqBoF%2F-MV_-mrQ2IAVKpmjvG1N%2Fimage.png?alt=media&token=88310228-c081-4431-829e-dce84d4bc5b0)

 从上边的项目页面中，你点击左侧边栏中的一个分组，可以获得一个像下边[这样一个](https://wandb.ai/carey/group-demo/groups/exp_5?workspace=user-carey)独立的页面：

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-MV_2g_6Cs08x016lUuI%2F-MV_315Eu7sQEIHPdJ53%2Fimage.png?alt=media&token=79a11442-0607-4914-8885-da28a4f583ec)

## **在UI中动态分组** <a id="grouping-dynamically-in-the-ui"></a>

你可以通过任何列对运行进行分组，例如通过超参数。如下示例所示:

* **侧边栏:**通过周期数对运行分组。
* **图形：** 每条线代表一个分组**，**阴影表示方差。图形的行为可以在图形设置中修改。

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-M2qgFEA6OtonVaq8TOV%2F-M2qrB8rkvV4tp-6C1rl%2Fdemo%20-%20grouping.png?alt=media&token=d2a113da-ff34-49cb-9bc3-a41ef993b786)

## **关闭分组** <a id="turn-off-grouping"></a>

点击分组按钮并清空分组字段，将使表格和图返回到未分组的状态。

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-M2qgFEA6OtonVaq8TOV%2F-M2qu0Ckm5rLHkzsv1dy%2Fdemo%20-%20no%20grouping.png?alt=media&token=5ac807d7-5585-4959-a76c-6310679f4a0f)

## **分组图设置** <a id="grouping-graph-settings"></a>

点击图右上角的编辑按钮，选择“**高级**”选项卡来更改线条和阴影。你可以为每组中的线条选择平均值、最小值、最大值。对于阴影，你可以选择关闭阴影、显示最小和最大值、标准方差和标准误差。

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-MKAiDr8tHewYkq2KghI%2F-MKAiM26223fafU0Bb-E%2Fdemo%20-%20grouping%20options%20for%20line%20plots.gif?alt=media&token=4f7998c9-e3d2-42aa-9556-0ee1b8312f4e)

