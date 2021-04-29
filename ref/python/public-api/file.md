# wandb.apis.public.File

[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/v0.10.28/wandb/apis/public.py#L1655-L1758)

File is a class associated with a file saved by wandb.

```text
File(
    client, attrs
)
```

| Attributes |
| :--- |


## Methods

### `delete` <a id="delete"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.28/wandb/apis/public.py#L1738-L1751)

```text
delete()
```

### `download` <a id="download"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.28/wandb/apis/public.py#L1715-L1736)

```text
download(
    root='.', replace=(False)
)
```

Downloads a file previously saved by a run from the wandb server.

| Arguments |
| :--- |
|  replace \(boolean\): If `True`, download will overwrite a local file if it exists. Defaults to `False`. root \(str\): Local directory to save the file. Defaults to ".". |

| Raises |
| :--- |
|  `ValueError` if file already exists and replace=False |

