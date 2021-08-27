# wandb.controller

[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/v0.12.1/wandb/sdk/wandb_sweep.py#L97-L118)

Public sweep controller constructor.

```python
controller(
    sweep_id_or_config: Optional[Union[str, Dict]] = None,
    entity: Optional[str] = None,
    project: Optional[str] = None
)
```

## Usage:

import wandb tuner = wandb.controller\(...\) print\(tuner.sweep\_config\) print\(tuner.sweep\_id\) tuner.configure\_search\(...\) tuner.configure\_stopping\(...\)

