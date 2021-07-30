# wandb.apis.public.File

[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/v0.11.1/wandb/apis/public.py#L1628-L1731)

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

[View source](https://www.github.com/wandb/client/tree/v0.11.1/wandb/apis/public.py#L1711-L1724)

```python
delete()
```

### `download` <a id="download"></a>

[View source](https://www.github.com/wandb/client/tree/v0.11.1/wandb/apis/public.py#L1688-L1709)

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

