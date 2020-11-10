# MLflow \(beta\)

> The MLFlow integration is currently in beta and is not a part of the official wandb python package. To try this integration you can install wandb from our git branch by running:

```bash
pip install --upgrade git+git://github.com/wandb/client.git@feature/mlflow#egg=wandb
```

## MLflow Integration

If you're already using [MLflow](https://www.mlflow.org/docs/latest/tracking.html) to track your experiments it's easy to visualize them with W&B. Simply by calling `import wandb` in your mlflow scripts we'll mirror all metrics, params, and artifacts to W&B. We do this by patching the mlflow [python library](https://github.com/mlflow/mlflow). Our current integration is write only. All data will also be written to the [backend](https://www.mlflow.org/docs/latest/tracking.html#where-runs-are-recorded) you've configured for mlflow.

## Concept mappings

When mirroring data to both a wandb and mlflow tracking backend, the following concepts are mapped to each-other.

| MLflow | W&B |
| :--- | :--- |
| [Experiment](https://www.mlflow.org/docs/latest/tracking.html#organizing-runs-in-experiments) | [Project](../../app/pages/project-page.md) |
| [mlflow.start\_run](https://www.mlflow.org/docs/latest/python_api/mlflow.html#mlflow.start_run) | [wandb.init](../init.md) |
| [mlflow.log\_params](https://www.mlflow.org/docs/latest/python_api/mlflow.html#mlflow.log_param) | [wandb.config](../config.md) |
| [mlflow.log\_metrics](https://www.mlflow.org/docs/latest/python_api/mlflow.html#mlflow.log_metric) | [wandb.log](../log.md) |
| [mlflow.log\_artifacts](https://www.mlflow.org/docs/latest/python_api/mlflow.html#mlflow.log_artifact) | [wandb.save](../save.md) |
| [mlflow.start\_run\(nested=True\)](https://mlflow.org/docs/latest/python_api/mlflow.html#mlflow.start_run) | [Grouping](../grouping.md) |

## Logging rich metrics

If you want to log rich media like Images, Video, or Plots you can call [wandb.log](../log.md) in your code as well. Be sure to pass a step argument to your calls to log so they can be aligned with the metrics you're logging with mlflow.

## Advanced configuration

By default wandb only logs metrics, params and artifacts. If you don't want to store artifacts with wandb, you can set `WANDB_SYNC_MLFLOW=metrics,params` . If you want to disable mirroring of all data to wandb you can set the `WANDB_SYNC_MLFLOW=false`.

