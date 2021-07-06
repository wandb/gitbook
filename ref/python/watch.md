# wandb.watch

[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/v0.10.33/wandb/sdk/wandb_watch.py#L17-L99)

Hooks into the torch model to collect gradients and the topology.

```python
watch(
    models, criterion=None, log="gradients", log_freq=1000, idx=None
)
```

Should be extended to accept arbitrary ML models.

| Args |  |
| :--- | :--- |
| `models` | \(torch.Module\) The model to hook, can be a tuple |
| `criterion` | \(torch.F\) An optional loss value being optimized |
| `log` | \(str\) One of "gradients", "parameters", "all", or None |
| `log_freq` | \(int\) log gradients and parameters every N batches |
| `idx` | \(int\) an index to be used when calling wandb.watch on multiple models |

| Returns |  |
| :--- | :--- |
| `wandb.Graph` The graph object that will populate after the first backward pass |  |

| Raises |  |
| :--- | :--- |
| `ValueError` | If called before `wandb.init` or if any of models is not a torch.nn.Module. |

