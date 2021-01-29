# Api

<!-- Insert buttons and diff -->


[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L182-L521)




Used for querying the wandb server.

<pre class="devsite-click-to-copy prettyprint lang-py tfo-signature-link">
<code>wandb.Api(
    overrides={}
)
</code></pre>



<!-- Placeholder for "Used in" -->


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
(dict) You can set `base_url` if you are using a wandb server
other than https://api.wandb.ai.
You can also set defaults for `entity`, `project`, and `run`.
</td>
</tr>
</table>





<!-- Tabular view -->
<table>
<tr><th>Attributes</th></tr>

<tr>
<td>
<code>api_key</code>
</td>
<td>

</td>
</tr><tr>
<td>
<code>client</code>
</td>
<td>

</td>
</tr><tr>
<td>
<code>default_entity</code>
</td>
<td>

</td>
</tr><tr>
<td>
<code>user_agent</code>
</td>
<td>

</td>
</tr>
</table>



## Methods

<h3 id="artifact"><code>artifact</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L500-L521">View source</a>

<pre class="devsite-click-to-copy prettyprint lang-py tfo-signature-link">
<code>artifact(
    name, type=None
)
</code></pre>

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
A `Artifact` object.
</td>
</tr>

</table>



<h3 id="artifact_type"><code>artifact_type</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L489-L492">View source</a>

<pre class="devsite-click-to-copy prettyprint lang-py tfo-signature-link">
<code>artifact_type(
    type_name, project=None
)
</code></pre>




<h3 id="artifact_types"><code>artifact_types</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L484-L487">View source</a>

<pre class="devsite-click-to-copy prettyprint lang-py tfo-signature-link">
<code>artifact_types(
    project=None
)
</code></pre>




<h3 id="artifact_versions"><code>artifact_versions</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L494-L498">View source</a>

<pre class="devsite-click-to-copy prettyprint lang-py tfo-signature-link">
<code>artifact_versions(
    type_name, name, per_page=50
)
</code></pre>




<h3 id="create_run"><code>create_run</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L244-L247">View source</a>

<pre class="devsite-click-to-copy prettyprint lang-py tfo-signature-link">
<code>create_run(
    **kwargs
)
</code></pre>




<h3 id="flush"><code>flush</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L275-L281">View source</a>

<pre class="devsite-click-to-copy prettyprint lang-py tfo-signature-link">
<code>flush()
</code></pre>

The api object keeps a local cache of runs, so if the state of the run may
change while executing your script you must clear the local cache with `api.flush()`
to get the latest values associated with the run.

<h3 id="projects"><code>projects</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L338-L360">View source</a>

<pre class="devsite-click-to-copy prettyprint lang-py tfo-signature-link">
<code>projects(
    entity=None, per_page=200
)
</code></pre>

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
default entity passed to `Api`.  If no default entity, will raise a `ValueError`.
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
A `Projects` object which is an iterable collection of `Project` objects.
</td>
</tr>

</table>



<h3 id="reports"><code>reports</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L362-L393">View source</a>

<pre class="devsite-click-to-copy prettyprint lang-py tfo-signature-link">
<code>reports(
    path=&#x27;&#x27;, name=None, per_page=50
)
</code></pre>

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
A `Reports` object which is an iterable collection of `BetaReport` objects.
</td>
</tr>

</table>



<h3 id="run"><code>run</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L448-L464">View source</a>

<pre class="devsite-click-to-copy prettyprint lang-py tfo-signature-link">
<code>run(
    path=&#x27;&#x27;
)
</code></pre>

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
and if `api.project` is set this can just be the run_id.
</td>
</tr>
</table>



<!-- Tabular view -->
<table>
<tr><th>Returns</th></tr>
<tr>
<td>
A `Run` object.
</td>
</tr>

</table>



<h3 id="runs"><code>runs</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L395-L446">View source</a>

<pre class="devsite-click-to-copy prettyprint lang-py tfo-signature-link">
<code>runs(
    path=&#x27;&#x27;, filters={}, order=&#x27;-created_at&#x27;, per_page=50
)
</code></pre>

Return a set of runs from a project that match the filters provided.

You can filter by `config.*`, `summary.*`, `state`, `entity`, `createdAt`, etc.

#### Examples:

Find runs in my_project config.experiment_name has been set to "foo"
```
api.runs(path="my_entity/my_project", {"config.experiment_name": "foo"})
```

Find runs in my_project config.experiment_name has been set to "foo" or "bar"
```
api.runs(path="my_entity/my_project",
    {"$or": [{"config.experiment_name": "foo"}, {"config.experiment_name": "bar"}]})
```

Find runs in my_project sorted by ascending loss
```
api.runs(path="my_entity/my_project", {"order": "+summary_metrics.loss"})
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
(str) Order can be `created_at`, `heartbeat_at`, `config.*.value`, or `summary_metrics.*`.
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
A `Runs` object, which is an iterable collection of `Run` objects.
</td>
</tr>

</table>



<h3 id="sweep"><code>sweep</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L466-L482">View source</a>

<pre class="devsite-click-to-copy prettyprint lang-py tfo-signature-link">
<code>sweep(
    path=&#x27;&#x27;
)
</code></pre>

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
is set, this can be in the form project/sweep_id and if `api.project` is set
this can just be the sweep_id.
</td>
</tr>
</table>



<!-- Tabular view -->
<table>
<tr><th>Returns</th></tr>
<tr>
<td>
A `Sweep` object.
</td>
</tr>

</table>







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

