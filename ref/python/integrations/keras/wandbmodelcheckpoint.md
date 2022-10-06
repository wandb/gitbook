# WandbModelCheckpoint



[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/latest/wandb/integration/keras/callbacks/model_checkpoint.py#L26-L173)



`WandbModelCheckpoint` periodically saves a Keras model or model weights

```python
WandbModelCheckpoint(
    filepath: Union[str, os.PathLike],
    monitor: str = "val_loss",
    verbose: int = 0,
    save_best_only: bool = (False),
    save_weights_only: bool = (False),
    mode: Mode = "auto",
    save_freq: Union[SaveStrategy, int] = "epoch",
    options: Optional[str] = None,
    initial_value_threshold: Optional[float] = None,
    **kwargs
) -> None
```



and uploads it to W&B as a `wandb.Artifact`.

Since this callback is subclassed from `tf.keras.callbacks.ModelCheckpoint`,
the checkpointing logic is taken care of by the parent callback. You can learn
more here:
https://www.tensorflow.org/api_docs/python/tf/keras/callbacks/ModelCheckpoint

This callback is to be used in conjunction with training using `model.fit()`
to save a model or weights (in a checkpoint file) at some interval. The
model checkpoints will be logged as W&B Artifacts. You can learn more here:
https://docs.wandb.ai/guides/artifacts

This callback provides the following features:
    - Save the model that has achieved "best performance" based on "monitor".
    - Save the model at the end of every epoch regardless of the performance.
    - Save the model at the end of epoch or after a fixed number of training batches.
    - Save only model weights, or save the whole model.
    - Save the model either in SavedModel format or in `.h5` format.

| Arguments |  |
| :--- | :--- |
|  filepath (Union[str, os.PathLike]): path to save the model file. monitor (str): The metric name to monitor. verbose (int): Verbosity mode, 0 or 1. Mode 0 is silent, and mode 1 displays messages when the callback takes an action. save_best_only (bool): if `save_best_only=True`, it only saves when the model is considered the "best" and the latest best model according to the quantity monitored will not be overwritten. save_weights_only (bool): if True, then only the model's weights will be saved. mode (Mode): one of {'auto', 'min', 'max'}. For `val_acc`, this should be `max`, for `val_loss` this should be `min`, etc. save_weights_only (bool): if True, then only the model's weights will be saved save_freq (Union[SaveStrategy, int]): `epoch` or integer. When using `'epoch'`, the callback saves the model after each epoch. When using an integer, the callback saves the model at end of this many batches. Note that when monitoring validation metrics such as `val_acc` or `val_loss`, save_freq must be set to "epoch" as those metrics are only available at the end of an epoch. options (Optional[str]): Optional `tf.train.CheckpointOptions` object if `save_weights_only` is true or optional `tf.saved_model.SaveOptions` object if `save_weights_only` is false. initial_value_threshold (Optional[float]): Floating point initial "best" value of the metric to be monitored. |



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






