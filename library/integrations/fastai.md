# Fast.ai

For scripts using fast.ai, we have a callback that can automatically log model topology, losses, metrics, weights, gradients, sample predictions and best trained model.

```python
import wandb
from wandb.fastai import WandbCallback

wandb.init()

learn = cnn_learner(data,
                    model,
                    callback_fns=WandbCallback)
learn.fit(epochs)
```

Requested logged data is configurable through the callback constructor.

```python
from functools import partial

learn = cnn_learner(data, model, callback_fns=partial(WandbCallback, input_type='images'))
```

It is also possible to use WandbCallback only when starting training. In this case it must be instantiated.

```python
learn.fit(epochs, callbacks=WandbCallback(learn))
```

Custom parameters can also be given at that stage.

```python
learn.fit(epochs, callbacks=WandbCallback(learn, input_type='images'))
```

## Example Code

We've created a few examples for you to see how the integration works:

**Fastai v1**

* [Classify Simpsons characters](https://github.com/borisdayma/simpsons-fastai)[: ](https://app.wandb.ai/jxmorris12/huggingface-demo/reports/A-Step-by-Step-Guide-to-Tracking-Hugging-Face-Model-Performance--VmlldzoxMDE2MTU)A simple demo to track and compare Fastai models
* [Semantic Segmentation with Fastai](https://github.com/borisdayma/semantic-segmentation): Optimize neural networks on self-driving cars

**Fastai v2**

* [Track and compare](https://app.wandb.ai/borisd13/demo_config/reports/Compare-%26-monitor-fastai2-models--Vmlldzo4MzAyNA) Fastai model performance, and visualize results in a live dashboard
* [Run in colab](https://colab.research.google.com/gist/borisdayma/c3f5576b68c4ec422c41bca304a860ba/fastai2-demo.ipynb): A simple notebook example to get you started

## Options

`WandbCallback()` class supports a number of options:

| Keyword argument | Default | Description |
| :--- | :--- | :--- |
| learn | N/A | the fast.ai learner to hook. |
| save\_model | True | save the model if it's improved at each step. It will also load best model at the end of training. |
| mode | auto | 'min', 'max', or 'auto': How to compare the training metric specified in `monitor` between steps. |
| monitor | None | training metric used to measure performance for saving the best model. None defaults to validation loss. |
| log | gradients | "gradients", "parameters", "all", or None. Losses & metrics are always logged. |
| input\_type | None | "images" or None. Used to display sample predictions. |
| validation\_data | None | data used for sample predictions if input\_type is set. |
| predictions | 36 | number of predictions to make if input\_type is set and validation\_data is None. |
| seed | 12345 | initialize random generator for sample predictions if input\_type is set and validation\_data is None. |

## Example

We've created a few examples for you to see how the integration works:

* [Example on Github](https://github.com/wandb/examples/blob/master/tf-estimator-mnist/mnist.py): U-Net segmentation example
* [Wandb Dashboard](https://app.wandb.ai/wandb/witness/runs/uy25i7te?workspace=user-cayush): View result on W&B

