# Keras

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](http://wandb.me/intro-keras)

Use our callback to automatically save all the metrics and the loss values tracked in `model.fit`.

```python
import wandb
from wandb.keras import WandbCallback

wandb.init(config={"hyper": "parameter"})

...  # code to set up your model in Keras

# 🧙 magic
model.fit(X_train, y_train,  validation_data=(X_test, y_test),
          callbacks=[WandbCallback()])
```

## Usage Examples

{% hint style="info" %}
Try our integration out in a [colab notebook](http://wandb.me/keras-colab) (with video walkthrough below) or see our [example repo](https://github.com/wandb/examples) for scripts, including a [Fashion MNIST example](https://github.com/wandb/examples/blob/master/examples/keras/keras-cnn-fashion/train.py) and the [W\&B Dashboard](https://wandb.ai/wandb/keras-fashion-mnist/runs/5z1d85qs) it generates.
{% endhint %}

{% embed url="https://www.youtube.com/watch?v=Bsudo7jbMow" %}

## Configuring the `WandbCallback`

The `WandbCallback` class supports a wide variety of logging configuration options: specifying a metric to `monitor`, tracking of `weights` and `gradients`, logging of `predictions` on `training_data` and `validation_data`, and more.

Check out [the reference documentation for the `keras.WandbCallback`](../../ref/python/integrations/keras/wandbcallback.md) for details.

## Frequently Asked Questions

### **How do I use `Keras` multiprocessing with `wandb`?**

If you're setting `use_multiprocessing=True` and seeing an error like:

```python
Error('You must call wandb.init() before wandb.config.batch_size')
```

then try this:

1. In the `Sequence` class construction, add: `wandb.init(group='...')`
2. In your main program, make sure you're using `if __name__ == "__main__":` and then put the rest of your script logic inside that.
