# Table

<!-- Insert buttons and diff -->


[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/master/wandb/data_types.py#L109-L398)




This is a table designed to display sets of records.

<pre><code>Table(
    columns=None, data=None, rows=None, dataframe=None, dtype=None, optional=(True),
    allow_mixed_types=(False)
)</code></pre>



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
optional (Union[bool,List[bool]]): If None values are allowed. Singular bool
applies to all columns. A list of bool values applies to each respective column.
Default to True.
allow_mixed_types (bool): Determines if columns are allowed to have mixed types (disables type validation). Defaults to False
</td>
</tr>
</table>



## Methods

<h3 id="add_data"><code>add_data</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/data_types.py#L264-L273">View source</a>

<pre><code>add_data(
    *data
)</code></pre>

Add a row of data to the table. Argument length should match column length


<h3 id="add_row"><code>add_row</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/data_types.py#L260-L262">View source</a>

<pre><code>add_row(
    *row
)</code></pre>




<h3 id="cast"><code>cast</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/data_types.py#L222-L239">View source</a>

<pre><code>cast(
    col_name, dtype, optional=(False)
)</code></pre>




<h3 id="iterrows"><code>iterrows</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/data_types.py#L388-L398">View source</a>

<pre><code>iterrows()</code></pre>

Iterate over rows as (ndx, row)
Yields
------
index : int
    The index of the row.
row : List[any]
    The data of the row





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

