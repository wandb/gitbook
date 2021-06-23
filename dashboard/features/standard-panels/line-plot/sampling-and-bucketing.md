# Sampling and Bucketing

## 采样

出于性能原因，当为线图度量选择超过 1500 个点时，W&B 将返回 1500 个随机采样点。 每个指标都是单独采样的，并且只考虑实际记录指标的步骤。

如果您想查看为运行记录的所有指标或实施您自己的抽样，您可以使用 W&B Api。

```python
run = api.run("l2k2/examples-numpy-boston/i0wt6xua")
history = run.scan_history(keys=["Loss"])
losses = [row["Loss"] for row in history]
```

## 分桶\(**Bucketing**\)

当分组或使用具有可能未对齐的 x 轴值的多次运行的表达式时，分桶用于对点进行下采样\(downsample\)。 x 轴被分成 200 个大小均匀的段，然后在每个段内对给定指标的所有点进行平均。 当分组或使用表达式来组合指标时，段内的这个平均值被用作指标的值

