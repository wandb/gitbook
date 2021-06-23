---
description: 在线图中，使用平滑来查看有噪音的数据的趋势。
---

# Smoothing

在Weights & Biases线图中，我们支持三种类型的平滑：

* [指数移动平均](https://docs.wandb.ai/v/zh-hans/dashboard/features/standard-panels/line-plot/smoothing#exponential-moving-average-default)（默认）
* ​[高斯平滑​](https://docs.wandb.ai/v/zh-hans/dashboard/features/standard-panels/line-plot/smoothing#gaussian-smoothing)
*  [运行平均​](https://docs.wandb.ai/v/zh-hans/dashboard/features/standard-panels/line-plot/smoothing#running-average)

 在[交互式 W&B 报告](https://wandb.ai/carey/smoothing-example/reports/W-B-Smoothing-Features--Vmlldzo1MzY3OTc)中实时查看这些内容。

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-MVwQ9p8HMAXP_hdvAdY%2F-MVwR6xXY6CPDB14teMy%2Fbeamer%20-%20smoothing.gif?alt=media&token=b3a00a28-6e65-4e81-a42c-b891a483a2c5)

## 指数移动平均（默认） <a id="exponential-moving-average-default"></a>

 实现指数移动平均以匹配 TensorBoard 的平滑算法。范围是 0 到 1。有关背景，请参阅[指数平滑](https://www.wikiwand.com/en/Exponential_smoothing)。这里添加了一个 debias 项，以便时间序列中的早期值不会偏向于零。

以下是有关其内部工作原理的示例代码：

```text
  data.forEach(d => {    const nextVal = d;    last = last * smoothingWeight + (1 - smoothingWeight) * nextVal;    numAccum++;    debiasWeight = 1.0 - Math.pow(smoothingWeight, numAccum);    smoothedData.push(last / debiasWeight);
```

 这是[app](https://wandb.ai/carey/smoothing-example/reports/W-B-Smoothing-Features--Vmlldzo1MzY3OTc)中的样子

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-MVwThFT-sefB8FrHjn6%2F-MVwZCKrKK0Wd8vs5dRZ%2FScreen%20Shot%202021-03-16%20at%2012.43.45%20PM.png?alt=media&token=882367c3-3be7-4385-8527-0c7c83e2508b)

## 高斯平滑 <a id="gaussian-smoothing"></a>

高斯平滑（或高斯核平滑）计算点的加权平均值，其中权重对应于高斯分布，标准偏差指定为平滑参数。看 。为每个输入 x 值计算平滑值。

如果您不关心匹配 TensorBoard 的行为，高斯平滑是一个很好的平滑标准选择。与指数移动平均线不同，该点将根据该值前后出现的点进行平滑处理。

 这是[app](https://wandb.ai/carey/smoothing-example/reports/W-B-Smoothing-Features--Vmlldzo1MzY3OTc#3.-gaussian-smoothing)中的样子：

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-MVwThFT-sefB8FrHjn6%2F-MVw_9RhHbqQTvW6-uxV%2Fimage.png?alt=media&token=2cff82ca-7d78-4c0b-adc9-1c9c8597091d)

## 运行平均 <a id="running-average"></a>

运行平均是一种简单的平滑算法，它用给定 x 值前后的窗口中点的平均值替换一个点。请参阅[https://en.wikipedia.org/wiki/Moving\_average](https://en.wikipedia.org/wiki/Moving_average) 上的“Boxcar 过滤器”。为运行平均选择的参数告诉Weights and Biases在移动平均中要考虑的点数。

运行平均是一种简单的用来复制平滑算法的方法。如果您的点在 x 轴上的间距不均匀，高斯平滑可能是更好的选择。

 这是[app](https://wandb.ai/carey/smoothing-example/reports/W-B-Smoothing-Features--Vmlldzo1MzY3OTc#4.-running-average)中的样子：

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-MVwThFT-sefB8FrHjn6%2F-MVw_KWzi9oQGXccdoEV%2Fimage.png?alt=media&token=2504d9a7-d22e-4554-874b-ee2cc7a1eea0)

##  实施细则 <a id="implementation-details"></a>

所有平滑算法都在采样数据上运行，这意味着如果您记录超过 3000 个点，平滑算法将在从服务器下载这些点后运行。平滑算法的目的是帮助快速找到数据中的模式。如果您需要具有大量记录点的指标的精确平滑值，最好通过 API 下载您的指标并运行您自己的平滑方法。

##  隐藏原始数据 <a id="hide-original-data"></a>

默认情况下，我们将原始的、未平滑的数据显示为背景中的一条模糊线。点击**Show Original**  


切换以关闭此功能。[  
](https://docs.wandb.ai/app/features/panels/compare-metrics/sampling-and-bucketing)

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-MVwThFT-sefB8FrHjn6%2F-MVw_lJkBdmzI9eZvWov%2Fdemo%20-%20wandb%20smoothing%20turn%20on%20and%20off%20original%20data.gif?alt=media&token=de5155ad-ff65-43b8-a96d-758bd5368c33)

