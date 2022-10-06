# Log Tables

The simplest way to log a table is to log a [pandas dataframe](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.html) which will be automatically converted into a W\&B Table.

```python
wandb.log({"table": my_dataframe})
```

![Tables UI](<../../../.gitbook/assets/image (4) (1).png>)

There are many ways to use tables with rich media and interactive visualization. For more detail please refer to [tables-quickstart.md](../../data-vis/tables-quickstart.md "mention") and [table.md](../../../ref/python/data-types/table.md "mention").
