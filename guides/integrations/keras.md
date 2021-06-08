---
description: How to integrate a Keras script to log metrics to W&B
---

# Keras

Use our callback to automatically save all the metrics and the loss values tracked in `model.fit`.

```python
import wandb
from wandb.keras import WandbCallback

wandb.init(config={"hyper": "parameter"})

# Magic
model.fit(X_train, y_train,  validation_data=(X_test, y_test),
          callbacks=[WandbCallback()])
```

## Usage Examples

{% hint style="info" %}
Try our integration out in a [colab notebook](http://wandb.me/keras-colab) \(with video walkthrough below\) or see our [example repo](https://github.com/wandb/examples) for scripts, including a [Fashion MNIST example](https://github.com/wandb/examples/blob/master/examples/keras/keras-cnn-fashion/train.py) and the [W&B Dashboard](https://wandb.ai/wandb/keras-fashion-mnist/runs/5z1d85qs) it generates.
{% endhint %}

{% embed url="https://www.youtube.com/watch?v=Bsudo7jbMow" caption="" %}

## Using the `WandbCallback`

The `WandbCallback()` class supports a number of options:

| Keyword argument | Default | Description |
| :--- | :--- | :--- |
| `monitor` | `val_loss` | The training metric used to measure performance for saving the best model. i.e. `val_loss` |
| `mode` | `auto` | `'min'`, `'max',` or `'auto'`: How to compare the training metric specified in `monitor` between steps |
| `save_weights_only` | `False` | only save the weights instead of the entire model |
| `save_model` | `True` | save the model if it's improved at each step |
| `log_weights` | `False` | log the values of each layers parameters at each epoch |
| `log_gradients` | `False` | log the gradients of each layers parameters at each epoch |
| `training_data` | `None` | tuple `(X,y)` needed for calculating gradients |
| `data_type` | `None` | the type of data we're saving, currently only `"image"` is supported |
| `labels` | `None` | only used if `data_type` is specified, list of labels to convert numeric output to if you are building classifier. \(supports binary classification\) |
| `predictions` | `36` | the number of predictions to make if `data_type` is specified. Max is `100`. |
| `generator` | `None` | if using data augmentation and `data_type` you can specify a generator to make predictions with. |

## Common Questions

### **How do I use Keras multiprocessing with wandb?**

If you're setting `use_multiprocessing=True` and seeing the error `Error('You must call wandb.init() before wandb.config.batch_size')` then try this:

1. In the `Sequence` class init, add: `wandb.init(group='...')` 
2. In your main program, make sure you're using `if __name__ == "__main__":` and then put the rest of your script logic inside that.

## 

