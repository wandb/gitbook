---
description: Visualize PyTorch Lightning models with W&B
---

# Lightning

PyTorch Lightning provides a lightweight wrapper for organizing your PyTorch code and easily add advanced features such as distributed training and 16-bit precising. We have a nice integration to visualize your results.

```python
from pytorch_lightning.loggers import WandbLogger
from pytorch_lightning import Trainer

wandb_logger = WandbLogger()
trainer = Trainer(logger=wandb_logger)
```

**Parameters**

* **name** \([_str_](https://docs.python.org/3/library/stdtypes.html#str)\) – display name for the run.
* **save\_dir** \([_str_](https://docs.python.org/3/library/stdtypes.html#str)\) – path where data is saved.
* **offline** \([_bool_](https://docs.python.org/3/library/functions.html#bool)\) – run offline \(data can be streamed later to wandb servers\).
* **version** \(_id_\) – sets the version, mainly used to resume a previous run.
* **anonymous** \([_bool_](https://docs.python.org/3/library/functions.html#bool)\) – enables or explicitly disables anonymous logging.
* **project** \([_str_](https://docs.python.org/3/library/stdtypes.html#str)\) – the name of the project to which this run will belong.
* **tags** \(_list of str_\) – tags associated with this run.


**Log model topology and gradients**

Log model topology as well as optionally gradients and weights.

```python
wandb_logger.watch(model, log='gradients', log_freq=100)
```

Parameters:

* **model** \(nn.Module\) – Model to be logged
* **log** \(str\) – Can be "gradients" (default), "parameters", "all" or None.
* **log_freq** \(int\) – Step number at which the metrics should be recorded


**Hyperparameters**

Record hyperparameters.

*Note: this function is called automatically*

```python
wandb_logger.log_hyperparams(params)
```
Parameters: **params** – argparse.Namespace containing the hyperparameters (should be a dict).


**Metrics**

Record metrics.

*Note: this function is called automatically*

```python
wandb_logger.log_metrics(metrics, step=None)
```

Parameters:

* **metric** \(float\) – Dictionary with metric names as keys and measured quantities as values
* **step** \(int\|None\) – Step number at which the metrics should be recorded
