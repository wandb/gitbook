# Html

<!-- Insert buttons and diff -->


[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/master/wandb/sdk/data_types.py#L810-L900)




Wandb class for arbitrary html

<pre><code>Html(
    data: Union[str, 'TextIO'],
    inject: bool = (True)
) -> None</code></pre>



<!-- Placeholder for "Used in" -->


<!-- Tabular view -->
<table>
<tr><th>Arguments</th></tr>

<tr>
<td>
<code>data</code>
</td>
<td>
(string or io object) HTML to display in wandb
</td>
</tr><tr>
<td>
<code>inject</code>
</td>
<td>
(boolean) Add a stylesheet to the HTML object.  If set
to False the HTML will pass through unchanged.
</td>
</tr>
</table>



## Methods

<h3 id="inject_head"><code>inject_head</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/sdk/data_types.py#L852-L867">View source</a>

<pre><code>inject_head() -> None</code></pre>








<!-- Tabular view -->
<table>
<tr><th>Class Variables</th></tr>

<tr>
<td>
artifact_type<a id="artifact_type"></a>
</td>
<td>
`'html-file'`
</td>
</tr>
</table>

