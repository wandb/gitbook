# WandbMetricsLogger



[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/latest/wandb/integration/keras/callbacks/metrics_logger.py#L22-L79)



`WandbMetricsLogger` automatically logs the `logs` dictionary

```python
WandbMetricsLogger(
    log_freq: Union[LogStrategy, int] = "epoch",
    *args,
    **kwargs
) -> None
```



that callback methods take as argument to wandb.

It also logs the system metrics to wandb.

| Arguments |  |
| :--- | :--- |
|  log_freq ("epoch", "batch", or int): if "epoch", logs metrics at the end of each epoch. If "batch", logs metrics at the end of each batch. If an integer, logs metrics at the end of that many batches. Defaults to "epoch". |



## Methods

<h3 id="set_model"><code>set_model</code></h3>

```python
set_model(
    model
)
```




<h3 id="set_params"><code>set_params</code></h3>

```python
set_params(
    params
)
```






