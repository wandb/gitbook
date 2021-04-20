# Sweep



[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/18a721ba0f880a64aea802ebd3e2862f394610f4/wandb/apis/public.py#L1389-L1567)




A set of runs associated with a sweep

<pre><code>Sweep(
    client, entity, project, sweep_id, attrs={}
)</code></pre>





#### Instantiate with:

api.sweep(sweep_path)





<!-- Tabular view -->
<table>
<tr><th>Attributes</th></tr>

<tr>
<td>
<code>config</code>
</td>
<td>

</td>
</tr><tr>
<td>
<code>entity</code>
</td>
<td>

</td>
</tr><tr>
<td>
<code>order</code>
</td>
<td>

</td>
</tr><tr>
<td>
<code>path</code>
</td>
<td>

</td>
</tr><tr>
<td>
<code>url</code>
</td>
<td>

</td>
</tr><tr>
<td>
<code>username</code>
</td>
<td>

</td>
</tr>
</table>



## Methods

<h3 id="best_run"><code>best_run</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/18a721ba0f880a64aea802ebd3e2862f394610f4/wandb/apis/public.py#L1475-L1498">View source</a>

<pre><code>best_run(
    order=None
)</code></pre>

Returns the best run sorted by the metric defined in config or the order passed in


<h3 id="get"><code>get</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/18a721ba0f880a64aea802ebd3e2862f394610f4/wandb/apis/public.py#L1514-L1564">View source</a>

<pre><code>@classmethod</code>
<code>get(
    client, entity=None, project=None, sid=None, withRuns=(True), order=None,
    query=None, **kwargs
)</code></pre>

Execute a query against the cloud backend


<h3 id="load"><code>load</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/18a721ba0f880a64aea802ebd3e2862f394610f4/wandb/apis/public.py#L1455-L1464">View source</a>

<pre><code>load(
    force=(False)
)</code></pre>




<h3 id="snake_to_camel"><code>snake_to_camel</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/18a721ba0f880a64aea802ebd3e2862f394610f4/wandb/apis/public.py#L561-L563">View source</a>

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

