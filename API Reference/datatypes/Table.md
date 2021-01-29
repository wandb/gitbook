# Table

<!-- Insert buttons and diff -->


[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/master/wandb/data_types.py#L481-L749)




This is a table designed to display sets of records.

<pre class="devsite-click-to-copy prettyprint lang-py tfo-signature-link">
<code>datatypes.Table(
    columns=None, data=None, rows=None, dataframe=None, dtype=None, optional=True
)
</code></pre>



<!-- Placeholder for "Used in" -->


<!-- Tabular view -->
<table>
<tr><th>Arguments</th></tr>

<tr>
<td>
<code>columns</code>
</td>
<td>
([str]) Names of the columns in the table.
Defaults to ["Input", "Output", "Expected"].
</td>
</tr><tr>
<td>
<code>data</code>
</td>
<td>
(array) 2D Array of values that will be displayed as strings.
</td>
</tr><tr>
<td>
<code>dataframe</code>
</td>
<td>
(pandas.DataFrame) DataFrame object used to create the table.
When set, the other arguments are ignored.
</td>
</tr>
</table>



## Methods

<h3 id="add_data"><code>add_data</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/data_types.py#L628-L637">View source</a>

<pre class="devsite-click-to-copy prettyprint lang-py tfo-signature-link">
<code>add_data(
    *data
)
</code></pre>

Add a row of data to the table. Argument length should match column length


<h3 id="add_row"><code>add_row</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/data_types.py#L624-L626">View source</a>

<pre class="devsite-click-to-copy prettyprint lang-py tfo-signature-link">
<code>add_row(
    *row
)
</code></pre>




<h3 id="cast"><code>cast</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/data_types.py#L587-L604">View source</a>

<pre class="devsite-click-to-copy prettyprint lang-py tfo-signature-link">
<code>cast(
    col_name, dtype, optional=False
)
</code></pre>








<!-- Tabular view -->
<table>
<tr><th>Class Variables</th></tr>

<tr>
<td>
MAX_ARTIFACT_ROWS<a id="MAX_ARTIFACT_ROWS"></a>
</td>
<td>
`200000`
</td>
</tr><tr>
<td>
MAX_ROWS<a id="MAX_ROWS"></a>
</td>
<td>
`10000`
</td>
</tr><tr>
<td>
artifact_type<a id="artifact_type"></a>
</td>
<td>
`'table'`
</td>
</tr>
</table>

