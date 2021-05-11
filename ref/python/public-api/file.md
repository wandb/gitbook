# File



[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/v0.10.30/wandb/apis/public.py#L1662-L1765)




File is a class associated with a file saved by wandb.

<pre><code>File(
    client, attrs
)</code></pre>







<!-- Tabular view -->
<table>
<tr><th>Attributes</th></tr>


</table>



## Methods

<h3 id="delete"><code>delete</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.30/wandb/apis/public.py#L1745-L1758">View source</a>

<pre><code>delete()</code></pre>




<h3 id="download"><code>download</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.30/wandb/apis/public.py#L1722-L1743">View source</a>

<pre><code>download(
    root=&#x27;.&#x27;, replace=(False)
)</code></pre>

Downloads a file previously saved by a run from the wandb server.


<!-- Tabular view -->
<table>
<tr><th>Arguments</th></tr>
<tr>
<td>
replace (boolean): If <code>True</code>, download will overwrite a local file
if it exists. Defaults to <code>False</code>.
root (str): Local directory to save the file.  Defaults to ".".
</td>
</tr>

</table>



<!-- Tabular view -->
<table>
<tr><th>Raises</th></tr>
<tr>
<td>
<code>ValueError</code> if file already exists and replace=False
</td>
</tr>

</table>





