# wandb.apis.public.Runs

[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/v0.11.1/wandb/apis/public.py#L740-L839)

An iterable collection of runs associated with a project and optional filter.

```python
Runs(
    client, entity, project, filters={}, order=None, per_page=50
)
```

This is generally used indirectly via the `Api`.runs method

| Class Variables |  |
| :--- | :--- |
| `QUERY` |  |

