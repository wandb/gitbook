# wandb.controller

[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/v0.12.7/wandb/sdk/wandb\_sweep.py#L112-L133)

Public sweep controller constructor.

```python
controller(
    sweep_id_or_config: Optional[Union[str, Dict]] = None,
    entity: Optional[str] = None,
    project: Optional[str] = None
)
```

#### Usage:

```python
import wandb

tuner = wandb.controller(sweep_config)
print(tuner.sweep_config)

tuner.configure_search("grid")
tuner.configure_parameter("param", value=3)

tuner.create()
tuner.run()
```
