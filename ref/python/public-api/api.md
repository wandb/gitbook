# Api



[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/v0.10.28/wandb/apis/public.py#L185-L554)




Used for querying the wandb server.

<pre><code>Api(
    overrides={}
)</code></pre>





#### Examples:

Most common way to initialize
```
>>> wandb.Api()
```



<!-- Tabular view -->
<table>
<tr><th>Arguments</th></tr>

<tr>
<td>
<code>overrides</code>
</td>
<td>
(dict) You can set <code>base_url</code> if you are using a wandb server
other than https://api.wandb.ai.
You can also set defaults for <code>entity</code>, <code>project</code>, and <code>run</code>.
</td>
</tr>
</table>





<!-- Tabular view -->
<table>
<tr><th>Attributes</th></tr>


</table>



## Methods

<h3 id="artifact"><code>artifact</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.28/wandb/apis/public.py#L533-L554">View source</a>

<pre><code>artifact(
    name, type=None
)</code></pre>

Returns a single artifact by parsing path in the form `entity/project/run_id`.


<!-- Tabular view -->
<table>
<tr><th>Arguments</th></tr>

<tr>
<td>
<code>name</code>
</td>
<td>
(str) An artifact name. May be prefixed with entity/project. Valid names
can be in the following forms:
name:version
name:alias
digest
</td>
</tr><tr>
<td>
<code>type</code>
</td>
<td>
(str, optional) The type of artifact to fetch.
</td>
</tr>
</table>



<!-- Tabular view -->
<table>
<tr><th>Returns</th></tr>
<tr>
<td>
A <code>Artifact</code> object.
</td>
</tr>

</table>



<h3 id="artifact_type"><code>artifact_type</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.28/wandb/apis/public.py#L522-L525">View source</a>

<pre><code>artifact_type(
    type_name, project=None
)</code></pre>




<h3 id="artifact_types"><code>artifact_types</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.28/wandb/apis/public.py#L517-L520">View source</a>

<pre><code>artifact_types(
    project=None
)</code></pre>




<h3 id="artifact_versions"><code>artifact_versions</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.28/wandb/apis/public.py#L527-L531">View source</a>

<pre><code>artifact_versions(
    type_name, name, per_page=50
)</code></pre>




<h3 id="create_run"><code>create_run</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.28/wandb/apis/public.py#L247-L251">View source</a>

<pre><code>create_run(
    **kwargs
)</code></pre>

Create a new run


<h3 id="flush"><code>flush</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.28/wandb/apis/public.py#L302-L308">View source</a>

<pre><code>flush()</code></pre>

The api object keeps a local cache of runs, so if the state of the run may
change while executing your script you must clear the local cache with <code>api.flush()</code>
to get the latest values associated with the run.

<h3 id="projects"><code>projects</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.28/wandb/apis/public.py#L365-L387">View source</a>

<pre><code>projects(
    entity=None, per_page=200
)</code></pre>

Get projects for a given entity.


<!-- Tabular view -->
<table>
<tr><th>Arguments</th></tr>

<tr>
<td>
<code>entity</code>
</td>
<td>
(str) Name of the entity requested.  If None will fallback to
default entity passed to <code>Api</code>.  If no default entity, will raise a <code>ValueError</code>.
</td>
</tr><tr>
<td>
<code>per_page</code>
</td>
<td>
(int) Sets the page size for query pagination.  None will use the default size.
Usually there is no reason to change this.
</td>
</tr>
</table>



<!-- Tabular view -->
<table>
<tr><th>Returns</th></tr>
<tr>
<td>
A <code>Projects</code> object which is an iterable collection of <code>Project</code> objects.
</td>
</tr>

</table>



<h3 id="reports"><code>reports</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.28/wandb/apis/public.py#L389-L420">View source</a>

<pre><code>reports(
    path=&#x27;&#x27;, name=None, per_page=50
)</code></pre>

Get reports for a given project path.

WARNING: This api is in beta and will likely change in a future release

<!-- Tabular view -->
<table>
<tr><th>Arguments</th></tr>

<tr>
<td>
<code>path</code>
</td>
<td>
(str) path to project the report resides in, should be in the form: "entity/project"
</td>
</tr><tr>
<td>
<code>name</code>
</td>
<td>
(str) optional name of the report requested.
</td>
</tr><tr>
<td>
<code>per_page</code>
</td>
<td>
(int) Sets the page size for query pagination.  None will use the default size.
Usually there is no reason to change this.
</td>
</tr>
</table>



<!-- Tabular view -->
<table>
<tr><th>Returns</th></tr>
<tr>
<td>
A <code>Reports</code> object which is an iterable collection of <code>BetaReport</code> objects.
</td>
</tr>

</table>



<h3 id="run"><code>run</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.28/wandb/apis/public.py#L481-L497">View source</a>

<pre><code>run(
    path=&#x27;&#x27;
)</code></pre>

Returns a single run by parsing path in the form entity/project/run_id.


<!-- Tabular view -->
<table>
<tr><th>Arguments</th></tr>

<tr>
<td>
<code>path</code>
</td>
<td>
(str) path to run in the form `entity/project/run_id`.
If api.entity is set, this can be in the form `project/run_id`
and if <code>api.project</code> is set this can just be the run_id.
</td>
</tr>
</table>



<!-- Tabular view -->
<table>
<tr><th>Returns</th></tr>
<tr>
<td>
A <code>Run</code> object.
</td>
</tr>

</table>



<h3 id="runs"><code>runs</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.28/wandb/apis/public.py#L422-L479">View source</a>

<pre><code>runs(
    path=&#x27;&#x27;, filters=None, order=&#x27;-created_at&#x27;, per_page=50
)</code></pre>

Return a set of runs from a project that match the filters provided.

You can filter by `config.*<code>, </code>summary.*<code>, </code>state<code>, </code>entity<code>, </code>createdAt`, etc.

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



<!-- Tabular view -->
<table>
<tr><th>Arguments</th></tr>

<tr>
<td>
<code>path</code>
</td>
<td>
(str) path to project, should be in the form: "entity/project"
</td>
</tr><tr>
<td>
<code>filters</code>
</td>
<td>
(dict) queries for specific runs using the MongoDB query language.
You can filter by run properties such as config.key, summary_metrics.key, state, entity, createdAt, etc.
For example: {"config.experiment_name": "foo"} would find runs with a config entry
of experiment name set to "foo"
You can compose operations to make more complicated queries,
see Reference for the language is at  https://docs.mongodb.com/manual/reference/operator/query
</td>
</tr><tr>
<td>
<code>order</code>
</td>
<td>
(str) Order can be <code>created_at</code>, <code>heartbeat_at</code>, `config.*.value<code>, or </code>summary_metrics.*`.
If you prepend order with a + order is ascending.
If you prepend order with a - order is descending (default).
The default order is run.created_at from newest to oldest.
</td>
</tr>
</table>



<!-- Tabular view -->
<table>
<tr><th>Returns</th></tr>
<tr>
<td>
A <code>Runs</code> object, which is an iterable collection of <code>Run</code> objects.
</td>
</tr>

</table>



<h3 id="sweep"><code>sweep</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.28/wandb/apis/public.py#L499-L515">View source</a>

<pre><code>sweep(
    path=&#x27;&#x27;
)</code></pre>

Returns a sweep by parsing path in the form `entity/project/sweep_id`.


<!-- Tabular view -->
<table>
<tr><th>Arguments</th></tr>

<tr>
<td>
<code>path</code>
</td>
<td>
(str, optional) path to sweep in the form entity/project/sweep_id.  If api.entity
is set, this can be in the form project/sweep_id and if <code>api.project</code> is set
this can just be the sweep_id.
</td>
</tr>
</table>



<!-- Tabular view -->
<table>
<tr><th>Returns</th></tr>
<tr>
<td>
A <code>Sweep</code> object.
</td>
</tr>

</table>



<h3 id="sync_tensorboard"><code>sync_tensorboard</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.28/wandb/apis/public.py#L253-L274">View source</a>

<pre><code>sync_tensorboard(
    root_dir, run_id=None, project=None, entity=None
)</code></pre>

Sync a local directory containing tfevent files to wandb






<!-- Tabular view -->
<table>
<tr><th>Class Variables</th></tr>

<tr>
<td>
VIEWER_QUERY<a id="VIEWER_QUERY"></a>
</td>
<td>

</td>
</tr>
</table>

