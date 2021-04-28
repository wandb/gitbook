---
description: Weights & Biases UI中自定义图表功能的使用教程
---

# Custom Charts Walkthrough

要超越Weights & Biases中的内置图表功能，请使用新的**自定义图表**功能来控制你要加载到面板中的数据的细节，以及如何将这些数据可视化。

**概述**

1. 记录数据到W&B
2. 创建一个查询
3. 自定义图表

## 1.  **记录数据到W&B** <a id="1-log-data-to-w-and-b"></a>

首先，在你的脚本中记录数据。使用[wandb.config](https://docs.wandb.ai/v/zh-hans/library/wandb.config) 来记录训练开始时设置的单个点，例如超参数。使用[wandb.log\(\) ](https://docs.wandb.ai/v/zh-hans/library/wandb.log)来记录一段时间内的多个点，并使用wandb.Table\(\)来记录自定义的二维数组。我们建议每个记录键最多记录10,000 个数据点。

```text
# Logging a custom table of datamy_custom_data = [[x1, y1, z1], [x2, y2, z2]]wandb.log({“custom_data_table”: wandb.Table(data=my_custom_data,                                columns = ["x", "y", "z"])})
```

​ [尝试一个快速示例笔记本](https://bit.ly/custom-charts-colab)来记录数据表，下一步我们将设置自定义图表。在[实时报告](https://wandb.ai/demo-team/custom-charts/reports/Custom-Charts--VmlldzoyMTk5MDc)中看看生成的图表是什么样子的。

## 2. **创建一个查询** <a id="2-create-a-query"></a>

 一旦你记录了要可视化的数据，进入你的项目页面，并点击`+`按钮添加一个新面板，然后选择**自定义图表**。你可以在[这个工作空间里](https://wandb.ai/demo-team/custom-charts)跟着做。

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-MFpVi21a7ZcyrfSzti9%2F-MFpWFMFa8nEVysqOEMX%2FScreen%20Shot%202020-08-28%20at%207.41.37%20AM.png?alt=media&token=f3e5771f-1b85-45a6-ba89-9da939e4bb1f)

**添加一个查询**

1.   单击`summary`，选择`historyTable`，设置一个新的查询，从运行历史中提取数据
2.  输入你记录**wandb.Table\(\)** 的键。在上面的代码片段中，它是`my_custom_table` 。在[示例笔记本中](https://bit.ly/custom-charts-colab), 键是`pr_curve` 和`roc_curve`

### **设置Vega字段** <a id="set-vega-fields"></a>

现在查询正在这些列中加载，他们作为选项可以在Vega 字段下拉菜单中选择：

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-MFpYa0dZQk1sUDry1wD%2F-MFpaPD3jxjAzBD4gHLN%2FScreen%20Shot%202020-08-28%20at%208.04.39%20AM.png?alt=media&token=73dad583-ce91-49e1-9a28-495355a003f5)

* **x-axis:** runSets\_historyTable\_r \(召回率\)
* **y-axis:** runSets\_historyTable\_p \(精确率\)
* **color:** runSets\_historyTable\_c \(类标签\)

## 3. **自定义图表** <a id="3-customize-the-chart"></a>

现在看起来很不错，但我想从散点图换成线图。点击**编辑**来改变这个内置图表的Vega规范。在[这个工作空间](https://wandb.ai/demo-team/custom-charts)中跟着做。

![](https://paper-attachments.dropbox.com/s_5FCA7E5A968820ADD0CD5402B4B0F71ED90882B3AC586103C1A96BF845A0EAC7_1597442115525_Screen+Shot+2020-08-14+at+2.52.24+PM.png)

我更新了Vega 规范以自定义可视化：

* 为绘图、图例、x-轴和y-轴添加标题\(为每个字段设置标题“title”\)
* 将“mark”的值从“point”改为“line”
* 移除未使用的“size”字段

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-MFpYa0dZQk1sUDry1wD%2F-MFpbWpPViR86jIE7Ruv%2Fcustomize%20vega%20spec%20for%20pr%20curve.png?alt=media&token=d8d76b36-df79-4bef-8f46-bbb3ecf5d4a8)

要将其保存为一个预设，以便在项目中的其他地方使用，请点击页面顶部的**另存为**。下面是结果,还有一条ROC曲线:

![](https://paper-attachments.dropbox.com/s_5FCA7E5A968820ADD0CD5402B4B0F71ED90882B3AC586103C1A96BF845A0EAC7_1597442868347_Screen+Shot+2020-08-14+at+3.07.30+PM.png)

感谢你的关注！有问题和反馈请发消息到Carey \(c@wandb.com\) [😊](https://emojipedia.org/smiling-face-with-smiling-eyes/)​

### **奖励:复合直方图**

直方图可以将数值分布可视化，帮助我们理解更大的数据集。复合直方图在同一分箱中显示多个分布，让我们可以比较不同模型或模型中的不同类中的两个或多个指标。对于检测驾驶场景中物体的语义分割模型来说，我们可能会比较优化准确率与并交比（IOU）的有效性，或者我们可能想要知道不同模型检测汽车 \(数据中大的、常见的区域\) 与交通标志 \(小的多，不常见的区域\)的效果。在[演示Colab](https://bit.ly/custom-charts-colab)中， 你可以比较十类生物中的两类的置信度。

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-MFpQUCfGqtD3B_3Cg-U%2F-MFpR7yBK2aiHxUEpj41%2FScreen%20Shot%202020-08-28%20at%207.19.47%20AM.png?alt=media&token=8aeca9bb-f15f-45e0-9cc0-079224fc859a)

要创建你自己版本的自定义复合直方图面板：

1. 在你的工作空间或报告中创建一个新的自定义图表面板\(通过添加一个“自定义图表”可视化\)。点击右上角的“编辑”按钮，从任何内置面板类型开始修改Vega规范。
2. 用我的[MVP代码（用于Vega复合直方图）](https://gist.github.com/staceysv/9bed36a2c0c2a427365991403611ce21)代替那个内置的Vega规范，你可以直接在这个Vega 规范中[使用Vega语](https://vega.github.io/)法修改标题、轴标题、输入域以及任何其他细节\(你可以改变颜色甚至添加第三个直方图:\)
3. 修改右侧的查询，从你的wandb日志中加载正确的数据。添加字段“summaryTable”并相应地设置“tableKey”为“class\_scores”以获取你的运行记录的wandb.Table。这将让你通过下拉菜单将wandb.Table中记录为“class\_scores”的列填充到两个直方图分箱集 \(“red\_bins” and “blue\_bins”\) 中。例如在我的示例中，我选择animal”分类预测分数为红色分箱，“plant”为蓝色分箱。
4. 你可以不断地对Vega 规范和查询进行修改，直到你对预览渲染中看到的绘图满意为止。一旦你完成了，点击顶部的“另存为”，并给你的自定义绘图命名，以便你可以重复使用。然后点击“从面板库应用”来完成你的绘制。

以下是我从一个非常简短的实验中得到的结果：在一个周期仅对1000个示例进行训练，这个模型非常确定大多数图片不是植物，而对于哪些图片可能是动物非常不确定。[  
](https://docs.wandb.ai/app/features/custom-charts)

![](https://paper-attachments.dropbox.com/s_5FCA7E5A968820ADD0CD5402B4B0F71ED90882B3AC586103C1A96BF845A0EAC7_1598376160845_Screen+Shot+2020-08-25+at+10.08.11+AM.png)

![](https://paper-attachments.dropbox.com/s_5FCA7E5A968820ADD0CD5402B4B0F71ED90882B3AC586103C1A96BF845A0EAC7_1598376315319_Screen+Shot+2020-08-25+at+10.24.49+AM.png)

