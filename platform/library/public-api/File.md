# File

<!-- Insert buttons and diff -->


[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L1603-L1704)




File is a class associated with a file saved by wandb.

<pre><code>File(
    client, attrs
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
<code>direct_url</code>
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
<code>md5</code>
</td>
<td>

</td>
</tr><tr>
<td>
<code>mimetype</code>
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
<code>updated_at</code>
</td>
<td>

</td>
</tr><tr>
<td>
<code>url</code>
</td>
<td>

</td>
</tr>
</table>



## Methods

<h3 id="delete"><code>delete</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L1686-L1699">View source</a>

<pre><code>delete()</code></pre>




<h3 id="download"><code>download</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L1663-L1684">View source</a>

<pre><code>download(
    root=&#x27;.&#x27;, replace=False
)</code></pre>

Downloads a file previously saved by a run from the wandb server.


<!-- Tabular view -->
<table>
<tr><th>Arguments</th></tr>
<tr>
<td>
replace (boolean): If `True`, download will overwrite a local file
if it exists. Defaults to `False`.
root (str): Local directory to save the file.  Defaults to ".".
</td>
</tr>

</table>



<!-- Tabular view -->
<table>
<tr><th>Raises</th></tr>
<tr>
<td>
`ValueError` if file already exists and replace=False
</td>
</tr>

</table>





