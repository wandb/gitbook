# Python Library

[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/c129c32964aca6a8509d98a0cc3c9bc46f2d8a4c/wandb/__init__.py)

## Classes

[`class Artifact`](artifact.md): Constructs an empty artifact whose contents can be populated using its

[`class Run`](run.md): The run object corresponds to a single execution of your script,

## Functions

[`agent(...)`](agent.md): Generic agent entrypoint, used for CLI or jupyter.

[`config(...)`](config.md): Config object

[`finish(...)`](finish.md): Marks a run as finished, and finishes uploading all data.

[`init(...)`](init.md): Start a new tracked run with `wandb.init()`.

[`log(...)`](log.md): Log a dict to the global run's history.

[`save(...)`](save.md): Ensure all files matching _glob\_str_ are synced to wandb with the policy specified.

[`setup(...)`](setup.md)

[`summary(...)`](summary.md): Summary tracks single values for each run. By default, summary is set to the

[`sweep(...)`](sweep.md)

| Other Members |  |
| :--- | :--- |
|  \_\_version\_\_ |  \`'0.10.27'\` |

