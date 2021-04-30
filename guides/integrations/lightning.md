---
description: >-
  Build scalable, structured, high-performance PyTorch models with Lightning and
  log them with W&B.
---

# PyTorch Lightning

PyTorch Lightning provides a lightweight wrapper for organizing your PyTorch code and easily adding advanced features such as [distributed training](https://pytorch-lightning.readthedocs.io/en/latest/multi_gpu.html) and [16-bit precision](https://pytorch-lightning.readthedocs.io/en/latest/amp.html). W&B provides a lightweight wrapper for logging your ML experiments. We're incorporated directly into the PyTorch Lightning library, so you can always check out [their documentation](https://pytorch-lightning.readthedocs.io/en/stable/extensions/generated/pytorch_lightning.loggers.WandbLogger.html#pytorch_lightning.loggers.WandbLogger) for the API and reference info.

## ⚡ Get going lightning-fast with just two lines.

```python
from pytorch_lightning.loggers import WandbLogger  # newline 1
from pytorch_lightning import Trainer

wandb_logger = WandbLogger()  # newline 2
trainer = Trainer(logger=wandb_logger)
```

![Interactive dashboards accessible anywhere, and more, in just two lines!](../../.gitbook/assets/n6p7k4m.gif)

## Check out **real** examples!

{% tabs %}
{% tab title="Colab + Video Tutorial" %}
Run GPU-accelerated PyTorch Lighting plus W&B logging without installing anything using [this Colab](http://wandb.me/lit-colab). And follow along with a video tutorial!

{% embed url="https://www.youtube.com/watch?v=hUXQm46TAKc" %}
{% endtab %}

{% tab title="Kaggle Kernel" %}
See how PyTorch Lighting and W&B can accelerate your model development and help you climb the leaderboard with [this Kaggle Kernel](https://www.kaggle.com/ayuraj/use-pytorch-lightning-with-weights-and-biases).

![](../../.gitbook/assets/lgklnrt.gif)
{% endtab %}

{% tab title="Blog Posts" %}
Read more on specific topics in these blog posts made with Weights & Biases' [Reports](../reports.md):

* [Multi-GPU Training](https://wandb.ai/wandb/wandb-lightning/reports/Multi-GPU-Training-Using-PyTorch-Lightning--VmlldzozMTk3NTk)
* [Image Classification](https://wandb.ai/wandb/wandb-lightning/reports/Image-Classification-using-PyTorch-Lightning--VmlldzoyODk1NzY) and [Semantic Segmentation](https://wandb.ai/borisd13/lightning-kitti/reports/Lightning-Kitti--Vmlldzo3MTcyMw)
* [Transfer Learning](https://wandb.ai/wandb/wandb-lightning/reports/Transfer-Learning-Using-PyTorch-Lightning--VmlldzoyODk2MjA)
{% endtab %}
{% endtabs %}

