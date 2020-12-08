# Fast.ai

If you're using **fastai** to train your models, W&B has an easy integration using the `WandbCallback`. Explore the details in[ interactive docs with examples →](https://app.wandb.ai/borisd13/demo_config/reports/Visualize-track-compare-Fastai-models--Vmlldzo4MzAyNA)

First install Weights & Biases and log in:

```text
pip install wandb
wandb login
```

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

`WandbCallback` accepts the following arguments:

<table>
  <thead>
    <tr>
      <th style="text-align:left">Args</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">log</td>
      <td style="text-align:left">&quot;gradients&quot; (default), &quot;parameters&quot;, &quot;all&quot;
        or None. Losses &amp; metrics are always logged.</td>
    </tr>
    <tr>
      <td style="text-align:left">log_preds</td>
      <td style="text-align:left">whether we want to log prediction samples (default to True).</td>
    </tr>
    <tr>
      <td style="text-align:left">log_model</td>
      <td style="text-align:left">whether we want to log our model (default to True). This also requires
        SaveModelCallback.</td>
    </tr>
    <tr>
      <td style="text-align:left">log_dataset</td>
      <td style="text-align:left">
        <ul>
          <li>False (default)</li>
          <li>True will log folder referenced by learn.dls.path.</li>
          <li>a path can be defined explicitly to reference which folder to log.</li>
        </ul>
        <p><em>Note: subfolder &quot;models&quot; is always ignored.</em>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">dataset_name</td>
      <td style="text-align:left">name of logged dataset (default to folder name).</td>
    </tr>
    <tr>
      <td style="text-align:left">valid_dl</td>
      <td style="text-align:left"><code>DataLoaders</code> containing items used for prediction samples (default
        to random items from <code>learn.dls.valid</code>.</td>
    </tr>
    <tr>
      <td style="text-align:left">n_preds</td>
      <td style="text-align:left">number of logged predictions (default to 36).</td>
    </tr>
    <tr>
      <td style="text-align:left">seed</td>
      <td style="text-align:left">used for defining random samples.</td>
    </tr>
  </tbody>
</table>

For custom workflows, you can manually log your datasets and models:

* `log_dataset(path, name=None, medata={})`
* `log_model(path, name=None, metadata={})` 

_Note: any subfolder "models" will be ignored._

## Examples

* [Visualize, track, and compare Fastai models](https://app.wandb.ai/borisd13/demo_config/reports/Visualize-track-compare-Fastai-models--Vmlldzo4MzAyNA): A thoroughly documented walkthrough
* [Image Segmentation on CamVid](http://bit.ly/fastai-wandb): A sample use case of the integration

