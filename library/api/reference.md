# index

## Api

```python
Api(self, overrides={})
```

W&B Public API Used for querying the wandb server. Initialize with wandb.Api\(\)

**Arguments**:

* `overrides` _dict_ - You can set defaults such as

  entity, project, and run here as well as which api server to use.

### flush

```python
Api.flush(self)
```

Api keeps a local cache of runs, so if the state of the run may change while executing your script you must clear the local cache with api.flush\(\) to get the latest values associated with the run.

### projects

```python
Api.projects(self, entity=None, per_page=None)
```

Return a list of projects for a given entity.

### runs

```python
Api.runs(self, path='', filters={}, order='-created_at', per_page=None)
```

Return a set of runs from a project that match the filters provided. You can filter by config._, summary._, state, entity, createdAt, etc.

**Arguments**:

* `path` _str_ - path to project, should be in the form: "entity/project"
* `filters` _dict_ - queries for specific runs using the MongoDB query language.

  Reference for the language is at  [https://docs.mongodb.com/manual/reference/operator/query](https://docs.mongodb.com/manual/reference/operator/query)

  You can filter by run properties such as config, summary, state, entity, createdAt

  For example: {"config.experiment\_name": "foo"} would find runs with a config entry

  of experiment name set to "foo"

  You can compose operations to make more complicated queries like:

* `{"$or"` - \[{"config.experiment\_name": "foo"}, {"config.experiment\_name": "bar"}
* `order` _str_ - Order can be created\_at, heartbeat\_at, config._.value, or summary._.

  If you prepend order with a + order is ascending.

  If you prepend order with a - order is descending \(default\).

  The dafault order is run.created\_at from newest to oldest.

**Returns**:

A Runs object, which is an iterable set of Run objects.

### run

```python
Api.run(self, path='')
```

Returns a single run by parsing path in the form entity/project/run\_id.

**Arguments**:

* `path` _str_ - path to run in the form entity/project/run\_id.  If api.entity

  is set, this can be in the form project/run\_id and if api.project is set

  this can just be the run\_id.

**Returns**:

A `Run` object.

### sweep

```python
Api.sweep(self, path='')
```

Returns a sweep by parsing path in the form entity/project/sweep\_id.

**Arguments**:

* `path` _str_ - path to sweep in the form entity/project/sweep\_id.  If api.entity

  is set, this can be in the form project/sweep\_id and if api.project is set

  this can just be the sweep\_id.

**Returns**:

A `Sweep` object.

## Projects

```python
Projects(self, client, entity, per_page=50)
```

An iterable set of projects

## Project

```python
Project(self, entity, project, attrs)
```

A project is a namespace for runs

## Runs

```python
Runs(self, client, entity, project, filters={}, order=None, per_page=50)
```

An iterable set of runs associated with a project and optional filter.

## Run

```python
Run(self, client, entity, project, run_id, attrs={})
```

A single run associated with an entity and project

**Attributes**:

tags \(list\(str\)\): a list of tags associated with the run

* `url` _str_ - the url of this run
* `id` _str_ - unique identifier for the run \(defaults to eight characters\)
* `name` _str_ - the name of the run
* `state` _str_ - one of: running, finished, crashed, aborted
* `config` _dict_ - a dict of hyperparameters associated with the run
* `created_at` _str_ - ISO timestamp when the run was started
* `system_metrics` _dict_ - the latest system metrics recorded for the run
* `summary` _dict_ - A mutable dict-like property that holds the current summary.

  Calling update will persist any changes.

* `project` _str_ - the project associated with the run
* `entity` _str_ - the name of the entity associated with the run
* `user` _str_ - the name of the user who created the run
* `path` _str_ - Unique identifier \[entity\]/\[project\]/\[run\_id\]
* `notes` _str_ - Notes about the run
* `read_only` _boolean_ - Whether the run is editable
* `history_keys` _str_ - Keys of the history metrics that have been logged

  with wandb.log\({key: value}\)

### storage\_id

For compatibility with wandb.Run, which has storage IDs in self.storage\_id and names in self.id.

### create

```python
Run.create(api, run_id=None, project=None, entity=None)
```

Create a run for the given project

### update

```python
Run.update(self)
```

Persists changes to the run object to the wandb backend.

### files

```python
Run.files(self, names=[], per_page=50)
```

**Arguments**:

* `names` _list_ - names of the requested files, if empty returns all files
* `per_page` - \(integer\): number of results per page

**Returns**:

Files object

### file

```python
Run.file(self, name)
```

**Arguments**:

* `name` _str_ - name of requested file.

  Returns File

### history

```python
Run.history(self, samples=500, keys=None, x_axis='_step', pandas=True, stream='default')
```

Return history metrics for a run

**Arguments**:

* `samples` _int, optional_ - The number of samples to return
* `pandas` _bool, optional_ - Return a pandas dataframe
* `keys` _list, optional_ - Only return metrics for specific keys
* `x_axis` _str, optional_ - Use this metric as the xAxis defaults to \_step
* `stream` _str, optional_ - "default" for metrics, "system" for machine metrics

### scan\_history

```python
Run.scan_history(self, keys=None, page_size=1000)
```

Returns an iterable that returns all history for a run unsampled

**Arguments**:

* `keys` _\[str\], optional_ - only fetch these keys, and rows that have all of them
* `page_size` _int, optional_ - size of pages to fetch from the api

## Sweep

```python
Sweep(self, client, entity, project, sweep_id, attrs={})
```

A set of runs associated with a sweep Instantiate with: api.sweep\(sweep\_path\)

**Attributes**:

* `runs` _Runs_ - list of runs
* `id` _str_ - sweep id
* `project` _str_ - name of project
* `config` _str_ - dictionary of sweep configuration

## Files

```python
Files(self, client, run, names=[], per_page=50, upload=False)
```

Files is a paginated list of files.

## File

```python
File(self, client, attrs)
```

File is a file saved by wandb.

