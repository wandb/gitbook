---
description: Visualize PyTorch Lightning models with W&B
---

# Lightning

PyTorch Lightning provides a very simple template for organizing your PyTorch code, and we have a nice integration to visualize your results.

```python
from pytorch_lightning.logging.wandb import WandbLogger
from pytorch_lightning import Trainer

wandb_logger = WandbLogger()
trainer = Trainer(logger=wandb_logger)
```

**Parameters**

* **name** \([_str_](https://docs.python.org/3/library/stdtypes.html#str)\) – display name for the run.
* **save\_dir** \([_str_](https://docs.python.org/3/library/stdtypes.html#str)\) – path where data is saved.
* **offline** \([_bool_](https://docs.python.org/3/library/functions.html#bool)\) – run offline \(data can be streamed later to wandb servers\).
* **or version** \(_id_\) – sets the version, mainly used to resume a previous run.
* **anonymous** \([_bool_](https://docs.python.org/3/library/functions.html#bool)\) – enables or explicitly disables anonymous logging.
* **project** \([_str_](https://docs.python.org/3/library/stdtypes.html#str)\) – the name of the project to which this run will belong.
* **tags** \(_list of str_\) – tags associated with this run.

```python
finalize(status='success')
```

Do any processing that is necessary to finalize an experiment.

Parameters: **status** – Status that the experiment finished with \(e.g. success, failed, aborted\)

```python
log_hyperparams(params)
```

Record hyperparameters.

Parameters: **params** – argparse.Namespace containing the hyperparameters

```python
log_metrics(metrics, step=None)
```

Record metrics.

Parameters:	

* **metric** \(float\) – Dictionary with metric names as keys and measured quantities as values
* **step** \(int\|None\) – Step number at which the metrics should be recorded

```python
save()
```

Save log data.

```python
watch(model, log='gradients', log_freq=100)
```

```python
_abc_impl = <_ABC_DATA OBJECT>
```

```text
experiment
```

Actual wandb object. To use wandb features do the following.

Example: `self.logger.experiment.some_wandb_function()`

```text
name
```

Return the experiment name.

```text
version
```

Return the experiment version.



