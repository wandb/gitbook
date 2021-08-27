# wandb.apis.public.File

[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/v0.12.1/wandb/apis/public.py#L1725-L1828)

File is a class associated with a file saved by wandb.

```python
File(
    client, attrs
)
```

| Attributes |  |
| :--- | :--- |


## Methods

### `delete` <a id="delete"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.1/wandb/apis/public.py#L1808-L1821)

```python
delete()
```

### `download` <a id="download"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.1/wandb/apis/public.py#L1785-L1806)

```python
download(
    root=".", replace=(False)
)
```

Downloads a file previously saved by a run from the wandb server.

| Arguments |  |
| :--- | :--- |
| replace \(boolean\): If `True`, download will overwrite a local file if it exists. Defaults to `False`. root \(str\): Local directory to save the file. Defaults to ".". |  |

| Raises |  |
| :--- | :--- |
| `ValueError` if file already exists and replace=False |  |

