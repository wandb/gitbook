# Table



[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/v0.10.31/wandb/data_types.py#L147-L815)




The Table class is used to display and analyze tabular data.

<pre><code>Table(
    columns=None, data=None, rows=None, dataframe=None, dtype=None, optional=(True),
    allow_mixed_types=(False)
)</code></pre>




This class is the primary class used to generate the Table Visualizer
in the UI: https://docs.wandb.ai/guides/data-vis/tables.

Tables can be constructed with initial data using the <code>data</code> or
<code>dataframe</code> parameters. Additionally, users can add data to Tables
incrementally by using the <code>add_data</code>, <code>add_column</code>, and
<code>add_computed_column</code> functions for adding rows, columns, and computed
columns, respectively.

Tables can be logged directly to runs using `run.log({"my_table": table})`
or added to artifacts using `artifact.add(table, "my_table")`. Tables added
directly to runs will produce a corresponding Table Visualizer in the
Workspace which can be used for further analysis and exporting to reports.
Tables added to artifacts can be viewed in the Artifact Tab and will render
an equivalent Table Visualizer directly in the artifact browser.

Note that Tables support numerous types of data: traditional scalar values,
numpy arrays, and most subclasses of wandb.data_types.Media. This means you
can embed Images, Video, Audio, and other sorts of rich, annotated media
directly in Tables, alongside other traditional scalar values. Tables expect
each value for a column to be of the same type. By default, a column supports
optional values, but not mixed values. If you absolutely need to mix types,
you can enable the <code>allow_mixed_types</code> flag which will disable type checking
on the data. This will result in some table analytics features being disabled
due to lack of consistent typing.

<!-- Tabular view -->
<table>
<tr><th>Arguments</th></tr>

<tr>
<td>
<code>columns</code>
</td>
<td>
(List[str]) Names of the columns in the table.
Defaults to ["Input", "Output", "Expected"].
</td>
</tr><tr>
<td>
<code>data</code>
</td>
<td>
(List[List[any]]) 2D row-oriented array of values.
</td>
</tr><tr>
<td>
<code>dataframe</code>
</td>
<td>
(pandas.DataFrame) DataFrame object used to create the table.
When set, <code>data</code> and <code>columns</code> arguments are ignored.
</td>
</tr><tr>
<td>
<code>optional</code>
</td>
<td>
(Union[bool,List[bool]]) Determines if <code>None</code> values are allowed. Default to True
- If a singular bool value, then the optionality is enforced for all
columns specified at construction time
- If a list of bool values, then the optionality is applied to each
column - should be the same length as <code>columns</code>
applies to all columns. A list of bool values applies to each respective column.
</td>
</tr><tr>
<td>
<code>allow_mixed_types</code>
</td>
<td>
(bool) Determines if columns are allowed to have mixed types
(disables type validation). Defaults to False
</td>
</tr>
</table>



## Methods

<h3 id="add_column"><code>add_column</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.31/wandb/data_types.py#L713-L752">View source</a>

<pre><code>add_column(
    name, data, optional=(False)
)</code></pre>

Add a column of data to the table.

Arguments
    name: (str) - the unique name of the column
    data: (list | np.array) - a column of homogenous data
    optional: (bool) - if null-like values are permitted

<h3 id="add_computed_columns"><code>add_computed_columns</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.31/wandb/data_types.py#L795-L815">View source</a>

<pre><code>add_computed_columns(
    fn
)</code></pre>

Adds one or more computed columns based on existing data


<!-- Tabular view -->
<table>
<tr><th>Args</th></tr>
<tr>
<td>
fn (function): A function which accepts one or two paramters: ndx (int) and row (dict)
which is expected to return a dict representing new columns for that row, keyed
by the new column names.
- <code>ndx</code> is an integer representing the index of the row. Only included if <code>include_ndx</code>
is set to true
- <code>row</code> is a dictionary keyed by existing columns
</td>
</tr>

</table>



<h3 id="add_data"><code>add_data</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.31/wandb/data_types.py#L397-L427">View source</a>

<pre><code>add_data(
    *data
)</code></pre>

Add a row of data to the table. Argument length should match column length


<h3 id="add_row"><code>add_row</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.31/wandb/data_types.py#L393-L395">View source</a>

<pre><code>add_row(
    *row
)</code></pre>




<h3 id="cast"><code>cast</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.31/wandb/data_types.py#L292-L346">View source</a>

<pre><code>cast(
    col_name, dtype, optional=(False)
)</code></pre>

Casts a column to a specific type


<!-- Tabular view -->
<table>
<tr><th>Arguments</th></tr>

<tr>
<td>
<code>col_name</code>
</td>
<td>
(str) - name of the column to cast
</td>
</tr><tr>
<td>
<code>dtype</code>
</td>
<td>
(class, wandb.wandb_sdk.interface._dtypes.Type, any) - the target dtype. Can be one of
normal python class, internal WB type, or an example object (eg. an instance of wandb.Image or wandb.Classes)
</td>
</tr><tr>
<td>
<code>optional</code>
</td>
<td>
(bool) - if the column should allow Nones
</td>
</tr>
</table>



<h3 id="get_column"><code>get_column</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.31/wandb/data_types.py#L754-L777">View source</a>

<pre><code>get_column(
    name, convert_to=None
)</code></pre>

Retrieves a column of data from the table

Arguments
    name: (str) - the name of the column
    convert_to: (str, optional)
        - "numpy": will convert the underlying data to numpy object

<h3 id="get_index"><code>get_index</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.31/wandb/data_types.py#L779-L786">View source</a>

<pre><code>get_index()</code></pre>

Returns an array of row indexes which can be used in other tables to create links


<h3 id="index_ref"><code>index_ref</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.31/wandb/data_types.py#L788-L793">View source</a>

<pre><code>index_ref(
    index
)</code></pre>

Get a reference to a particular row index in the table


<h3 id="iterrows"><code>iterrows</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.31/wandb/data_types.py#L592-L605">View source</a>

<pre><code>iterrows()</code></pre>

Iterate over rows as (ndx, row)
Yields
------
index : int
    The index of the row. Using this value in other WandB tables
    will automatically build a relationship between the tables
row : List[any]
    The data of the row

<h3 id="set_fk"><code>set_fk</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.31/wandb/data_types.py#L612-L616">View source</a>

<pre><code>set_fk(
    col_name, table, table_col
)</code></pre>




<h3 id="set_pk"><code>set_pk</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.31/wandb/data_types.py#L607-L610">View source</a>

<pre><code>set_pk(
    col_name
)</code></pre>








<!-- Tabular view -->
<table>
<tr><th>Class Variables</th></tr>

<tr>
<td>
MAX_ARTIFACT_ROWS<a id="MAX_ARTIFACT_ROWS"></a>
</td>
<td>
<code>200000</code>
</td>
</tr><tr>
<td>
MAX_ROWS<a id="MAX_ROWS"></a>
</td>
<td>
<code>10000</code>
</td>
</tr>
</table>

