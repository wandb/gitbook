# Artifact

<!-- Insert buttons and diff -->


[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_artifacts.py#L37-L302)




An artifact object you can write files into, and pass to log_artifact.

<pre><code>Artifact(
    name, type, description=None, metadata=None
)</code></pre>



<!-- Placeholder for "Used in" -->




<!-- Tabular view -->
<table>
<tr><th>Attributes</th></tr>

<tr>
<td>
<code>digest</code>
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
<code>project</code>
</td>
<td>

</td>
</tr>
</table>



## Methods

<h3 id="add"><code>add</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_artifacts.py#L220-L262">View source</a>

<pre><code>add(
    obj, name
)</code></pre>

Adds `obj` to the artifact, located at `name`. You can
use <a href="../library/Artifact.md#get"><code>Artifact.get(name)</code></a> after downloading the artifact to retrieve this object.

<!-- Tabular view -->
<table>
<tr><th>Arguments</th></tr>
<tr>
<td>
obj (wandb.WBValue): The object to save in an artifact
name (str): The path to save
</td>
</tr>

</table>



<h3 id="add_dir"><code>add_dir</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_artifacts.py#L150-L183">View source</a>

<pre><code>add_dir(
    local_path, name=None
)</code></pre>




<h3 id="add_file"><code>add_file</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_artifacts.py#L124-L148">View source</a>

<pre><code>add_file(
    local_path, name=None, is_tmp=False
)</code></pre>

Adds a local file to the artifact


<!-- Tabular view -->
<table>
<tr><th>Args</th></tr>
<tr>
<td>
local_path (str): path to the file
name (str, optional): new path and filename to assign inside artifact. Defaults to None.
is_tmp (bool, optional): If true, then the file is renamed deterministically. Defaults to False.
</td>
</tr>

</table>



<!-- Tabular view -->
<table>
<tr><th>Returns</th></tr>

<tr>
<td>
<code>ArtifactManifestEntry</code>
</td>
<td>
the added entry
</td>
</tr>
</table>



<h3 id="add_reference"><code>add_reference</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_artifacts.py#L185-L218">View source</a>

<pre><code>add_reference(
    uri, name=None, checksum=True, max_objects=None
)</code></pre>

adds `uri` to the artifact via a reference, located at `name`. 
You can use <a href="../library/Artifact.md#get_path"><code>Artifact.get_path(name)</code></a> to retrieve this object.

<!-- Tabular view -->
<table>
<tr><th>Arguments</th></tr>
<tr>
<td>
uri (str) - the URI path of the reference to add. Can be an object returned from
Artifact.get_path to store a reference to another artifact's entry.
name (str) - the path to save
</td>
</tr>

</table>



<h3 id="download"><code>download</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_artifacts.py#L274-L275">View source</a>

<pre><code>download()</code></pre>




<h3 id="finalize"><code>finalize</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_artifacts.py#L280-L286">View source</a>

<pre><code>finalize()</code></pre>




<h3 id="get"><code>get</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_artifacts.py#L277-L278">View source</a>

<pre><code>get()</code></pre>




<h3 id="get_added_local_path_name"><code>get_added_local_path_name</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_artifacts.py#L264-L269">View source</a>

<pre><code>get_added_local_path_name(
    local_path
)</code></pre>

If local_path was already added to artifact, return its internal name.


<h3 id="get_path"><code>get_path</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_artifacts.py#L271-L272">View source</a>

<pre><code>get_path(
    name
)</code></pre>




<h3 id="new_file"><code>new_file</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_artifacts.py#L109-L122">View source</a>

<pre><code>@contextlib.contextmanager</code>
<code>new_file(
    name, mode=&#x27;w&#x27;
)</code></pre>






