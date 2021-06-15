# wandb.data\_types.Table

[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/v0.10.32/wandb/data_types.py#L135-L802)

The Table class is used to display and analyze tabular data.

```python
Table(
    columns=None, data=None, rows=None, dataframe=None, dtype=None, optional=(True),
    allow_mixed_types=(False)
)
```

This class is the primary class used to generate the Table Visualizer in the UI: [https://docs.wandb.ai/guides/data-vis/tables](https://docs.wandb.ai/guides/data-vis/tables).

Tables can be constructed with initial data using the `data` or `dataframe` parameters. Additionally, users can add data to Tables incrementally by using the `add_data`, `add_column`, and `add_computed_column` functions for adding rows, columns, and computed columns, respectively.

Tables can be logged directly to runs using `run.log({"my_table": table})` or added to artifacts using `artifact.add(table, "my_table")`. Tables added directly to runs will produce a corresponding Table Visualizer in the Workspace which can be used for further analysis and exporting to reports. Tables added to artifacts can be viewed in the Artifact Tab and will render an equivalent Table Visualizer directly in the artifact browser.

Note that Tables support numerous types of data: traditional scalar values, numpy arrays, and most subclasses of wandb.data\_types.Media. This means you can embed Images, Video, Audio, and other sorts of rich, annotated media directly in Tables, alongside other traditional scalar values. Tables expect each value for a column to be of the same type. By default, a column supports optional values, but not mixed values. If you absolutely need to mix types, you can enable the `allow_mixed_types` flag which will disable type checking on the data. This will result in some table analytics features being disabled due to lack of consistent typing.

| Arguments |  |
| :--- | :--- |
| `columns` | \(List\[str\]\) Names of the columns in the table. Defaults to \["Input", "Output", "Expected"\]. |
| `data` | \(List\[List\[any\]\]\) 2D row-oriented array of values. |
| `dataframe` | \(pandas.DataFrame\) DataFrame object used to create the table. When set, `data` and `columns` arguments are ignored. |
| `optional` | \(Union\[bool,List\[bool\]\]\) Determines if `None` values are allowed. Default to True - If a singular bool value, then the optionality is enforced for all columns specified at construction time - If a list of bool values, then the optionality is applied to each column - should be the same length as `columns` applies to all columns. A list of bool values applies to each respective column. |
| `allow_mixed_types` | \(bool\) Determines if columns are allowed to have mixed types \(disables type validation\). Defaults to False |

## Methods

### `add_column` <a id="add_column"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.32/wandb/data_types.py#L700-L739)

```python
add_column(
    name, data, optional=(False)
)
```

Add a column of data to the table.

Arguments name: \(str\) - the unique name of the column data: \(list \| np.array\) - a column of homogenous data optional: \(bool\) - if null-like values are permitted

### `add_computed_columns` <a id="add_computed_columns"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.32/wandb/data_types.py#L782-L802)

```python
add_computed_columns(
    fn
)
```

Adds one or more computed columns based on existing data

| Args |  |
| :--- | :--- |
| fn \(function\): A function which accepts one or two paramters: ndx \(int\) and row \(dict\) which is expected to return a dict representing new columns for that row, keyed by the new column names. - `ndx` is an integer representing the index of the row. Only included if `include_ndx` is set to true - `row` is a dictionary keyed by existing columns |  |

### `add_data` <a id="add_data"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.32/wandb/data_types.py#L385-L415)

```python
add_data(
    *data
)
```

Add a row of data to the table. Argument length should match column length

### `add_row` <a id="add_row"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.32/wandb/data_types.py#L381-L383)

```python
add_row(
    *row
)
```

### `cast` <a id="cast"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.32/wandb/data_types.py#L280-L334)

```python
cast(
    col_name, dtype, optional=(False)
)
```

Casts a column to a specific type

| Arguments |  |
| :--- | :--- |
| `col_name` | \(str\) - name of the column to cast |
| `dtype` | \(class, wandb.wandb\_sdk.interface.\_dtypes.Type, any\) - the target dtype. Can be one of normal python class, internal WB type, or an example object \(eg. an instance of wandb.Image or wandb.Classes\) |
| `optional` | \(bool\) - if the column should allow Nones |

### `get_column` <a id="get_column"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.32/wandb/data_types.py#L741-L764)

```python
get_column(
    name, convert_to=None
)
```

Retrieves a column of data from the table

Arguments name: \(str\) - the name of the column convert\_to: \(str, optional\)

* "numpy": will convert the underlying data to numpy object

### `get_index` <a id="get_index"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.32/wandb/data_types.py#L766-L773)

```python
get_index()
```

Returns an array of row indexes which can be used in other tables to create links

### `index_ref` <a id="index_ref"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.32/wandb/data_types.py#L775-L780)

```python
index_ref(
    index
)
```

Get a reference to a particular row index in the table

### `iterrows` <a id="iterrows"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.32/wandb/data_types.py#L579-L592)

```python
iterrows()
```

Iterate over rows as \(ndx, row\)

## Yields

index : int The index of the row. Using this value in other WandB tables will automatically build a relationship between the tables row : List\[any\] The data of the row

### `set_fk` <a id="set_fk"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.32/wandb/data_types.py#L599-L603)

```python
set_fk(
    col_name, table, table_col
)
```

### `set_pk` <a id="set_pk"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.32/wandb/data_types.py#L594-L597)

```python
set_pk(
    col_name
)
```

| Class Variables |  |
| :--- | :--- |
| `MAX_ARTIFACT_ROWS` | `200000` |
| `MAX_ROWS` | `10000` |

