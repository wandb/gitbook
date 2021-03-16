# Table

[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/master/wandb/data_types.py#L481-L770)

This is a table designed to display sets of records.

```text
Table(
    columns=None, data=None, rows=None, dataframe=None, dtype=None, optional=True,
    allow_mixed_types=False
)
```

| Arguments |  |
| :--- | :--- |
|  `columns` |  \(\[str\]\) Names of the columns in the table. Defaults to \["Input", "Output", "Expected"\]. |
|  `data` |  \(array\) 2D Array of values that will be displayed as strings. |
|  `dataframe` |  \(pandas.DataFrame\) DataFrame object used to create the table. When set, the other arguments are ignored. optional \(Union\[bool,List\[bool\]\]\): If None values are allowed. Singular bool applies to all columns. A list of bool values applies to each respective column. Default to True. allow\_mixed\_types \(bool\): Determines if columns are allowed to have mixed types \(disables type validation\). Defaults to False |

## Methods

### `add_data` <a id="add_data"></a>

[View source](https://www.github.com/wandb/client/tree/master/wandb/data_types.py#L636-L645)

```text
add_data(
    *data
)
```

Add a row of data to the table. Argument length should match column length

### `add_row` <a id="add_row"></a>

[View source](https://www.github.com/wandb/client/tree/master/wandb/data_types.py#L632-L634)

```text
add_row(
    *row
)
```

### `cast` <a id="cast"></a>

[View source](https://www.github.com/wandb/client/tree/master/wandb/data_types.py#L594-L611)

```text
cast(
    col_name, dtype, optional=False
)
```

### `iterrows` <a id="iterrows"></a>

[View source](https://www.github.com/wandb/client/tree/master/wandb/data_types.py#L760-L770)

```text
iterrows()
```

Iterate over rows as \(ndx, row\)

## Yields

index : int The index of the row. row : List\[any\] The data of the row

| Class Variables |  |
| :--- | :--- |
|  MAX\_ARTIFACT\_ROWS |  \`200000\` |
|  MAX\_ROWS |  \`10000\` |
|  artifact\_type |  \`'table'\` |

