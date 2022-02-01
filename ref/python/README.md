# python

<!-- Insert buttons and diff -->


[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/latest/wandb/__init__.py)



Use wandb to track machine learning work.


The most commonly used functions/objects are:
  - wandb.init — initialize a new run at the top of your training script
  - wandb.config — track hyperparameters and metadata
  - wandb.log — log metrics and media over time within your training loop

For guides and examples, see https://docs.wandb.com/guides.

For scripts and interactive notebooks, see https://github.com/wandb/examples.

For reference documentation, see https://docs.wandb.com/ref/python.

## Classes

[`class Artifact`](./artifact.md): Flexible and lightweight building block for dataset and model versioning.

[`class Run`](./run.md): A unit of computation logged by wandb. Typically this is an ML experiment.

## Functions

[`agent(...)`](./agent.md): Generic agent entrypoint, used for CLI or jupyter.

[`config(...)`](./config.md): Config object

[`controller(...)`](./controller.md): Public sweep controller constructor.

[`finish(...)`](./finish.md): Marks a run as finished, and finishes uploading all data.

[`init(...)`](./init.md): Starts a new run to track and log to W&B.

[`log(...)`](./log.md): Logs a dictonary of data to the current run's history.

[`save(...)`](./save.md): Ensure all files matching `glob_str` are synced to wandb with the policy specified.

[`summary(...)`](./summary.md): Tracks single values for each metric for each run.

[`sweep(...)`](./sweep.md): Initialize a hyperparameter sweep.

[`watch(...)`](./watch.md): Hooks into the torch model to collect gradients and the topology.



| Other Members |  |
| :--- | :--- |
|  `__version__`<a id="__version__"></a> |  `'0.12.9'` |

