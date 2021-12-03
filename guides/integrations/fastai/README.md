# Fastai

If you're using **fastai** to train your models, W\&B has an easy integration using the `WandbCallback`. Explore the details in[ interactive docs with examples →](https://app.wandb.ai/borisd13/demo\_config/reports/Visualize-track-compare-Fastai-models--Vmlldzo4MzAyNA)

## Start Logging with W\&B

First install Weights & Biases and log in:

{% tabs %}
{% tab title="Notebook" %}
```python
!pip install wandb

import wandb
wandb.login()
```
{% endtab %}

{% tab title="Command Line" %}
```python
pip install wandb
wandb login
```
{% endtab %}
{% endtabs %}

Then add the callback to the `learner` or `fit` method:

```python
import wandb
from fastai.callback.wandb import *

# start logging a wandb run
wandb.init(project='my_project')

# To log only during one training phase
learn.fit(..., cbs=WandbCallback())

# To log continuously for all training phases
learn = learner(..., cbs=WandbCallback())
```

{% hint style="info" %}
If you use version 1 of Fastai, refer to the [Fastai v1 docs](v1.md).
{% endhint %}

## WandbCallback Arguments

`WandbCallback` accepts the following arguments:

| Args          | Description                                                                                                                                                                                                                        |
| ------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| log           | "gradients" (default), "parameters", "all" or None. Losses & metrics are always logged.                                                                                                                                            |
| log\_preds    | whether we want to log prediction samples (default to True).                                                                                                                                                                       |
| log\_model    | whether we want to log our model (default to True). This also requires SaveModelCallback.                                                                                                                                          |
| log\_dataset  | <ul><li>False (default)</li><li>True will log folder referenced by learn.dls.path.</li><li>a path can be defined explicitly to reference which folder to log.</li></ul><p><em>Note: subfolder "models" is always ignored.</em></p> |
| dataset\_name | name of logged dataset (default to folder name).                                                                                                                                                                                   |
| valid\_dl     | `DataLoaders` containing items used for prediction samples (default to random items from `learn.dls.valid`.                                                                                                                        |
| n\_preds      | number of logged predictions (default to 36).                                                                                                                                                                                      |
| seed          | used for defining random samples.                                                                                                                                                                                                  |

For custom workflows, you can manually log your datasets and models:

* `log_dataset(path, name=None, medata={})`
* `log_model(path, name=None, metadata={})`&#x20;

_Note: any subfolder "models" will be ignored._

## Examples

* [Visualize, track, and compare Fastai models](https://app.wandb.ai/borisd13/demo\_config/reports/Visualize-track-compare-Fastai-models--Vmlldzo4MzAyNA): A thoroughly documented walkthrough
* [Image Segmentation on CamVid](http://bit.ly/fastai-wandb): A sample use case of the integration
