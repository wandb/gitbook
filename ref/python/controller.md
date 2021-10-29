# controller



[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/a71719bdde474b8048d942c5b1be20afadaef59a/wandb/sdk/wandb_sweep.py#L97-L118)



Public sweep controller constructor.

```python
controller(
    sweep_id_or_config: Optional[Union[str, Dict]] = None,
    entity: Optional[str] = None,
    project: Optional[str] = None
)
```





#### Usage:

import wandb
tuner = wandb.controller(...)
print(tuner.sweep_config)
print(tuner.sweep_id)
tuner.configure_search(...)
tuner.configure_stopping(...)
