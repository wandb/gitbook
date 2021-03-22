# Import/Export API

[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/master/wandb/__init__.py)

Use the Public API to export or update data that you have saved to W&B.

Before using this API, you'll want to log data from your script — check the [Quickstart](https://github.com/ariG23498/Aritra-Documentation/tree/2e5d9ed059a09db9833a6e62b80135edccf67d05/library/quickstart.md) for more details.

**Use Cases for the Public API**

* **Export Data**: Pull down a dataframe for custom analysis in a Jupyter Notebook. Once you have explored the data, you can sync your findings by creating a new analysis run and logging results, for example: `wandb.init(job_type="analysis")`
* **Update Existing Runs**: You can update the data logged in association with a W&B run. For example, you might want to update the config of a set of runs to include additional information, like the architecture or a hyperparameter that wasn't originally logged.

## Classes

[`class Api`](api.md): Used for querying the wandb server.

[`class Artifact`](artifact.md)

[`class File`](file.md): File is a class associated with a file saved by wandb.

[`class Files`](files.md): Files is an iterable collection of `File` objects.

[`class Project`](project.md): A project is a namespace for runs

[`class Projects`](projects.md): An iterable collection of `Project` objects.

[`class Run`](run.md): A single run associated with an entity and project.

[`class Runs`](runs.md): An iterable collection of runs associated with a project and optional filter.

[`class Sweep`](sweep.md): A set of runs associated with a sweep

