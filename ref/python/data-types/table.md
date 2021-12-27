# wandb.data\_types.Table

[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/v0.12.9/wandb/data\_types.py#L117-L853)

The Table class is used to display and analyze tabular data.

```python
Table(
    columns=None, data=None, rows=None, dataframe=None, dtype=None, optional=(True),
    allow_mixed_types=(False)
)
```

Unlike traditional spreadsheets, Tables support numerous types of data: scalar values, strings, numpy arrays, and most subclasses of `wandb.data_types.Media`. This means you can embed `Images`, `Video`, `Audio`, and other sorts of rich, annotated media directly in Tables, alongside other traditional scalar values.

This class is the primary class used to generate the [Table Visualizer in the UI](https://docs.wandb.ai/guides/data-vis/table)

Tables can be constructed with initial data using the `data` or `dataframe` parameters:

```python
import pandas as pd
import wandb

data = {"users": ["geoff", "juergen", "ada"],
        "feature_01": [1, 117, 42]}
df = pd.DataFrame(data)

tbl = wandb.Table(data=df)
assert all(tbl.get_column("users") == df["users"])
assert all(tbl.get_column("feature_01") == df["feature_01"])
```

Additionally, users can add data to Tables incrementally by using the `add_data`, `add_column`, and `add_computed_column` functions for adding rows, columns, and columns computed from data in other columns, respectively:

```python
import wandb

tbl = wandb.Table(columns=["user"])

users = ["geoff", "juergen", "ada"]

[tbl.add_data(user) for user in users]
assert tbl.get_column("user") == users

def get_user_name_length(index, row): return {"feature_01": len(row["user"])}
tbl.add_computed_columns(get_user_name_length)
assert tbl.get_column("feature_01") == [5, 7, 3]
```

Tables can be logged directly to runs using `run.log({"my_table": table})` or added to artifacts using `artifact.add(table, "my_table")`:

```python
import numpy as np
import wandb

wandb.init()

tbl = wandb.Table(columns=["image", "label"])

images = np.random.randint(0, 255, [2, 100, 100, 3], dtype=np.uint8)
labels = ["panda", "gibbon"]
[tbl.add_data(wandb.Image(image), label) for image, label in zip(images, labels)]

wandb.log({"classifier_out": tbl})
```

Tables added directly to runs as above will produce a corresponding Table Visualizer in the Workspace which can be used for further analysis and exporting to reports.

Tables added to artifacts can be viewed in the Artifact Tab and will render an equivalent Table Visualizer directly in the artifact browser.

Tables expect each value for a column to be of the same type. By default, a column supports optional values, but not mixed values. If you absolutely need to mix types, you can enable the `allow_mixed_types` flag which will disable type checking on the data. This will result in some table analytics features being disabled due to lack of consistent typing.

| Arguments           |                                                                                                                                                                                                                                                                                                                                                                                                      |
| ------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `columns`           | (List\[str]) Names of the columns in the table. Defaults to \["Input", "Output", "Expected"].                                                                                                                                                                                                                                                                                                        |
| `data`              | (List\[List\[any]]) 2D row-oriented array of values.                                                                                                                                                                                                                                                                                                                                                 |
| `dataframe`         | (pandas.DataFrame) DataFrame object used to create the table. When set, `data` and `columns` arguments are ignored.                                                                                                                                                                                                                                                                                  |
| `optional`          | (Union\[bool,List\[bool]]) Determines if `None` values are allowed. Default to True - If a singular bool value, then the optionality is enforced for all columns specified at construction time - If a list of bool values, then the optionality is applied to each column - should be the same length as `columns` applies to all columns. A list of bool values applies to each respective column. |
| `allow_mixed_types` | (bool) Determines if columns are allowed to have mixed types (disables type validation). Defaults to False                                                                                                                                                                                                                                                                                           |

## Methods

### `add_column` <a href="#add_column" id="add_column"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.9/wandb/data\_types.py#L749-L788)

```python
add_column(
    name, data, optional=(False)
)
```

Add a column of data to the table.

Arguments name: (str) - the unique name of the column data: (list | np.array) - a column of homogenous data optional: (bool) - if null-like values are permitted

### `add_computed_columns` <a href="#add_computed_columns" id="add_computed_columns"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.9/wandb/data\_types.py#L831-L853)

```python
add_computed_columns(
    fn
)
```

Adds one or more computed columns based on existing data

| Args |                                                                                                                                                                                                                                                                                                                                            |
| ---- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `fn` | A function which accepts one or two parameters, ndx (int) and row (dict), which is expected to return a dict representing new columns for that row, keyed by the new column names. `ndx` is an integer representing the index of the row. Only included if `include_ndx` is set to `True`. `row` is a dictionary keyed by existing columns |

### `add_data` <a href="#add_data" id="add_data"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.9/wandb/data\_types.py#L415-L445)

```python
add_data(
    *data
)
```

Add a row of data to the table. Argument length should match column length

### `add_row` <a href="#add_row" id="add_row"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.9/wandb/data\_types.py#L411-L413)

```python
add_row(
    *row
)
```

### `cast` <a href="#cast" id="cast"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.9/wandb/data\_types.py#L310-L364)

```python
cast(
    col_name, dtype, optional=(False)
)
```

Casts a column to a specific type

| Arguments  |                                                                                                                                                                                                        |
| ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `col_name` | (str) - name of the column to cast                                                                                                                                                                     |
| `dtype`    | (class, wandb.wandb\_sdk.interface.\_dtypes.Type, any) - the target dtype. Can be one of normal python class, internal WB type, or an example object (eg. an instance of wandb.Image or wandb.Classes) |
| `optional` | (bool) - if the column should allow Nones                                                                                                                                                              |

### `get_column` <a href="#get_column" id="get_column"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.9/wandb/data\_types.py#L790-L813)

```python
get_column(
    name, convert_to=None
)
```

Retrieves a column of data from the table

Arguments name: (str) - the name of the column convert\_to: (str, optional) - "numpy": will convert the underlying data to numpy object

### `get_index` <a href="#get_index" id="get_index"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.9/wandb/data\_types.py#L815-L822)

```python
get_index()
```

Returns an array of row indexes which can be used in other tables to create links

### `index_ref` <a href="#index_ref" id="index_ref"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.9/wandb/data\_types.py#L824-L829)

```python
index_ref(
    index
)
```

Get a reference to a particular row index in the table

### `iterrows` <a href="#iterrows" id="iterrows"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.9/wandb/data\_types.py#L628-L641)

```python
iterrows()
```

## Iterate over rows as (ndx, row) Yields

index : int The index of the row. Using this value in other WandB tables will automatically build a relationship between the tables row : List\[any] The data of the row

### `set_fk` <a href="#set_fk" id="set_fk"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.9/wandb/data\_types.py#L648-L652)

```python
set_fk(
    col_name, table, table_col
)
```

### `set_pk` <a href="#set_pk" id="set_pk"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.9/wandb/data\_types.py#L643-L646)

```python
set_pk(
    col_name
)
```

| Class Variables     |          |
| ------------------- | -------- |
| `MAX_ARTIFACT_ROWS` | `200000` |
| `MAX_ROWS`          | `10000`  |
