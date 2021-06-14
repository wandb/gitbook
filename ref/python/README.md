# python

<!-- Insert buttons and diff -->


[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/v0.10.32/wandb/__init__.py)







## Classes

[`class Artifact`](./artifact.md): Flexible and lightweight building block for dataset and model versioning.

[`class Run`](./run.md): A unit of computation logged by wandb. Typically this is an ML experiment.

## Functions

[`agent(...)`](./agent.md): Generic agent entrypoint, used for CLI or jupyter.

[`config(...)`](./config.md): Config object

[`finish(...)`](./finish.md): Marks a run as finished, and finishes uploading all data.

[`init(...)`](./init.md): Start a new tracked run with `wandb.init()`.

[`log(...)`](./log.md): Log a dict to the global run's history.

[`save(...)`](./save.md): Ensure all files matching `glob_str` are synced to wandb with the policy specified.

[`summary(...)`](./summary.md): Tracks single values for each metric for each run.

[`sweep(...)`](./sweep.md): Initialize a hyperparameter sweep.

[`watch(...)`](./watch.md): Hooks into the torch model to collect gradients and the topology.



| Other Members |  |
| :--- | :--- |
|  __version__<a id="__version__"></a> |  `'0.10.32'` |

