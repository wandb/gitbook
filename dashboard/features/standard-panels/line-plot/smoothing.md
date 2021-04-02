---
description: 'In line plots, use smoothing to see trends in noisy data.'
---

# Smoothing

In Weights & Biases line plots, we support three types of smoothing:

* ​[exponential moving average](https://docs.wandb.ai/app/features/panels/compare-metrics/smoothing#exponential-moving-average-default) \(default\)
* ​[gaussian smoothing](https://docs.wandb.ai/app/features/panels/compare-metrics/smoothing#gaussian-smoothing)​
* ​[running average](https://docs.wandb.ai/app/features/panels/compare-metrics/smoothing#running-average)​

See these live in an [interactive W&B report](https://wandb.ai/carey/smoothing-example/reports/W-B-Smoothing-Features--Vmlldzo1MzY3OTc).

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-MVwQ9p8HMAXP_hdvAdY%2F-MVwR6xXY6CPDB14teMy%2Fbeamer%20-%20smoothing.gif?alt=media&token=b3a00a28-6e65-4e81-a42c-b891a483a2c5)

## Exponential Moving Average \(Default\) <a id="exponential-moving-average-default"></a>

Exponential moving average is implemented to match TensorBoard's smoothing algorithm. The range is 0 to 1. See [Exponential Smoothing](https://www.wikiwand.com/en/Exponential_smoothing) for background. There is a debias term added so that early values in the time series are not biases towards zero.

Here is sample code for how this works under the hood:

```text
  data.forEach(d => {    const nextVal = d;    last = last * smoothingWeight + (1 - smoothingWeight) * nextVal;    numAccum++;    debiasWeight = 1.0 - Math.pow(smoothingWeight, numAccum);    smoothedData.push(last / debiasWeight);
```

Here's what this looks like [in the app](https://wandb.ai/carey/smoothing-example/reports/W-B-Smoothing-Features--Vmlldzo1MzY3OTc):![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-MVwThFT-sefB8FrHjn6%2F-MVwZCKrKK0Wd8vs5dRZ%2FScreen%20Shot%202021-03-16%20at%2012.43.45%20PM.png?alt=media&token=882367c3-3be7-4385-8527-0c7c83e2508b)

## Gaussian Smoothing <a id="gaussian-smoothing"></a>

Gaussian smoothing \(or gaussian kernel smoothing\) computes a weighted average of the points, where the weights correspond to a gaussian distribution with the standard deviation specified as the smoothing parameter. See . The smoothed value is calculated for every input x value.

Gaussian smoothing is a good standard choice for smoothing if you are not concerned with matching TensorBoard's behavior. Unlike an exponential moving average the point will be smoothed based on points occurring both before and after the value.

Here's what this looks like [in the app](https://wandb.ai/carey/smoothing-example/reports/W-B-Smoothing-Features--Vmlldzo1MzY3OTc#3.-gaussian-smoothing):![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-MVwThFT-sefB8FrHjn6%2F-MVw_9RhHbqQTvW6-uxV%2Fimage.png?alt=media&token=2cff82ca-7d78-4c0b-adc9-1c9c8597091d)

## Running Average <a id="running-average"></a>

Running average is a simple smoothing algorithm that replaces a point with the average of points in a window before and after the given x value. See "Boxcar Filter" at [https://en.wikipedia.org/wiki/Moving\_average](https://en.wikipedia.org/wiki/Moving_average). The selected parameter for running average tells Weights and Biases the number of points to consider in the moving average.

Running average is a simple, trivial to replicate smoothing algorithm. If your points are spaced unevenly on the x-axis Gaussian Smoothing may be a better choice.

Here's what this looks like [in the app](https://wandb.ai/carey/smoothing-example/reports/W-B-Smoothing-Features--Vmlldzo1MzY3OTc#4.-running-average):![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-MVwThFT-sefB8FrHjn6%2F-MVw_KWzi9oQGXccdoEV%2Fimage.png?alt=media&token=2504d9a7-d22e-4554-874b-ee2cc7a1eea0)

## Implementation Details <a id="implementation-details"></a>

All of the smoothing algorithms run on the sampled data, meaning that if you log more than 3000 points, the smoothing algorithm will run _after_ the points are downloaded from the server. The intention of the smoothing algorithms is to help find patterns in data quickly. If you need exact smoothed values on metrics with a large number of logged points, it may be better to download your metrics through the API and run your own smoothing methods.

## Hide original data <a id="hide-original-data"></a>

By default we show the original, unsmoothed data as a faint line in the background. Click the **Show Original** toggle to turn this off.![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-MVwThFT-sefB8FrHjn6%2F-MVw_lJkBdmzI9eZvWov%2Fdemo%20-%20wandb%20smoothing%20turn%20on%20and%20off%20original%20data.gif?alt=media&token=de5155ad-ff65-43b8-a96d-758bd5368c33)[  
](https://docs.wandb.ai/app/features/panels/compare-metrics/sampling-and-bucketing)

