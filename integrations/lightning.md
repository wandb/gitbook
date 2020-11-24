---
description: Visualize PyTorch Lightning models with W&B
---

# PyTorch Lightning

PyTorch Lightning provides a lightweight wrapper for organizing your PyTorch code and easily adding advanced features such as [distributed training](https://pytorch-lightning.readthedocs.io/en/latest/multi_gpu.html) and [16-bit precision](https://pytorch-lightning.readthedocs.io/en/latest/amp.html). W&B provides a lightweight wrapper for logging your ML experiments. We're incorporated directly into the PyTorch Lightning library, so you can always check out [their documentation](https://pytorch-lightning.readthedocs.io/en/latest/loggers.html#weights-and-biases).

## ⚡Get going lightning-fast with just two lines:

```python
from pytorch_lightning.loggers import WandbLogger
from pytorch_lightning import Trainer

wandb_logger = WandbLogger()
trainer = Trainer(logger=wandb_logger)
```

## ✅ Check out **real** examples!

We've created a few examples for you to see how the integration works:

* [Run in a Google Colab](https://colab.research.google.com/github/wandb/examples/blob/master/colabs/pytorch-lightning/Supercharge_your_Training_with_Pytorch_Lightning_%2B_Weights_%26_Biases.ipynb) and try out the integration in a simple notebook
* [A step by step guide](https://app.wandb.ai/cayush/pytorchlightning/reports/Use-Pytorch-Lightning-with-Weights-%26-Biases--Vmlldzo2NjQ1Mw) to tracking your Lightning model performance
* [Semantic Segmentation with Lightning](https://app.wandb.ai/borisd13/lightning-kitti/reports/Lightning-Kitti--Vmlldzo3MTcyMw): optimize neural networks for self-driving cars

## **💻 API Reference**

### `WandbLogger`

Parameters:

* **name** \(_str_\) – display name for the run.
* **save\_dir** \(_str_\) – path where data is saved.
* **offline** \(_bool_\) – run offline \(data can be streamed later to wandb servers\).
* **version** \(_id_\) – sets the version, mainly used to resume a previous run.
* **anonymous** \(_bool_\) – enables or explicitly disables anonymous logging.
* **project** \(_str_\) – the name of the project to which this run will belong.
* **tags** \(_list of str_\) – tags associated with this run.

### **`WandbLogger.watch`**

Log model topology as well as optionally gradients and weights.

```python
wandb_logger.watch(model, log='gradients', log_freq=100)
```

Parameters:

* **model** \(_nn.Module_\) – model to be logged.
* **log** \(_str_\) – can be "gradients" \(default\), "parameters", "all" or None.
* **log\_freq** \(_int_\) – step count between logging of gradients and parameters.

### **`WandbLogger.log_hyperparams`**

Record hyperparameter configuration.

_Note: this function is called automatically by `Trainer`_

```python
wandb_logger.log_hyperparams(params)
```

Parameters:

* **params** \(dict\)  – dictionary with hyperparameter names as keys and configuration values as values

### `WandbLogger.log_metrics`

Record training metrics.

_Note: this function is called automatically by `Trainer`_

```python
wandb_logger.log_metrics(metrics, step=None)
```

Parameters:

* **metric** \(numeric\) – dictionary with metric names as keys and measured quantities as values
* **step** \(int\|None\) – step number at which the metrics should be recorded

\*\*\*\*

