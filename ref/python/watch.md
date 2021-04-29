# wandb.watch

[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/v0.10.28/wandb/sdk/wandb_watch.py#L19-L98)

Hooks into the torch model to collect gradients and the topology. Should be extended

```text
watch(
    models, criterion=None, log='gradients', log_freq=1000, idx=None
)
```

to accept arbitrary ML models.

| Args |  |
| :--- | :--- |
|  `models` |  \(torch.Module\) The model to hook, can be a tuple |
|  `criterion` |  \(torch.F\) An optional loss value being optimized |
|  `log` |  \(str\) One of "gradients", "parameters", "all", or None |
|  `log_freq` |  \(int\) log gradients and parameters every N batches |
|  `idx` |  \(int\) an index to be used when calling wandb.watch on multiple models |

| Returns |
| :--- |
|  `wandb.Graph` The graph object that will populate after the first backward pass |

