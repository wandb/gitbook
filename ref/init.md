# Init

[wandb.init\(\)](https://docs.wandb.ai/ref/init) indica el comienzo de una nueva ejecución. En un pipeline de entrenamiento de ML, podrías agregar wandb.init\(\) al comienzo de tu script de entrenamiento, así también como al comienzo de tu script de evaluación, y cada paso sería rastreado como una ejecución en W&B.

## init

`def init( job_type: Optional[str] = None, dir=None, config: Union[Dict, str, None] = None, project: Optional[str] = None, entity: Optional[str] = None, reinit: bool = None, tags: Optional[Sequence] = None, group: Optional[str] = None, name: Optional[str] = None, notes: Optional[str] = None, magic: Union[dict, str, bool] = None, config_exclude_keys=None, config_include_keys=None, anonymous: Optional[str] = None, mode: Optional[str] = None, allow_val_change: Optional[bool] = None, resume: Optional[Union[bool, str]] = None, force: Optional[bool] = None, tensorboard=None, # alias for sync_tensorboard sync_tensorboard=None, monitor_gym=None, save_code=None, id=None, settings: Union[Settings, Dict[str, Any], None] = None, ) -> Union[Run, Dummy]: Union[Run, Dummy]`

[![Badge](https://img.shields.io/badge/SOURCE-black?style=plastic&logo=github)](https://github.com/wandb/client/tree/master/wandb/sdk/wandb_init.py#L456-#L613)

Initialize W&B Spawns a new process to start or resume a run locally and communicate with a wandb server. Should be called before any calls to wandb.log.

**Arguments**

config \(dict, argparse, or absl.flags, str, optional\): Sets the config parameters \(typically hyperparameters\) to store with the run. See also wandb.config.

| **Filed** | **Type** | **Description** |
| :--- | :--- | :--- |
| job\_type | \(str, optional\) | The type of job running, defaults to 'train' |
| dir | \(str, optional\) | An absolute path to a directory where metadata will be stored. |
| flags |  | will load the key value pairs into the runs config object. If str: will look for a yaml file that includes config parameters and load them into the run's config object. |
| project | \(str, optional\) | W&B Project. |
| entity | \(str, optional\) | W&B Entity. |
| reinit | \(bool, optional\) | Allow multiple calls to init in the same process. |
| tags | \(list, optional\) | A list of tags to apply to the run. |
| group | \(str, optional\) | A unique string shared by all runs in a given group. |
| name | \(str, optional\) | A display name for the run which does not have to be unique. |
| notes | \(str, optional\) | A multiline string associated with the run. |
| magic | \(bool, dict, or str, optional\) | magic configuration as bool, dict, json string, yaml filename. |
| config\_exclude\_keys | \(list, optional\) | string keys to exclude storing in W&B when specifying config. |
| config\_include\_keys | \(list, optional\) | string keys to include storing in W&B when specifying config. |
| anonymous | \(str, optional\) | Can be "allow", "must", or "never". Controls whether anonymous logging is allowed. Defaults to never. |
| mode | \(str, optional\) | Can be "online", "offline" or "disabled". Defaults to online. |
| allow\_val\_change | \(bool, optional\) | allow config values to be changed after setting. Defaults to true in jupyter and false otherwise. |
| resume | \(bool, str, optional\) | Sets the resuming behavior. Should be one of: "allow", "must", "never", "auto" or None. Defaults to None. Cases: - "auto" \(or True\): automatically resume the previous run on the same machine. if the previous run crashed, otherwise starts a new run. - "allow": if id is set with init\(id="UNIQUE\_ID"\) or WANDB\_RUN\_ID="UNIQUE\_ID" and it is identical to a previous run, wandb will automatically resume the run with the id. Otherwise wandb will start a new run. - "never": if id is set with init\(id="UNIQUE\_ID"\) or WANDB\_RUN\_ID="UNIQUE\_ID" and it is identical to a previous run, wandb will crash. - "must": if id is set with init\(id="UNIQUE\_ID"\) or WANDB\_RUN\_ID="UNIQUE\_ID" and it is identical to a previous run, wandb will automatically resume the run with the id. Otherwise wandb will crash. - None: never resumes - if a run has a duplicate run\_id the previous run is overwritten. See [https://docs.wandb.com/library/advanced/resuming](https://docs.wandb.com/library/advanced/resuming) for more detail. |
| force | \(bool, optional\) | If true, will cause script to crash if user can't or isn't logged in to a wandb server. If false, will cause script to run in offline modes if user can't or isn't logged in to a wandb server. Defaults to false. |
| sync\_tensorboard | \(bool, optional\) | Synchronize wandb logs from tensorboard or tensorboardX and saves the relevant events file. Defaults to false. |
| monitor\_gym |  | \(bool, optional\): automatically logs videos of environment when using OpenAI Gym \(see [https://docs.wandb.com/library/integrations/openai-gym](https://docs.wandb.com/library/integrations/openai-gym)\) Defaults to false. |
| save\_code | \(bool, optional\) | Save the entrypoint or jupyter session history source code. |
| id | \(str, optional\) | A globally unique \(per project\) identifier for the run. This is primarily used for resuming. |

**Examples**

Basic usage

```python
wandb.init()
```

Launch multiple runs from the same script

```python
for x in range(10):
    with wandb.init(project="my-projo") as run:
        for y in range(100):
            run.log({"metric": x+y})
```

**Raises**

| **Filed** | **Type** | **Description** |
| :--- | :--- | :--- |
| Exception |  | if problem. |

**Returns**

A `Run` object.

## \_set\_logger

`def _set_logger(log_object):`

[![Badge](https://img.shields.io/badge/SOURCE-black?style=plastic&logo=github)](https://github.com/wandb/client/tree/master/wandb/sdk/wandb_init.py#L40-#L43)

Configure module logger.

## \_WandbInit

### setup

`def setup(self, kwargs):`

[![Badge](https://img.shields.io/badge/SOURCE-black?style=plastic&logo=github)](https://github.com/wandb/client/tree/master/wandb/sdk/wandb_init.py#L62-#L158)

Complete setup for wandb.init\(\). This includes parsing all arguments, applying them with settings and enabling logging.

### \_enable\_logging

`def _enable_logging(self, log_fname, run_id=None):`

[![Badge](https://img.shields.io/badge/SOURCE-black?style=plastic&logo=github)](https://github.com/wandb/client/tree/master/wandb/sdk/wandb_init.py#L167-#L200)

Enable logging to the global debug log. This adds a run\_id to the log, in case of muliple processes on the same machine. Currently there is no way to disable logging after it's enabled.

### \_jupyter\_teardown

`def _jupyter_teardown(self):`

[![Badge](https://img.shields.io/badge/SOURCE-black?style=plastic&logo=github)](https://github.com/wandb/client/tree/master/wandb/sdk/wandb_init.py#L232-#L245)

Teardown hooks and display saving, called with wandb.finish

### \_jupyter\_setup

`def _jupyter_setup(self, settings):`

[![Badge](https://img.shields.io/badge/SOURCE-black?style=plastic&logo=github)](https://github.com/wandb/client/tree/master/wandb/sdk/wandb_init.py#L247-#L269)

Add magic, hooks, and session history saving

### \_log\_setup

`def _log_setup(self, settings):`

[![Badge](https://img.shields.io/badge/SOURCE-black?style=plastic&logo=github)](https://github.com/wandb/client/tree/master/wandb/sdk/wandb_init.py#L271-#L304)

Set up logging from settings.

