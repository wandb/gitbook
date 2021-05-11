# Sweep



[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/v0.10.30/wandb/apis/public.py#L1410-L1593)




A set of runs associated with a sweep.

<pre><code>Sweep(
    client, entity, project, sweep_id, attrs={}
)</code></pre>





#### Examples:

Instantiate with:
```
api = wandb.Api()
sweep = api.sweep(path/to/sweep)
```





<!-- Tabular view -->
<table>
<tr><th>Attributes</th></tr>

<tr>
<td>
<code>runs</code>
</td>
<td>
(<code>Runs</code>) list of runs
</td>
</tr><tr>
<td>
<code>id</code>
</td>
<td>
(str) sweep id
</td>
</tr><tr>
<td>
<code>project</code>
</td>
<td>
(str) name of project
</td>
</tr><tr>
<td>
<code>config</code>
</td>
<td>
(str) dictionary of sweep configuration
</td>
</tr>
</table>



## Methods

<h3 id="best_run"><code>best_run</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.30/wandb/apis/public.py#L1501-L1524">View source</a>

<pre><code>best_run(
    order=None
)</code></pre>

Returns the best run sorted by the metric defined in config or the order passed in


<h3 id="get"><code>get</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.30/wandb/apis/public.py#L1540-L1590">View source</a>

<pre><code>@classmethod</code>
<code>get(
    client, entity=None, project=None, sid=None, withRuns=(True), order=None,
    query=None, **kwargs
)</code></pre>

Execute a query against the cloud backend


<h3 id="load"><code>load</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.30/wandb/apis/public.py#L1481-L1490">View source</a>

<pre><code>load(
    force=(False)
)</code></pre>




<h3 id="snake_to_camel"><code>snake_to_camel</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.30/wandb/apis/public.py#L567-L569">View source</a>

<pre><code>snake_to_camel(
    string
)</code></pre>








<!-- Tabular view -->
<table>
<tr><th>Class Variables</th></tr>

<tr>
<td>
QUERY<a id="QUERY"></a>
</td>
<td>

</td>
</tr>
</table>

