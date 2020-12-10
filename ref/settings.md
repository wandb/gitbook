# Settings

## wandb.sdk.wandb\_settings

[\[view\_source\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_settings.py#L2)

This module configures settings for wandb runs.

Order of loading settings: \(differs from priority\) defaults environment wandb.setup\(settings=\) system\_config workspace\_config wandb.init\(settings=\) network\_org network\_entity network\_project

Priority of settings: See "source" variable.

When override is used, it has priority over non-override settings

Override priorities are in the reverse order of non-override settings

### Settings Objects

```python
class Settings(object)
```

[\[view\_source\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_settings.py#L187)

Settings Constructor

**Arguments**:

* `entity` - personal user or team to use for Run.
* `project` - project name for the Run.

**Raises**:

* `Exception` - if problem.

**\_\_copy\_\_**

```python
 | __copy__()
```

[\[view\_source\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_settings.py#L656)

Copy \(note that the copied object will not be frozen\).

