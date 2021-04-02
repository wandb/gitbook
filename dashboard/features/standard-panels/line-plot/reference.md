# Reference

### **X轴**

选择X轴

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-M0e0PqYEmqY7IY6on_G%2F-Lw5FUIkutzWVFirQ_bw%2Fimage.png?alt=media)

你可以将线图的X轴设置为你用wandb.log记录的任何值，只要它总是以数值形式记录的

## **Y轴变量** <a id="y-axis-variables"></a>

你可以将y轴变量设置为你用wandb.log记录的任何值，只要你记录的是数值、数值数组或数值直方图。如果你为一个变量记录了超过1500个点，wandb只会采样1500个点。

你可以通过改变运行表中运行的颜色来改变Y轴线的颜色。

##  **X轴范围和Y轴范围** <a id="x-range-and-y-range"></a>

你可以改变绘图的X轴和Y轴的最大和最小值。

X范围默认为从X轴的最小值到最大值。

Y范围默认为从你的指标的最小值，零到你的指标的最大值。

## **最大运行数/组数** <a id="max-runs-groups"></a>

默认情况下，你只可以绘制10个运行或运行组。运行将从你的运行表或运行集的顶部开始提取，所以如果你对运行表或运行集进行排序，可以改变显示的运行。

## **图例** <a id="legend"></a>

你可以控制你的任何运行的图表的图例，你记录的配置值，以及运行的元数据，如创建的时间或创建运行的用户。

例如：

{config:x}将为一个运行或组插入x的配置值。

你可以设置\[\[$x: $y\]\]以十字线显示显示特定值的点。

## **分组** <a id="grouping"></a>

你可以通过开启分组来汇总所有的运行情况，也可以对单个变量进行分组。你也可以通过在表中进行分组来开启分组，分组将自动填充到图表中。

## **平滑** <a id="smoothing"></a>

 你可以将平滑[系数](file:///E:/Dashboard_Features_Standard_Panels_Reference%20ZH.html#what-formula-do-you-use-for-your-smoothing-algorithm)设置在0和1之间，其中0是没有平滑，1是最大平滑。

##  **忽略异常值** <a id="ignore-outliers"></a>

忽略异常值使图形将y轴的最小值和最大值设置为数据的第5和第95百分位数，而不是设置为所有数据可见。

## **表达式** <a id="expression"></a>

表达式可以让你绘制从1-accuracy等指标中得出的值。目前，它只在绘制单个指标时有效。你可以进行简单的算术表达式、+、-、_、/和%，以及\*_的幂计算。

##  **绘图样式** <a id="plot-style"></a>

为你的线图选择一种样式。

**线图:**![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-M0dSP6Oyn4oMWeHq5jq%2F-M0dbaXHAHw4QPbjuzk9%2Fimage.png?alt=media&token=b5001a00-ca14-4500-861d-7770d7ce5290)

**面积图:**![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-M0dSP6Oyn4oMWeHq5jq%2F-M0dbhVrtBtarJOPqjcl%2Fimage.png?alt=media&token=98e8f53f-6d69-433e-b849-af1f08fa72b7)

**面积百分比图:**![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-M0dSP6Oyn4oMWeHq5jq%2F-M0dbl_-XlyGk9NNX86n%2Fimage.png?alt=media&token=f225578a-854f-4fa1-85bc-a9d995574fd1)[  
](https://docs.wandb.ai/app/features/panels/compare-metrics)

