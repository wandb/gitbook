# API

Wandb provides an API to give users direct access to wandb's database. This can be useful for automated systems to asynchronously add extra data to a completed run or to export data for custom analysis. For complete documentation refer to our [API Reference](../reference/wandb_api.md).

### Authentication

Before using the API you need to store your key locally by running `wandb login` or set the **WANDB\_API\_KEY** environment variable.

### Querying Runs

The most common use of wandb's API is to export data from a past or running run.  

This can be used for ad-hoc data analysis or taking action in automated environments using custom logic.  

```python
import wandb
api = wandb.Api()
run = api.run("<entity>/<project>/<run_id>")
```

The most commonly used attributes of a run object are:

| Attribute | Meaning |
| :--- | :--- |
| run.config | A dictionary meant to store inputs to the model such as hyperparameters. |
| run.summary | A dictionary of outputs which can contain everything from scalars such as accuracy or loss to large files.  The command wandb.log\(\) updates this obejct.  It can also be set directly. |
| run.history | A list of dictionaries meant to store values that change while the model is training such as loss.  The command wandb.log\(\) appends to this object. |

You can also modify or update the data of past runs.  

By default a single instance of an api object will cache all network requests.  If your use case  requires real time information in a running script, call api.flush\(\) to get updated values.

### Querying Multiple Runs

The W&B API also provides a way for you to query across runs in a project with api.runs\(\). The most common use case is exporting runs data for custom analysis.  The query interface is the same as the one [MongoDB uses](https://docs.mongodb.com/manual/reference/operator/query).

```python
runs = api.runs("username/project", {"$or": [{"config.experiment_name": "foo"}, {"config.experiment_name": "bar"}]})
print("Found %i" % len(runs))
```

Calling `api.runs(...)` returns a **Runs** object that is iterable and acts like a list. The object loads 50 runs at a time in sequence as required, you can change the number loaded per page with the **per\_page** keyword argument.

`api.runs(...)` also accepts an **order** keyword argument. The default order is `-created_at`, specify `+created_at` to get results in ascending order. You can also sort by config or summary values i.e. `summary.val_acc` or `config.experiment_name`

### Error Handling

If errors occur while talking to W&B servers a `wandb.CommError` will be raised. The original exception can be introspected via the **exc** attribute.

