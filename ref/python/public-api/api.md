# Api



[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/v0.12.1/wandb/apis/public.py#L191-L568)



Used for querying the wandb server.

```python
Api(
    overrides={},
    timeout: Optional[int] = None
)
```





#### Examples:

Most common way to initialize
```
>>> wandb.Api()
```



| Arguments |  |
| :--- | :--- |
|  `overrides` |  (dict) You can set `base_url` if you are using a wandb server other than https://api.wandb.ai. You can also set defaults for `entity`, `project`, and `run`. |





| Attributes |  |
| :--- | :--- |



## Methods

<h3 id="artifact"><code>artifact</code></h3>

[View source](https://www.github.com/wandb/client/tree/v0.12.1/wandb/apis/public.py#L547-L568)

```python
artifact(
    name, type=None
)
```

Returns a single artifact by parsing path in the form `entity/project/run_id`.


| Arguments |  |
| :--- | :--- |
|  `name` |  (str) An artifact name. May be prefixed with entity/project. Valid names can be in the following forms: name:version name:alias digest |
|  `type` |  (str, optional) The type of artifact to fetch. |



| Returns |  |
| :--- | :--- |
|  A `Artifact` object. |



<h3 id="artifact_type"><code>artifact_type</code></h3>

[View source](https://www.github.com/wandb/client/tree/v0.12.1/wandb/apis/public.py#L536-L539)

```python
artifact_type(
    type_name, project=None
)
```




<h3 id="artifact_types"><code>artifact_types</code></h3>

[View source](https://www.github.com/wandb/client/tree/v0.12.1/wandb/apis/public.py#L531-L534)

```python
artifact_types(
    project=None
)
```




<h3 id="artifact_versions"><code>artifact_versions</code></h3>

[View source](https://www.github.com/wandb/client/tree/v0.12.1/wandb/apis/public.py#L541-L545)

```python
artifact_versions(
    type_name, name, per_page=50
)
```




<h3 id="create_run"><code>create_run</code></h3>

[View source](https://www.github.com/wandb/client/tree/v0.12.1/wandb/apis/public.py#L254-L258)

```python
create_run(
    **kwargs
)
```

Create a new run


<h3 id="flush"><code>flush</code></h3>

[View source](https://www.github.com/wandb/client/tree/v0.12.1/wandb/apis/public.py#L309-L315)

```python
flush()
```

The api object keeps a local cache of runs, so if the state of the run may
change while executing your script you must clear the local cache with `api.flush()`
to get the latest values associated with the run.

<h3 id="projects"><code>projects</code></h3>

[View source](https://www.github.com/wandb/client/tree/v0.12.1/wandb/apis/public.py#L372-L394)

```python
projects(
    entity=None, per_page=200
)
```

Get projects for a given entity.


| Arguments |  |
| :--- | :--- |
|  `entity` |  (str) Name of the entity requested. If None will fallback to default entity passed to `Api`. If no default entity, will raise a `ValueError`. |
|  `per_page` |  (int) Sets the page size for query pagination. None will use the default size. Usually there is no reason to change this. |



| Returns |  |
| :--- | :--- |
|  A `Projects` object which is an iterable collection of `Project` objects. |



<h3 id="queued_job"><code>queued_job</code></h3>

[View source](https://www.github.com/wandb/client/tree/v0.12.1/wandb/apis/public.py#L506-L511)

```python
queued_job(
    path=""
)
```

Returns a single queued run by parsing the path in the form entity/project/queue_id/run_queue_item_id


<h3 id="reports"><code>reports</code></h3>

[View source](https://www.github.com/wandb/client/tree/v0.12.1/wandb/apis/public.py#L396-L427)

```python
reports(
    path="", name=None, per_page=50
)
```

Get reports for a given project path.

WARNING: This api is in beta and will likely change in a future release

| Arguments |  |
| :--- | :--- |
|  `path` |  (str) path to project the report resides in, should be in the form: "entity/project" |
|  `name` |  (str) optional name of the report requested. |
|  `per_page` |  (int) Sets the page size for query pagination. None will use the default size. Usually there is no reason to change this. |



| Returns |  |
| :--- | :--- |
|  A `Reports` object which is an iterable collection of `BetaReport` objects. |



<h3 id="run"><code>run</code></h3>

[View source](https://www.github.com/wandb/client/tree/v0.12.1/wandb/apis/public.py#L488-L504)

```python
run(
    path=""
)
```

Returns a single run by parsing path in the form entity/project/run_id.


| Arguments |  |
| :--- | :--- |
|  `path` |  (str) path to run in the form `entity/project/run_id`. If api.entity is set, this can be in the form `project/run_id` and if `api.project` is set this can just be the run_id. |



| Returns |  |
| :--- | :--- |
|  A `Run` object. |



<h3 id="runs"><code>runs</code></h3>

[View source](https://www.github.com/wandb/client/tree/v0.12.1/wandb/apis/public.py#L429-L486)

```python
runs(
    path="", filters=None, order="-created_at", per_page=50
)
```

Return a set of runs from a project that match the filters provided.

You can filter by `config.*`, `summary.*`, `state`, `entity`, `createdAt`, etc.

#### Examples:

Find runs in my_project where config.experiment_name has been set to "foo"
```
api.runs(path="my_entity/my_project", filters={"config.experiment_name": "foo"})
```

Find runs in my_project where config.experiment_name has been set to "foo" or "bar"
```
api.runs(path="my_entity/my_project",
    filters={"$or": [{"config.experiment_name": "foo"}, {"config.experiment_name": "bar"}]})
```

Find runs in my_project where config.experiment_name matches a regex (anchors are not supported)
```
api.runs(path="my_entity/my_project",
    filters={"config.experiment_name": {"$regex": "b.*"}})
```

Find runs in my_project sorted by ascending loss
```
api.runs(path="my_entity/my_project", order="+summary_metrics.loss")
```



| Arguments |  |
| :--- | :--- |
|  `path` |  (str) path to project, should be in the form: "entity/project" |
|  `filters` |  (dict) queries for specific runs using the MongoDB query language. You can filter by run properties such as config.key, summary_metrics.key, state, entity, createdAt, etc. For example: {"config.experiment_name": "foo"} would find runs with a config entry of experiment name set to "foo" You can compose operations to make more complicated queries, see Reference for the language is at https://docs.mongodb.com/manual/reference/operator/query |
|  `order` |  (str) Order can be `created_at`, `heartbeat_at`, `config.*.value`, or `summary_metrics.*`. If you prepend order with a + order is ascending. If you prepend order with a - order is descending (default). The default order is run.created_at from newest to oldest. |



| Returns |  |
| :--- | :--- |
|  A `Runs` object, which is an iterable collection of `Run` objects. |



<h3 id="sweep"><code>sweep</code></h3>

[View source](https://www.github.com/wandb/client/tree/v0.12.1/wandb/apis/public.py#L513-L529)

```python
sweep(
    path=""
)
```

Returns a sweep by parsing path in the form `entity/project/sweep_id`.


| Arguments |  |
| :--- | :--- |
|  `path` |  (str, optional) path to sweep in the form entity/project/sweep_id. If api.entity is set, this can be in the form project/sweep_id and if `api.project` is set this can just be the sweep_id. |



| Returns |  |
| :--- | :--- |
|  A `Sweep` object. |



<h3 id="sync_tensorboard"><code>sync_tensorboard</code></h3>

[View source](https://www.github.com/wandb/client/tree/v0.12.1/wandb/apis/public.py#L260-L281)

```python
sync_tensorboard(
    root_dir, run_id=None, project=None, entity=None
)
```

Sync a local directory containing tfevent files to wandb






| Class Variables |  |
| :--- | :--- |
|  `VIEWER_QUERY`<a id="VIEWER_QUERY"></a> |   |

