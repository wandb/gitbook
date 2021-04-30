---
description: >-
  Build scalable, structured, high-performance PyTorch models with Lightning and
  log them with W&B.
---

# PyTorch Lightning

PyTorch Lightning provides a lightweight wrapper for organizing your PyTorch code and easily adding advanced features such as [distributed training](https://pytorch-lightning.readthedocs.io/en/latest/multi_gpu.html) and [16-bit precision](https://pytorch-lightning.readthedocs.io/en/latest/amp.html). W&B provides a lightweight wrapper for logging your ML experiments. We're incorporated directly into the PyTorch Lightning library, so you can always check out [their documentation](https://pytorch-lightning.readthedocs.io/en/stable/extensions/generated/pytorch_lightning.loggers.WandbLogger.html#pytorch_lightning.loggers.WandbLogger) for the API and reference info.

## ⚡ Get going lightning-fast with just two lines:

```python
from pytorch_lightning.loggers import WandbLogger  # newline 1
from pytorch_lightning import Trainer

wandb_logger = WandbLogger()  # newline 2
trainer = Trainer(logger=wandb_logger)
```

## Check out **real** examples!

{% hint style="info" %}
Run GPU-accelerated PyTorch Lighting with W&B logging without installing anything in [this Colab](http://wandb.me/lit-colab), which includes a video walkthrough, linked below.
{% endhint %}

{% embed url="https://www.youtube.com/watch?v=hUXQm46TAKc" %}

Here are two more examples demonstrating some advanced features:

* [Semantic Segmentation with Lightning](https://app.wandb.ai/borisd13/lightning-kitti/reports/Lightning-Kitti--Vmlldzo3MTcyMw): optimize neural networks for self-driving cars
* [A step by step guide](https://app.wandb.ai/cayush/pytorchlightning/reports/Use-Pytorch-Lightning-with-Weights-%26-Biases--Vmlldzo2NjQ1Mw) to tracking your Lightning model performance

