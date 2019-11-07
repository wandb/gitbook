# Ray Tune

W&B integrates with [Ray](https://github.com/ray-project/ray) by providing a logger for use with RLlib or Tune runs.

### WandbLogger

```python
from ray import tunefrom wandb.ray import WandbLoggertune.run("PG", loggers=[WandbLogger], config={           "monitor": True, "env_config": {               "wandb": {"project": "my-project-name", "monitor_gym": True}}})
```

The "wandb" dictionary of env\_config will be passed to `wandb.init` when training starts. See the [wandb.init](../python/init.md) docs for all available options.

