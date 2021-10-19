# wandb.apis.public.Api

[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/v0.12.2/wandb/apis/public.py#L191-L566)

Used for querying the wandb server.

```python
Api(
    overrides={},
    timeout: Optional[int] = None
)
```

#### Examples:

Most common way to initialize

```text
>>> wandb.Api()
```

| Arguments |  |
| :--- | :--- |
| `overrides` | \(dict\) You can set `base_url` if you are using a wandb server other than [https://api.wandb.ai](https://api.wandb.ai). You can also set defaults for `entity`, `project`, and `run`. |

| Attributes |  |
| :--- | :--- |


## Methods

### `artifact` <a id="artifact"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.2/wandb/apis/public.py#L545-L566)

```python
artifact(
    name, type=None
)
```

Returns a single artifact by parsing path in the form `entity/project/run_id`.

| Arguments |  |
| :--- | :--- |
| `name` | \(str\) An artifact name. May be prefixed with entity/project. Valid names can be in the following forms: name:version name:alias digest |
| `type` | \(str, optional\) The type of artifact to fetch. |

| Returns |  |
| :--- | :--- |
| A `Artifact` object. |  |

### `artifact_type` <a id="artifact_type"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.2/wandb/apis/public.py#L534-L537)

```python
artifact_type(
    type_name, project=None
)
```

### `artifact_types` <a id="artifact_types"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.2/wandb/apis/public.py#L529-L532)

```python
artifact_types(
    project=None
)
```

### `artifact_versions` <a id="artifact_versions"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.2/wandb/apis/public.py#L539-L543)

```python
artifact_versions(
    type_name, name, per_page=50
)
```

### `create_run` <a id="create_run"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.2/wandb/apis/public.py#L254-L258)

```python
create_run(
    **kwargs
)
```

Create a new run

### `flush` <a id="flush"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.2/wandb/apis/public.py#L309-L315)

```python
flush()
```

The api object keeps a local cache of runs, so if the state of the run may change while executing your script you must clear the local cache with `api.flush()` to get the latest values associated with the run.

### `projects` <a id="projects"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.2/wandb/apis/public.py#L372-L394)

```python
projects(
    entity=None, per_page=200
)
```

Get projects for a given entity.

| Arguments |  |
| :--- | :--- |
| `entity` | \(str\) Name of the entity requested. If None will fallback to default entity passed to `Api`. If no default entity, will raise a `ValueError`. |
| `per_page` | \(int\) Sets the page size for query pagination. None will use the default size. Usually there is no reason to change this. |

| Returns |  |
| :--- | :--- |
| A `Projects` object which is an iterable collection of `Project` objects. |  |

### `queued_job` <a id="queued_job"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.2/wandb/apis/public.py#L504-L509)

```python
queued_job(
    path=""
)
```

Returns a single queued run by parsing the path in the form entity/project/queue\_id/run\_queue\_item\_id

### `reports` <a id="reports"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.2/wandb/apis/public.py#L396-L425)

```python
reports(
    path="", name=None, per_page=50
)
```

Get reports for a given project path.

WARNING: This api is in beta and will likely change in a future release

| Arguments |  |
| :--- | :--- |
| `path` | \(str\) path to project the report resides in, should be in the form: "entity/project" |
| `name` | \(str\) optional name of the report requested. |
| `per_page` | \(int\) Sets the page size for query pagination. None will use the default size. Usually there is no reason to change this. |

| Returns |  |
| :--- | :--- |
| A `Reports` object which is an iterable collection of `BetaReport` objects. |  |

### `run` <a id="run"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.2/wandb/apis/public.py#L486-L502)

```python
run(
    path=""
)
```

Returns a single run by parsing path in the form entity/project/run\_id.

| Arguments |  |
| :--- | :--- |
| `path` | \(str\) path to run in the form `entity/project/run_id`. If api.entity is set, this can be in the form `project/run_id` and if `api.project` is set this can just be the run\_id. |

| Returns |  |
| :--- | :--- |
| A `Run` object. |  |

### `runs` <a id="runs"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.2/wandb/apis/public.py#L427-L484)

```python
runs(
    path="", filters=None, order="-created_at", per_page=50
)
```

Return a set of runs from a project that match the filters provided.

You can filter by `config.*`, `summary.*`, `state`, `entity`, `createdAt`, etc.

#### Examples:

Find runs in my\_project where config.experiment\_name has been set to "foo"

```text
api.runs(path="my_entity/my_project", filters={"config.experiment_name": "foo"})
```

Find runs in my\_project where config.experiment\_name has been set to "foo" or "bar"

```text
api.runs(path="my_entity/my_project",
    filters={"$or": [{"config.experiment_name": "foo"}, {"config.experiment_name": "bar"}]})
```

Find runs in my\_project where config.experiment\_name matches a regex \(anchors are not supported\)

```text
api.runs(path="my_entity/my_project",
    filters={"config.experiment_name": {"$regex": "b.*"}})
```

Find runs in my\_project sorted by ascending loss

```text
api.runs(path="my_entity/my_project", order="+summary_metrics.loss")
```

| Arguments |  |
| :--- | :--- |
| `path` | \(str\) path to project, should be in the form: "entity/project" |
| `filters` | \(dict\) queries for specific runs using the MongoDB query language. You can filter by run properties such as config.key, summary\_metrics.key, state, entity, createdAt, etc. For example: {"config.experiment\_name": "foo"} would find runs with a config entry of experiment name set to "foo" You can compose operations to make more complicated queries, see Reference for the language is at [https://docs.mongodb.com/manual/reference/operator/query](https://docs.mongodb.com/manual/reference/operator/query) |
| `order` | \(str\) Order can be `created_at`, `heartbeat_at`, `config.*.value`, or `summary_metrics.*`. If you prepend order with a + order is ascending. If you prepend order with a - order is descending \(default\). The default order is run.created\_at from newest to oldest. |

| Returns |  |
| :--- | :--- |
| A `Runs` object, which is an iterable collection of `Run` objects. |  |

### `sweep` <a id="sweep"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.2/wandb/apis/public.py#L511-L527)

```python
sweep(
    path=""
)
```

Returns a sweep by parsing path in the form `entity/project/sweep_id`.

| Arguments |  |
| :--- | :--- |
| `path` | \(str, optional\) path to sweep in the form entity/project/sweep\_id. If api.entity is set, this can be in the form project/sweep\_id and if `api.project` is set this can just be the sweep\_id. |

| Returns |  |
| :--- | :--- |
| A `Sweep` object. |  |

### `sync_tensorboard` <a id="sync_tensorboard"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.2/wandb/apis/public.py#L260-L281)

```python
sync_tensorboard(
    root_dir, run_id=None, project=None, entity=None
)
```

Sync a local directory containing tfevent files to wandb

| Class Variables |  |
| :--- | :--- |
| `VIEWER_QUERY` |  |

