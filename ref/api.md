---
description: wandb.apis.public
---

# Public API

## wandb.apis.public

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1)

### Api Objects

```python
class Api(object)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L181)

Used for querying the wandb server.

**Examples**:

Most common way to initialize

```text
wandb.Api()
```

**Arguments**:

* `overrides` _dict_ - You can set `base_url` if you are using a wandb server

  other than [https://api.wandb.ai](https://api.wandb.ai).

  You can also set defaults for `entity`, `project`, and `run`.

**flush**

```python
 | flush()
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L276)

The api object keeps a local cache of runs, so if the state of the run may change while executing your script you must clear the local cache with `api.flush()` to get the latest values associated with the run.

**projects**

```python
 | projects(entity=None, per_page=200)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L338)

Get projects for a given entity.

**Arguments**:

* `entity` _str_ - Name of the entity requested.  If None will fallback to

  default entity passed to `Api`.  If no default entity, will raise a `ValueError`.

* `per_page` _int_ - Sets the page size for query pagination.  None will use the default size.

  Usually there is no reason to change this.

**Returns**:

A `Projects` object which is an iterable collection of `Project` objects.

**reports**

```python
 | reports(path="", name=None, per_page=50)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L360)

Get reports for a given project path.

WARNING: This api is in beta and will likely change in a future release

**Arguments**:

* `path` _str_ - path to project the report resides in, should be in the form: "entity/project"
* `name` _str_ - optional name of the report requested.
* `per_page` _int_ - Sets the page size for query pagination.  None will use the default size.

  Usually there is no reason to change this.

**Returns**:

A `Reports` object which is an iterable collection of `BetaReport` objects.

**runs**

```python
 | runs(path="", filters={}, order="-created_at", per_page=50)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L393)

Return a set of runs from a project that match the filters provided. You can filter by `config.*`, `summary.*`, `state`, `entity`, `createdAt`, etc.

**Examples**:

Find runs in my\_project config.experiment\_name has been set to "foo"

```text
api.runs(path="my_entity/my_project", {"config.experiment_name": "foo"})
```

Find runs in my\_project config.experiment\_name has been set to "foo" or "bar"

```text
api.runs(path="my_entity/my_project",
- `{"$or"` - [{"config.experiment_name": "foo"}, {"config.experiment_name": "bar"}]})
```

Find runs in my\_project sorted by ascending loss

```text
api.runs(path="my_entity/my_project", {"order": "+summary_metrics.loss"})
```

**Arguments**:

* `path` _str_ - path to project, should be in the form: "entity/project"
* `filters` _dict_ - queries for specific runs using the MongoDB query language.

  You can filter by run properties such as config.key, summary\_metrics.key, state, entity, createdAt, etc.

  For example: {"config.experiment\_name": "foo"} would find runs with a config entry

  of experiment name set to "foo"

  You can compose operations to make more complicated queries,

  see Reference for the language is at  [https://docs.mongodb.com/manual/reference/operator/query](https://docs.mongodb.com/manual/reference/operator/query)

* `order` _str_ - Order can be `created_at`, `heartbeat_at`, `config.*.value`, or `summary_metrics.*`.

  If you prepend order with a + order is ascending.

  If you prepend order with a - order is descending \(default\).

  The default order is run.created\_at from newest to oldest.

**Returns**:

A `Runs` object, which is an iterable collection of `Run` objects.

**run**

```python
 | @normalize_exceptions
 | run(path="")
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L445)

Returns a single run by parsing path in the form entity/project/run\_id.

**Arguments**:

* `path` _str_ - path to run in the form entity/project/run\_id.

  If api.entity is set, this can be in the form project/run\_id

  and if api.project is set this can just be the run\_id.

**Returns**:

A `Run` object.

**sweep**

```python
 | @normalize_exceptions
 | sweep(path="")
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L462)

Returns a sweep by parsing path in the form entity/project/sweep\_id.

**Arguments**:

* `path` _str, optional_ - path to sweep in the form entity/project/sweep\_id.  If api.entity

  is set, this can be in the form project/sweep\_id and if api.project is set

  this can just be the sweep\_id.

**Returns**:

A `Sweep` object.

**artifact**

```python
 | @normalize_exceptions
 | artifact(name, type=None)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L496)

Returns a single artifact by parsing path in the form entity/project/run\_id.

**Arguments**:

* `name` _str_ - An artifact name. May be prefixed with entity/project. Valid names

  can be in the following forms:

  name:version

  name:alias

  digest

* `type` _str, optional_ - The type of artifact to fetch.

**Returns**:

A `Artifact` object.

### Projects Objects

```python
class Projects(Paginator)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L616)

An iterable collection of `Project` objects.

### Project Objects

```python
class Project(Attrs)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L678)

A project is a namespace for runs

### Runs Objects

```python
class Runs(Paginator)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L699)

An iterable collection of runs associated with a project and optional filter. This is generally used indirectly via the `Api`.runs method

### Run Objects

```python
class Run(Attrs)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L804)

A single run associated with an entity and project.

**Attributes**:

* `tags` _\[str\]_ - a list of tags associated with the run
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

  with `wandb.log({key: value})`

**\_\_init\_\_**

```python
 | __init__(client, entity, project, run_id, attrs={})
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L829)

Run is always initialized by calling api.runs\(\) where api is an instance of wandb.Api

**create**

```python
 | @classmethod
 | create(cls, api, run_id=None, project=None, entity=None)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L887)

Create a run for the given project

**update**

```python
 | @normalize_exceptions
 | update()
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L993)

Persists changes to the run object to the wandb backend.

**files**

```python
 | @normalize_exceptions
 | files(names=[], per_page=50)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1070)

**Arguments**:

* `names` _list_ - names of the requested files, if empty returns all files
* `per_page` _int_ - number of results per page

**Returns**:

A `Files` object, which is an iterator over `File` obejcts.

**file**

```python
 | @normalize_exceptions
 | file(name)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1082)

**Arguments**:

* `name` _str_ - name of requested file.

**Returns**:

A `File` matching the name argument.

**upload\_file**

```python
 | @normalize_exceptions
 | upload_file(path, root=".")
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1093)

**Arguments**:

* `path` _str_ - name of file to upload.
* `root` _str_ - the root path to save the file relative to.  i.e.

  If you want to have the file saved in the run as "my\_dir/file.txt"

  and you're currently in "my\_dir" you would set root to "../"

**Returns**:

A `File` matching the name argument.

**history**

```python
 | @normalize_exceptions
 | history(samples=500, keys=None, x_axis="_step", pandas=True, stream="default")
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1116)

Returns sampled history metrics for a run. This is simpler and faster if you are ok with the history records being sampled.

**Arguments**:

* `samples` _int, optional_ - The number of samples to return
* `pandas` _bool, optional_ - Return a pandas dataframe
* `keys` _list, optional_ - Only return metrics for specific keys
* `x_axis` _str, optional_ - Use this metric as the xAxis defaults to \_step
* `stream` _str, optional_ - "default" for metrics, "system" for machine metrics

**Returns**:

If pandas=True returns a `pandas.DataFrame` of history metrics. If pandas=False returns a list of dicts of history metrics.

**scan\_history**

```python
 | @normalize_exceptions
 | scan_history(keys=None, page_size=1000, min_step=None, max_step=None)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1150)

Returns an iterable collection of all history records for a run.

**Example**:

Export all the loss values for an example run

```python
run = api.run("l2k2/examples-numpy-boston/i0wt6xua")
history = run.scan_history(keys=["Loss"])
losses = [row["Loss"] for row in history]
```

**Arguments**:

* `keys` _\[str\], optional_ - only fetch these keys, and only fetch rows that have all of keys defined.
* `page_size` _int, optional_ - size of pages to fetch from the api

**Returns**:

An iterable collection over history records \(dict\).

**use\_artifact**

```python
 | @normalize_exceptions
 | use_artifact(artifact)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1207)

Declare an artifact as an input to a run.

**Arguments**:

* `artifact` _`Artifact`_ - An artifact returned from

  `wandb.Api().artifact(name)`

**Returns**:

A `Artifact` object.

**log\_artifact**

```python
 | @normalize_exceptions
 | log_artifact(artifact, aliases=None)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1234)

Declare an artifact as output of a run.

**Arguments**:

* `artifact` _`Artifact`_ - An artifact returned from

  `wandb.Api().artifact(name)`

* `aliases` _list, optional_ - Aliases to apply to this artifact

**Returns**:

A `Artifact` object.

### Sweep Objects

```python
class Sweep(Attrs)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1314)

A set of runs associated with a sweep Instantiate with: api.sweep\(sweep\_path\)

**Attributes**:

* `runs` _`Runs`_ - list of runs
* `id` _str_ - sweep id
* `project` _str_ - name of project
* `config` _str_ - dictionary of sweep configuration

**best\_run**

```python
 | best_run(order=None)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1400)

Returns the best run sorted by the metric defined in config or the order passed in

**get**

```python
 | @classmethod
 | get(cls, client, entity=None, project=None, sid=None, withRuns=True, order=None, query=None, **kwargs)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1440)

Execute a query against the cloud backend

### Files Objects

```python
class Files(Paginator)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1495)

Files is an iterable collection of `File` objects.

### File Objects

```python
class File(object)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1561)

File is a class associated with a file saved by wandb.

**Attributes**:

* `name` _string_ - filename
* `url` _string_ - path to file
* `md5` _string_ - md5 of file
* `mimetype` _string_ - mimetype of file
* `updated_at` _string_ - timestamp of last update
* `size` _int_ - size of file in bytes

**download**

```python
 | @normalize_exceptions
 | @retriable(
 |         retry_timedelta=RETRY_TIMEDELTA,
 |         check_retry_fn=util.no_retry_auth,
 |         retryable_exceptions=(RetryError, requests.RequestException),
 |     )
 | download(root=".", replace=False)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1618)

Downloads a file previously saved by a run from the wandb server.

**Arguments**:

* `replace` _boolean_ - If `True`, download will overwrite a local file

  if it exists. Defaults to `False`.

* `root` _str_ - Local directory to save the file.  Defaults to ".".

**Raises**:

`ValueError` if file already exists and replace=False

### Reports Objects

```python
class Reports(Paginator)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1641)

Reports is an iterable collection of `BetaReport` objects.

### QueryGenerator Objects

```python
class QueryGenerator(object)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1721)

QueryGenerator is a helper object to write filters for runs

### BetaReport Objects

```python
class BetaReport(Attrs)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1818)

BetaReport is a class associated with reports created in wandb.

WARNING: this API will likely change in a future release

**Attributes**:

* `name` _string_ - report name
* `description` _string_ - report descirpiton;
* `user` _User_ - the user that created the report
* `spec` _dict_ - the spec off the report;
* `updated_at` _string_ - timestamp of last update

### ArtifactType Objects

```python
class ArtifactType(object)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L2282)

**collections**

```python
 | @normalize_exceptions
 | collections(per_page=50)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L2337)

Artifact collections

### ArtifactCollection Objects

```python
class ArtifactCollection(object)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L2352)

**versions**

```python
 | @normalize_exceptions
 | versions(per_page=50)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L2366)

Artifact versions

### Artifact Objects

```python
class Artifact(object)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L2381)

**delete**

```python
 | delete()
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L2534)

Delete artifact and it's files.

**get**

```python
 | get(name)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L2628)

Returns the wandb.Media resource stored in the artifact. Media can be stored in the artifact via Artifact\#add\(obj: wandbMedia, name: str\)\`

**Arguments**:

* `name` _str_ - name of resource.

**Returns**:

A `wandb.Media` which has been stored at `name`

**download**

```python
 | download(root=None)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L2663)

Download the artifact to dir specified by the 

**Arguments**:

* `root` _str, optional_ - directory to download artifact to. If None

  artifact will be downloaded to './artifacts//'

**Returns**:

The path to the downloaded contents.

**file**

```python
 | file(root=None)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L2702)

Download a single file artifact to dir specified by the 

**Arguments**:

* `root` _str, optional_ - directory to download artifact to. If None

  artifact will be downloaded to './artifacts//'

**Returns**:

The full path of the downloaded file

**save**

```python
 | @normalize_exceptions
 | save()
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L2737)

Persists artifact changes to the wandb backend.

**verify**

```python
 | verify(root=None)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L2776)

Verify an artifact by checksumming its downloaded contents.

Raises a ValueError if the verification fails. Does not verify downloaded reference files.

**Arguments**:

* `root` _str, optional_ - directory to download artifact to. If None

  artifact will be downloaded to './artifacts//'

### ArtifactVersions Objects

```python
class ArtifactVersions(Paginator)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L2930)

An iterable collection of artifact versions associated with a project and optional filter. This is generally used indirectly via the `Api`.artifact\_versions method

