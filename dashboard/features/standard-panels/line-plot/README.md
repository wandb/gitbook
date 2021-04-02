---
description: 可视化指标，自定义坐标轴，并在同一图上比较多条线。
---

# Line Plot

当你使用 **wandb.log\(\)** 随时间推移绘制指标时，线图会默认显示。使用图表设置自定义，以在同一绘图上比较多条线，计算自定义轴，并重命名标签。

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-MAPLzkRv81ROK5m6XIV%2F-MAPM02mKAkmXPowdVGp%2Fline%20plot%20example.png?alt=media&token=4dcd41eb-0f42-4c51-8587-9becbc706d89)

## **设置** <a id="settings"></a>

**数据**

* **X轴:** 选择默认的X轴，包括步（Step）和相对时间，或者选择一个自定义的X轴。如果你想使用自定义的 x 轴，请确保它被记录在你用于记录y轴的 `wandb.log()` 的同一调用中。
  * **相对时间\(挂钟\)**是自进程开始后的时钟时间，所以如果你启动一个运行并在一天后断点续训它，并记录了一些东西，将被绘制24小时。
  * **相对时间\(进程\)**是指运行进程内部的时间，如果你启动了一个运行，并且运行了10秒，在一天后断点续训，那么点就会被绘制成10s的样
  * **挂钟时间**是指图上第一次运行开始后的分钟数。
  * 每次调用`wandb.log()`时，默认情况下**步**都会递增，并且应该反映你从模型中记录的训练步数。
* **Y轴：**从记录的数值中选择y轴，包括随时间变化的指标和超参数。
* **最小、最大和记录刻度：**线图中X轴和Y轴的最小、最大和记录刻度设置
* **平滑化和排除异常值：**改变线图的平滑度或重新调节，以从默认绘图的最小和最大刻度中排除异常值。
*  **要显示的最大运行数:** 通过增加这个数字，在线图上一次性显示更多的线条，默认为10条。如果有超过10个运行，但图表限制了可见的数量，你会在图表顶部看到"显示前10个运行"的消息。
* **图表类型:** 在线图、面积图和百分比面积图之间进行切换。

**X轴设置**

 ****X轴可以在图形层级进行设置，也可以在项目页面或报告页面层级进行全局设置。下面是全局设置的样子:

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-MCzmDLol_aQM39UhvOZ%2F-MCznzf-l1CccrBQvnxU%2Fx%20axis%20global%20settings.png?alt=media&token=b37ce294-dc16-4b13-aade-61cb4d2800cc)

在线图设置中选择**多个y轴**来在同一图表上比较不同的指标，比如精确率和验证精确率。

**分组**

* 开启分组，查看可视化平均值的设置。
*  **组键:** 选择一列，该列中所有数值相同的运行将被归为一组
* **Agg**: 聚合-- 图上线条的值。选项是组的平均值、中位数、最小值和最大值。
* **范围:** 切换分组曲线后的阴影区域的行为。无（None）表示没有阴影区域。Min/Max 显示阴影区域，它涵盖了分组中所有点的范围。Std Dev 显示分组中数值的标准差。Std Err 显示的是阴影区域的标准误差。
* **采样运行：**如果你选择了数百个运行，我们默认只对前100个运行进行采样。 你可以选择在分组计算中包含所有的运行，但这可能会降低用户界面的速度。

 **图例**

* **标题:** 为线型图添加一个自定义的标题，显示在图表的顶部。
* **X轴标题：**为线图的X轴添加自定义标题，显示在图表的右下角。
* **Y轴标题：**为线图的y轴添加自定义标题，显示在图表的左上角。
* **图例:** 选择你想在每条线的图例中看到的字段。例如， 你可以显示运行的名称和学习率。
* **图例模板：**完全可定制，这个强大的模板允许你精确地指定你想在模板中显示的文本和变量，在线图的顶部，以及当你把鼠标悬停在图上时出现的图例。 

编辑线图图例以显示超参数

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-MQYGYnDTLwSV5h3woBe%2F-MQYGjDvSLnRWQ1WyeHQ%2FScreen%20Shot%202021-01-08%20at%2011.33.04%20AM.png?alt=media&token=274c3452-47e1-43c0-a6e5-107fa4262adc)

**表达式**

* **Y轴表达式:** 将计算的指标添加到你的图表中。你可以使用任何记录的指标以及配置值（如超参数）来计算自定义线。
* **X轴表达式：**使用自定义表达式重新调整X轴的比例，以使用计算值。有用的变量包括**\_step** 用于默认的 X轴，以及引用总结（Summary）值的语法是 `${summary:value`}。

##  **在图上可视化平均值** <a id="visualize-average-values-on-a-plot"></a>

如果你有几个不同的实验，你想在一个图表上查看它们的值的平均值，你可以使用表格中的分组功能。点击运行表格上方的”分组”，然后选择“全部”，就可以在图中显示平均值。

下面是平均之前的图形:

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-LrBA6YyPIio8CkQUsAl%2F-LrBBZuGF_oT7aIm93oQ%2Fdemo%20-%20precision%20lines.png?alt=media&token=36c6b55f-a3dc-4f77-b236-8138d30ae0b9)

在这里，我把这些线条进行了分组，以查看运行的平均值。

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-LrBA6YyPIio8CkQUsAl%2F-LrBBge1GS2xiKJXKCEV%2Fdemo%20-%20average%20precision%20lines.png?alt=media&token=2e39f99f-8220-4fcf-b3df-f82a9280014f)

## **在一个图表上比较两个指标** <a id="compare-two-metrics-on-one-chart"></a>

点击一个运行，进入运行页面。这里是Stacey的Estuary项目的一个[运行示例](https://app.wandb.ai/stacey/estuary/runs/9qha4fuu?workspace=user-carey)。自动生成的图表显示的是单一指标。

![](https://downloads.intercomcdn.com/i/o/146033177/0ea3cdea62bdfca1211ce408/Screen+Shot+2019-09-04+at+9.08.55+AM.png)

点击页面右上方的”**添加可视化”，**然后选择“**线图**”。

![](https://downloads.intercomcdn.com/i/o/142936481/d0648728180887c52ab46549/image.png)

在**Y变量**字段中，选择几个你想比较的指标。它们会一起显示在线图上。

![](https://downloads.intercomcdn.com/i/o/146033909/899fc05e30795a1d7699dc82/Screen+Shot+2019-09-04+at+9.10.52+AM.png)

##  **以不同的X轴可视化** <a id="changing-the-color-of-the-line-plots"></a>

如果你想看一个实验所花的绝对时间，或者想看一个实验是在哪一天运行的，你可以切换X轴。下面是一个从步（step）切换到相对时间，再切换到挂钟时间的示例。

### **面积图** <a id="from-the-run-table"></a>

在线图设置中，在高级选项卡中，点击不同的绘图样式，可以得到面积图或面积百分比图。

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-MVtqWqvGUlku0vznrx3%2F-MVttxCjv4HPz0w7_093%2Fimage.png?alt=media&token=62437827-9237-46f4-9d74-19d44f1a06e8)

 点击一个运行，进入运行页面。这里是Stacey的Estuary项目的一个[运行示例](https://app.wandb.ai/stacey/estuary/runs/9qha4fuu?workspace=user-carey)。自动生成的图表显示的是单一指标。

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-MVtqWqvGUlku0vznrx3%2F-MVtuTsH5ivrqUbg7lPO%2Fimage.png?alt=media&token=4714c273-67b3-4ed9-9b37-4156672fc74c)

### 点击页面右上方的”**添加可视化”，**然后选择“**线图**”。 <a id="from-the-chart-legend-settings"></a>

在**Y变量**字段中，选择几个你想比较的指标。它们会一起显示在线图上。

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-MVtqWqvGUlku0vznrx3%2F-MVtwKhj0LZO2dYXllqX%2Fimage.png?alt=media&token=412edd98-a31d-4210-8fef-022ec49dd593)

## **以不同的X轴可视化** <a id="visualize-on-different-x-axes"></a>

如果你想看一个实验所花的绝对时间，或者想看一个实验是在哪一天运行的，你可以切换X轴。下面是一个从步（step）切换到相对时间，再切换到挂钟时间的示例。

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-Lv6SfafIqDnqcp4S-yq%2F-Lv6T7YbBmWP_MaJknPo%2Fhowto%20-%20use%20relative%20time%20or%20wall%20time.gif?alt=media&token=73960de7-e593-4c50-8fea-16facd177fe1)

##  **面积图** <a id="area-plots"></a>

 在线图设置中，在高级选项卡中，点击不同的绘图样式，可以得到面积图或面积百分比图。

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-M16zLbnJDX2CAH3ixe4%2F-M16zSKix8cdTGOZMF3I%2F2020-02-27%2010.49.10.gif?alt=media&token=7e487780-7958-4c39-9f66-1eb34782bc6c)

##  **缩放** <a id="zoom"></a>

点击并拖动一个矩形，可以同时进行垂直和水平缩放。这将改变X轴和Y轴的缩放。

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-M0s3Mic2svq947jOpxn%2F-M0s5J4w3HKy2fb3Aqzv%2F2020-02-24%2008.46.53.gif?alt=media&token=8035647d-3fe1-4428-9c01-8d9c3f9f613b)

##  **隐藏图例** <a id="hide-chart-legend"></a>

用这个简单的开关关闭线图中的图例:[  
](https://docs.wandb.ai/app/features/panels/parallel-coordinates)

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-MFCcuLKhdqrlE5ykKFt%2F-MFCd3wBuPZx-77FkUcS%2Fdemo%20-%20hide%20legend.gif?alt=media&token=d414784d-be41-430f-8f20-3f60852b08be)

