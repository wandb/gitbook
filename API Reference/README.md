# Module: wandb

<!-- Insert buttons and diff -->


[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/master/wandb/__init__.py)




Wandb is a library to help track machine learning experiments.


For more information on wandb see https://docs.wandb.com.

The most commonly used functions/objects are:
- wandb.init — initialize a new run at the top of your training script
- wandb.config — track hyperparameters
- wandb.log — log metrics over time within your training loop
- wandb.save — save files in association with your run, like model weights
- wandb.restore — restore the state of your code when you ran a given run

For examples usage, see github.com/wandb/examples

## Classes

[`class Api`](./wandb/Api.md): Used for querying the wandb server.

## Functions

[`agent(...)`](./wandb/agent.md): Generic agent entrypoint, used for CLI or jupyter.

[`config(...)`](./wandb/config.md): Config object

[`finish(...)`](./wandb/finish.md): Marks a run as finished, and finishes uploading all data.

[`init(...)`](./wandb/init.md): Start a new tracked run with <a href="./wandb/init.md"><code>wandb.init()</code></a>.

[`log(...)`](./wandb/log.md): Log a dict to the global run's history.

[`login(...)`](./wandb/login.md): Log in to W&B.

[`save(...)`](./wandb/save.md): Ensure all files matching *glob_str* are synced to wandb with the policy specified.

[`summary(...)`](./wandb/summary.md): Summary tracks single values for each run. By default, summary is set to the

