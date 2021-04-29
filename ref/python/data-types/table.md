# wandb.data\_types.Table

[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/v0.10.28/wandb/data_types.py#L147-L785)

This is a table designed to display sets of records.

```text
Table(
    columns=None, data=None, rows=None, dataframe=None, dtype=None, optional=(True),
    allow_mixed_types=(False)
)
```

| Arguments |  |
| :--- | :--- |
|  `columns` |  \(\[str\]\) Names of the columns in the table. Defaults to \["Input", "Output", "Expected"\]. |
|  `data` |  \(array\) 2D Array of values that will be displayed as strings. |
|  `dataframe` |  \(pandas.DataFrame\) DataFrame object used to create the table. When set, the other arguments are ignored. optional \(Union\[bool,List\[bool\]\]\): If None values are allowed. Singular bool applies to all columns. A list of bool values applies to each respective column. Default to True. allow\_mixed\_types \(bool\): Determines if columns are allowed to have mixed types \(disables type validation\). Defaults to False |

## Methods

### `add_column` <a id="add_column"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.28/wandb/data_types.py#L683-L722)

```text
add_column(
    name, data, optional=(False)
)
```

Add a column of data to the table.

Arguments name: \(str\) - the unique name of the column data: \(list \| np.array\) - a column of homogenous data optional: \(bool\) - if null-like values are permitted

### `add_computed_columns` <a id="add_computed_columns"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.28/wandb/data_types.py#L765-L785)

```text
add_computed_columns(
    fn
)
```

Adds one or more computed columns based on existing data

| Args |
| :--- |
|  fn \(function\): A function which accepts one or two paramters: ndx \(int\) and row \(dict\) which is expected to return a dict representing new columns for that row, keyed by the new column names. - `ndx` is an integer representing the index of the row. Only included if `include_ndx` is set to true - `row` is a dictionary keyed by existing columns |

### `add_data` <a id="add_data"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.28/wandb/data_types.py#L367-L397)

```text
add_data(
    *data
)
```

Add a row of data to the table. Argument length should match column length

### `add_row` <a id="add_row"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.28/wandb/data_types.py#L363-L365)

```text
add_row(
    *row
)
```

### `cast` <a id="cast"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.28/wandb/data_types.py#L262-L316)

```text
cast(
    col_name, dtype, optional=(False)
)
```

Casts a column to a specific type

| Arguments |  |
| :--- | :--- |
|  `col_name` |  \(str\) - name of the column to cast |
|  `dtype` |  \(class, wandb.wandb\_sdk.interface.\_dtypes.Type, any\) - the target dtype. Can be one of normal python class, internal WB type, or an example object \(eg. an instance of wandb.Image or wandb.Classes\) |
|  `optional` |  \(bool\) - if the column should allow Nones |

### `get_column` <a id="get_column"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.28/wandb/data_types.py#L724-L747)

```text
get_column(
    name, convert_to=None
)
```

Retrieves a column of data from the table

Arguments name: \(str\) - the name of the column convert\_to: \(str, optional\)

* "numpy": will convert the underlying data to numpy object

### `get_index` <a id="get_index"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.28/wandb/data_types.py#L749-L756)

```text
get_index()
```

Returns an array of row indexes which can be used in other tables to create links

### `index_ref` <a id="index_ref"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.28/wandb/data_types.py#L758-L763)

```text
index_ref(
    index
)
```

Get a reference to a particular row index in the table

### `iterrows` <a id="iterrows"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.28/wandb/data_types.py#L562-L575)

```text
iterrows()
```

Iterate over rows as \(ndx, row\)

## Yields

index : int The index of the row. Using this value in other WandB tables will automatically build a relationship between the tables row : List\[any\] The data of the row

### `set_fk` <a id="set_fk"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.28/wandb/data_types.py#L582-L586)

```text
set_fk(
    col_name, table, table_col
)
```

### `set_pk` <a id="set_pk"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.28/wandb/data_types.py#L577-L580)

```text
set_pk(
    col_name
)
```

| Class Variables |  |
| :--- | :--- |
|  MAX\_ARTIFACT\_ROWS |  `200000` |
|  MAX\_ROWS |  `10000` |

