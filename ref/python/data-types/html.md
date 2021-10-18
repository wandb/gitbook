# wandb.data_types.Html

[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/v0.12.2/wandb/sdk/data_types.py#L947-L1037)

Wandb class for arbitrary html

```python
Html(
    data: Union[str, 'TextIO'],
    inject: bool = (True)
) -> None
```

| Arguments |                                                                                                      |
| --------- | ---------------------------------------------------------------------------------------------------- |
| `data`    | (string or io object) HTML to display in wandb                                                       |
| `inject`  | (boolean) Add a stylesheet to the HTML object. If set to False the HTML will pass through unchanged. |

## Methods

### `inject_head` <a href="inject_head" id="inject_head"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.2/wandb/sdk/data_types.py#L989-L1004)

```python
inject_head() -> None
```
