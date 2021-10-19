# Sampling and Bucketing

## Sampling

For performance reasons, when over 1500 points are chosen for a line plot metric, W&B returns 1500 randomly sampled points.  Each metric is sampled separately and only steps where the metric is actually logged are considered.

If you want to look at all of the metrics logged for a run or implement your own sampling you can use the W&B Api.

```python
run = api.run("l2k2/examples-numpy-boston/i0wt6xua")
history = run.scan_history(keys=["Loss"])
losses = [row["Loss"] for row in history]
```

## Bucketing

When grouping or using expressions with multiple runs with possibly not-aligned x axis values, bucketing is used to downsample the points.  The x-axis is divided into 200 evenly sized segments and then within each segments all points for a given metric are averaged. When grouping or using expressions to combine metrics, this average inside a segment is used as the value of the metric.

