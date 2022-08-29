# Explore and traverse artifact graphs

Weights & Biases automatically tracks the artifacts a given run logged as well as the artifacts a given run used. To view an artifact graph:

1. Navigate to your project in the W\&B App UI
2. Choose the artifact icon on the left panel.
3. Select **Lineage**.

The `type` you provide when you create runs and artifacts are used to create the graph. Runs are depicted as squares and artifacts are depicted as circles. The input and output of a run or artifact is depicted in the graph with arrows. The default graph view shows a general overview of your pipeline.&#x20;

![](<../../.gitbook/assets/2021-02-08 08.40.34.gif>)

For a more detailed view, select the Explode toggle on the upper left hand side of the dashboard. The expanded graph shows details of every run and every artifact in the project that was logged. Above the circle or square is a key-value pair. The key is the type, and the value is either the run name (runs) or the version (artifacts).

&#x20;Try it yourself on this [example Graph page](https://wandb.ai/shawn/detectron2-11/artifacts/dataset/furniture-small-val/06d5ddd4deeb2a6ebdd5/graph).

### Traverse a graph

Programmatically walk a graph with the W\&B Public API (`wandb.Api`). Traverse a graph from an artifact or from a run.&#x20;

#### Traverse from an artifact

Create an artifact object with the W\&B Public API ([wandb.Api](https://docs.wandb.ai/ref/python/public-api/api)). Provide the name of the project, artifact and alias of the artifact:

```python
import wandb

api = wandb.Api()

artifact = api.artifact('project/artifact:alias')
```

Use the artifact objects [`logged_by`](https://docs.wandb.ai/ref/python/public-api/artifact#logged\_by) and [`used_by`](https://docs.wandb.ai/ref/python/public-api/artifact#used\_by) methods to walk the graph from the artifact:&#x20;

```python
# Walk up and down the graph from an artifact:
producer_run = artifact.logged_by()
consumer_runs = artifact.used_by()
```

#### Traverse from a run&#x20;

Create an artifact object with the W\&B Public API ([wandb.Api.Run](https://docs.wandb.ai/ref/python/public-api/run)). Provide the name of the entity, project, and run ID:

```python
import wandb

api = wandb.Api()

artifact = api.run('entity/project/run_id')
```

Use the [`logged_artifacts`](https://docs.wandb.ai/ref/python/public-api/run#logged\_artifacts) and [`used_artifacts`](https://docs.wandb.ai/ref/python/public-api/run#used\_artifacts) methods to walk the graph from a given run:

```python
# Walk up and down the graph from a run:
logged_artifacts = run.logged_artifacts()
used_artifacts = run.used_artifacts()
```
