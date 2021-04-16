# Skorch

You can use Weights & Biases with Skorch to automatically log the model with the best performance – along with all model performance metrics, the model topology and compute resources after each epoch. Every file saved in wandb\_run.dir is automatically logged to W&B servers.

See [example run](https://app.wandb.ai/borisd13/skorch/runs/s20or4ct?workspace=user-borisd13).

## **Parameters**

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Parameter</b>
      </th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">
        <p><b>wandb_run</b>:</p>
        <p>wandb.wandb_run.Run</p>
      </td>
      <td style="text-align:left">wandb run used to log data.</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>save_model<br /></b>bool (default=True)</td>
      <td style="text-align:left">Whether to save a checkpoint of the best model and upload it to your Run
        on W&amp;B servers.</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>keys_ignored<br /></b>str or list of str (default=None)</td>
      <td style="text-align:left">Key or list of keys that should not be logged to tensorboard. Note that
        in addition to the keys provided by the user, keys such as those starting
        with &#x2018;event_&#x2019; or ending on &#x2018;_best&#x2019; are ignored
        by default.</td>
    </tr>
  </tbody>
</table>

## Example Code

We've created a few examples for you to see how the integration works:

* [Colab](https://colab.research.google.com/drive/1Bo8SqN1wNPMKv5Bn9NjwGecBxzFlaNZn?usp=sharing): A simple demo to try the integration
* [A step by step guide](https://app.wandb.ai/cayush/uncategorized/reports/Automate-Kaggle-model-training-with-Skorch-and-W%26B--Vmlldzo4NTQ1NQ): to tracking your Skorch model performance

```python
# Install wandb
... pip install wandb

import wandb
from skorch.callbacks import WandbLogger

# Create a wandb Run
wandb_run = wandb.init()
# Alternative: Create a wandb Run without a W&B account
wandb_run = wandb.init(anonymous="allow")

# Log hyper-parameters (optional)
wandb_run.config.update({"learning rate": 1e-3, "batch size": 32})

net = NeuralNet(..., callbacks=[WandbLogger(wandb_run)])
net.fit(X, y)
```

## Methods

| Method | Description |
| :--- | :--- |
| `initialize`\(\) | \(Re-\)Set the initial state of the callback. |
| `on_batch_begin`\(net\[, X, y, training\]\) | Called at the beginning of each batch. |
| `on_batch_end`\(net\[, X, y, training\]\) | Called at the end of each batch. |
| `on_epoch_begin`\(net\[, dataset\_train, …\]\) | Called at the beginning of each epoch. |
| `on_epoch_end`\(net, \*\*kwargs\) | Log values from the last history step and save best model |
| `on_grad_computed`\(net, named\_parameters\[, X, …\]\) | Called once per batch after gradients have been computed but before an update step was performed. |
| `on_train_begin`\(net, \*\*kwargs\) | Log model topology and add a hook for gradients |
| `on_train_end`\(net\[, X, y\]\) | Called at the end of training. |

