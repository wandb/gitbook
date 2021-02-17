# Artifact

<!-- Insert buttons and diff -->


[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L2447-L3148)






<pre><code>Artifact(
    client, entity, project, name, attrs=None
)</code></pre>



<!-- Placeholder for "Used in" -->




<!-- Tabular view -->
<table>
<tr><th>Attributes</th></tr>

<tr>
<td>
<code>aliases</code>
</td>
<td>

</td>
</tr><tr>
<td>
<code>created_at</code>
</td>
<td>

</td>
</tr><tr>
<td>
<code>description</code>
</td>
<td>

</td>
</tr><tr>
<td>
<code>digest</code>
</td>
<td>

</td>
</tr><tr>
<td>
<code>id</code>
</td>
<td>

</td>
</tr><tr>
<td>
<code>manifest</code>
</td>
<td>

</td>
</tr><tr>
<td>
<code>metadata</code>
</td>
<td>

</td>
</tr><tr>
<td>
<code>name</code>
</td>
<td>

</td>
</tr><tr>
<td>
<code>size</code>
</td>
<td>

</td>
</tr><tr>
<td>
<code>state</code>
</td>
<td>

</td>
</tr><tr>
<td>
<code>type</code>
</td>
<td>

</td>
</tr><tr>
<td>
<code>updated_at</code>
</td>
<td>

</td>
</tr>
</table>



## Methods

<h3 id="add_dir"><code>add_dir</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L2666-L2667">View source</a>

<pre><code>add_dir(
    path, name=None
)</code></pre>




<h3 id="add_file"><code>add_file</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L2663-L2664">View source</a>

<pre><code>add_file(
    path, name=None
)</code></pre>




<h3 id="add_reference"><code>add_reference</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L2669-L2670">View source</a>

<pre><code>add_reference(
    path, name=None
)</code></pre>




<h3 id="delete"><code>delete</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L2643-L2658">View source</a>

<pre><code>delete()</code></pre>

Delete artifact and its files.


<h3 id="download"><code>download</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L2799-L2841">View source</a>

<pre><code>download(
    root=None, recursive=False
)</code></pre>

Download the artifact to dir specified by the <root>


<!-- Tabular view -->
<table>
<tr><th>Arguments</th></tr>
<tr>
<td>
root (str, optional): directory to download artifact to. If None
artifact will be downloaded to './artifacts/<self.name>/'
recursive (bool, optional): if set to true, then all dependent artifacts are
eagerly downloaded as well. If false, then the dependent artifact will
only be downloaded when needed.
</td>
</tr>

</table>



<!-- Tabular view -->
<table>
<tr><th>Returns</th></tr>
<tr>
<td>
The path to the downloaded contents.
</td>
</tr>

</table>



<h3 id="expected_type"><code>expected_type</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L2601-L2641">View source</a>

<pre><code>@staticmethod</code>
<code>expected_type(
    client, name, entity_name, project_name
)</code></pre>

Returns the expected type for a given artifact name and project


<h3 id="file"><code>file</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L2843-L2864">View source</a>

<pre><code>file(
    root=None
)</code></pre>

Download a single file artifact to dir specified by the <root>


<!-- Tabular view -->
<table>
<tr><th>Arguments</th></tr>
<tr>
<td>
root (str, optional): directory to download artifact to. If None
artifact will be downloaded to './artifacts/<self.name>/'
</td>
</tr>

</table>



<!-- Tabular view -->
<table>
<tr><th>Returns</th></tr>
<tr>
<td>
The full path of the downloaded file
</td>
</tr>

</table>



<h3 id="from_id"><code>from_id</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L2469-L2509">View source</a>

<pre><code>@classmethod</code>
<code>from_id(
    artifact_id, client
)</code></pre>




<h3 id="get"><code>get</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L2763-L2797">View source</a>

<pre><code>get(
    name
)</code></pre>

Returns the wandb.Media resource stored in the artifact. Media can be
stored in the artifact via Artifact#add(obj: wandbMedia, name: str)`
Arguments:
    name (str): name of resource.

<!-- Tabular view -->
<table>
<tr><th>Returns</th></tr>
<tr>
<td>
A `wandb.Media` which has been stored at `name`
</td>
</tr>

</table>



<h3 id="get_path"><code>get_path</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L2692-L2761">View source</a>

<pre><code>get_path(
    name
)</code></pre>




<h3 id="logged_by"><code>logged_by</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L3115-L3148">View source</a>

<pre><code>logged_by()</code></pre>

Retrieves the run which logged this artifact


<!-- Tabular view -->
<table>
<tr><th>Returns</th></tr>

<tr>
<td>
<code>Run</code>
</td>
<td>
Run object which logged this artifact
</td>
</tr>
</table>



<h3 id="new_file"><code>new_file</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L2660-L2661">View source</a>

<pre><code>new_file(
    name, mode=None
)</code></pre>




<h3 id="save"><code>save</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L2877-L2915">View source</a>

<pre><code>save()</code></pre>

Persists artifact changes to the wandb backend.


<h3 id="used_by"><code>used_by</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L3071-L3113">View source</a>

<pre><code>used_by()</code></pre>

Retrieves the runs which use this artifact directly


<!-- Tabular view -->
<table>
<tr><th>Returns</th></tr>
<tr>
<td>
[Run]: a list of Run objects which use this artifact
</td>
</tr>

</table>



<h3 id="verify"><code>verify</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L2917-L2942">View source</a>

<pre><code>verify(
    root=None
)</code></pre>

Verify an artifact by checksumming its downloaded contents.

Raises a ValueError if the verification fails. Does not verify downloaded
reference files.

<!-- Tabular view -->
<table>
<tr><th>Arguments</th></tr>
<tr>
<td>
root (str, optional): directory to download artifact to. If None
artifact will be downloaded to './artifacts/<self.name>/'
</td>
</tr>

</table>







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

