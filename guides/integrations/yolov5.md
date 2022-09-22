---
description: You'll Easily Log Everything if you combine W&B with YOLOv5.
---

# YOLOv5

[Ultralytics' YOLOv5](https://ultralytics.com/yolov5) ("You Only Look Once") model family enables real-time object detection with convolutional neural networks without all the agonizing pain.

[Weights & Biases](http://wandb.com) is directly integrated into YOLOv5, providing experiment metric tracking, model and dataset versioning, rich model prediction visualization, and more. **It's as easy as running a single `pip install` before you run your YOLO experiments!**

{% hint style="info" %}
For a quick overview of the model and data-logging features of our YOLOv5 integration, check out [this Colab](https://wandb.me/yolo-colab) and accompanying video tutorial, linked below.
{% endhint %}

{% embed url="https://www.youtube.com/watch?v=yyecuhBmLxE" %}

{% hint style="info" %}
All W\&B logging features are compatible with data-parallel multi-GPU training, e.g. with [PyTorch DDP](https://pytorch.org/tutorials/intermediate/ddp\_tutorial.html).
{% endhint %}

## Core Experiment Tracking

Simply by installing `wandb`, you'll activate the built-in W\&B [logging features](../track/log/): system metrics, model metrics, and media logged to interactive [Dashboards](../track/app.md).

```python
pip install wandb
git clone https://github.com/ultralytics/yolov5.git
python yolov5/train.py  # train a small network on a small dataset
```

Just follow the links printed to the standard out by wandb.

![All these charts and more!](<../../.gitbook/assets/image (105).png>)

## Model Versioning and Data Visualization

But that's not all! By passing a few simple command line arguments to YOLO, you can take advantage of even more W\&B features.

* Passing a number to `--save_period` will turn on [model versioning](../data-and-model-versioning/model-versioning.md). At the end of every `save_period` epochs, the model weights will be saved to W\&B. The best-performing model on the validation set will be tagged automatically.
* Turning on the `--upload_dataset` flag will also upload the dataset for [data versioning](../data-and-model-versioning/dataset-versioning.md).
* Passing a number to `--bbox_interval` will turn on [data visualization](../data-vis/). At the end of every `bbox_interval` epochs, the outputs of the model on the validation set will be uploaded to W\&B.

{% tabs %}
{% tab title="Model Versioning Only" %}
```python
python yolov5/train.py --epochs 20 --save_period 1
```
{% endtab %}

{% tab title="Model Versioning and Data Visualization" %}
```python
python yolov5/train.py --epochs 20 --save_period 1 \
  --upload_dataset --bbox_interval 1
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Every W\&B account comes with 100 GB of free storage for datasets and models.
{% endhint %}

Here's what that looks like.

![Model Versioning: the latest and the best versions of the model are identified.](<../../.gitbook/assets/image (109).png>)

![Data Visualization: compare the input image to the model's outputs and example-wise metrics.](<../../.gitbook/assets/image (110).png>)

{% hint style="info" %}
With data and model versioning, you can resume paused or crashed experiments from any device, no setup necessary! Check out [the Colab ](https://wandb.me/yolo-colab)for details.
{% endhint %}

### Learn more about versioning and visualization:

{% content-ref url="broken-reference" %}
[Broken link](broken-reference)
{% endcontent-ref %}

{% content-ref url="../data-vis/" %}
[data-vis](../data-vis/)
{% endcontent-ref %}
