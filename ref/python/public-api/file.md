# wandb.apis.public.File

[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/c129c32964aca6a8509d98a0cc3c9bc46f2d8a4c/wandb/apis/public.py#L1650-L1753)

File is a class associated with a file saved by wandb.

```text
File(
    client, attrs
)
```

| Attributes |  |
| :--- | :--- |
|  `digest` |  |
|  `direct_url` |  |
|  `id` |  |
|  `md5` |  |
|  `mimetype` |  |
|  `name` |  |
|  `size` |  |
|  `updated_at` |  |
|  `url` |  |

## Methods

### `delete` <a id="delete"></a>

[View source](https://www.github.com/wandb/client/tree/c129c32964aca6a8509d98a0cc3c9bc46f2d8a4c/wandb/apis/public.py#L1733-L1746)

```text
delete()
```

### `download` <a id="download"></a>

[View source](https://www.github.com/wandb/client/tree/c129c32964aca6a8509d98a0cc3c9bc46f2d8a4c/wandb/apis/public.py#L1710-L1731)

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

