# wandb.data\_types.Html

[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/18a721ba0f880a64aea802ebd3e2862f394610f4/wandb/sdk/data_types.py#L873-L963)

Wandb class for arbitrary html

```text
Html(
    data: Union[str, 'TextIO'],
    inject: bool = (True)
) -> None
```

| Arguments |  |
| :--- | :--- |
|  `data` |  \(string or io object\) HTML to display in wandb |
|  `inject` |  \(boolean\) Add a stylesheet to the HTML object. If set to False the HTML will pass through unchanged. |

## Methods

### `inject_head` <a id="inject_head"></a>

[View source](https://www.github.com/wandb/client/tree/18a721ba0f880a64aea802ebd3e2862f394610f4/wandb/sdk/data_types.py#L915-L930)

```text
inject_head() -> None
```

| Class Variables |  |
| :--- | :--- |
|  artifact\_type |  \`'html-file'\` |

