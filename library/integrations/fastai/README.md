# fastai

_Note: this documentation is for the current version of fastai.  
If you use fastai v1, you should refer to_ [_fastai v1 page_](fastai.md)_._

## Use of WandbCallback in fastai

W&B is integrated into fastai through `WandbCallback`.

First install wandb and login.

```text
pip install wandb
wandb login
```

Then you add the callback to your learner or call to fit methods:

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
      <td style="text-align:left">log_preds (bool)</td>
      <td style="text-align:left">whether we want to log prediction samples (default to True).</td>
    </tr>
    <tr>
      <td style="text-align:left">log_model (bool)</td>
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

Refer to[ fastai report](https://app.wandb.ai/borisd13/demo_config/reports/Compare-monitor-fastai-models--Vmlldzo4MzAyNA) which contains extensive documentation on how to use W&B integration.

## Examples

* [Image Segmentation on CamVid](https://colab.research.google.com/gist/borisdayma/6b83c8b7078610d71708b036421a3591/fastai-wandbcallback.ipynb)

