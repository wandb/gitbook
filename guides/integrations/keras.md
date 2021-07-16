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

## Data Visualization with W&B Tables

Use [W&B Tables](https://docs.wandb.ai/guides/data-vis) to log, query, and analyze your data. You can think of a W&B Table as a `DataFrame` that you can interact with inside W&B. Tables support rich media types, primitive and numeric types, as well as nested lists and dictionaries. 

This pseudo-code shows you how to log images, along with their ground truth and predicted class, to W&B Tables:

```python
# Create a new W&B Run
wandb.init(project="mnist")

# Create a W&B Table
my_table = wandb.Table(columns=["id", "image", "labels", "prediction"])

# Get your image data and make predictions
image_tensors, labels = get_mnist_data()
predictions = model(image_tensors)

# Add your image data and predictions to the W&B Table
for idx, im in enumerate(image_tensors): 
  my_table.add_data(idx, wandb.Image(im), labels[idx], predictions[id])

# Log your Table to W&B
wandb.log({"mnist_predictions": my_table})
```

This is will produce a Table like this:

![](../../.gitbook/assets/screenshot-2021-07-14-at-20.18.39.png)

For more examples of data visualization with W&B Tables, please see [the documentation](https://docs.wandb.ai/guides/data-vis).

## Common Questions

### **How do I use Keras multiprocessing with wandb?**

If you're setting `use_multiprocessing=True` and seeing the error `Error('You must call wandb.init() before wandb.config.batch_size')` then try this:

1. In the `Sequence` class init, add: `wandb.init(group='...')` 
2. In your main program, make sure you're using `if __name__ == "__main__":` and then put the rest of your script logic inside that.

