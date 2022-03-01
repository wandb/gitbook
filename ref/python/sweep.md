# sweep



[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/v0.12.11/wandb/sdk/wandb_sweep.py#L31-L111)



Initialize a hyperparameter sweep.

```python
sweep(
    sweep: Union[dict, Callable],
    entity: str = None,
    project: str = None
) -> str
```




To generate hyperparameter suggestions from the sweep and use them
to train a model, call `wandb.agent` with the sweep_id returned by
this command. For command line functionality, see the command line
tool `wandb sweep` (https://docs.wandb.ai/ref/cli/wandb-sweep).

| Args |  |
| :--- | :--- |
|  `sweep` |  dict, SweepConfig, or callable. The sweep configuration (or configuration generator). If a dict or SweepConfig, should conform to the W&B sweep config specification (https://docs.wandb.ai/guides/sweeps/configuration). If a callable, should take no arguments and return a dict that conforms to the W&B sweep config spec. |
|  `entity` |  str (optional). An entity is a username or team name where you're sending runs. This entity must exist before you can send runs there, so make sure to create your account or team in the UI before starting to log runs. If you don't specify an entity, the run will be sent to your default entity, which is usually your username. Change your default entity in [Settings](wandb.ai/settings) under "default location to create new projects". |
|  `project` |  str (optional). The name of the project where you're sending the new run. If the project is not specified, the run is put in an "Uncategorized" project. |



| Returns |  |
| :--- | :--- |
|  `sweep_id` |  str. A unique identifier for the sweep. |



#### Examples:

Basic usage
<!--yeadoc-test:one-parameter-sweep-->
```python
import wandb
sweep_configuration = {
    "name": "my-awesome-sweep",
    "metric": {"name": "accuracy", "goal": "maximize"},
    "method": "grid",
    "parameters": {
        "a": {
            "values": [1, 2, 3, 4]
        }
    }
}

def my_train_func():
    # read the current value of parameter "a" from wandb.config
    wandb.init()
    a = wandb.config.a

    wandb.log({"a": a, "accuracy": a + 1})

sweep_id = wandb.sweep(sweep_configuration)

# run the sweep
wandb.agent(sweep_id, function=my_train_func)
```
