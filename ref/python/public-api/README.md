# public-api

<!-- Insert buttons and diff -->


[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/v0.12.11/wandb/__init__.py)



Use the Public API to export or update data that you have saved to W&B.


Before using this API, you'll want to log data from your script — check the
[Quickstart](https://docs.wandb.ai/quickstart) for more details.

You might use the Public API to
 - update metadata or metrics for an experiment after it has been completed,
 - pull down your results as a dataframe for post-hoc analysis in a Jupyter notebook, or
 - check your saved model artifacts for those tagged as `ready-to-deploy`.

For more on using the Public API, check out [our guide](https://docs.wandb.com/guides/track/public-api-guide).

## Classes

[`class Api`](./api.md): Used for querying the wandb server.

[`class Artifact`](./artifact.md): A wandb Artifact.

[`class File`](./file.md): File is a class associated with a file saved by wandb.

[`class Files`](./files.md): An iterable collection of `File` objects.

[`class Project`](./project.md): A project is a namespace for runs.

[`class Projects`](./projects.md): An iterable collection of `Project` objects.

[`class Run`](./run.md): A single run associated with an entity and project.

[`class Runs`](./runs.md): An iterable collection of runs associated with a project and optional filter.

[`class Sweep`](./sweep.md): A set of runs associated with a sweep.

