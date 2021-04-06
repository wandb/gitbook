# wandb.data\_types.Plotly

[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/18a721ba0f880a64aea802ebd3e2862f394610f4/wandb/sdk/data_types.py#L1995-L2042)

Wandb class for plotly plots.

```text
Plotly(
    val: Union['plotly.Figure', 'matplotlib.artist.Artist']
)
```

| Arguments |  |
| :--- | :--- |
|  `val` |  matplotlib or plotly figure |

## Methods

### `make_plot_media` <a id="make_plot_media"></a>

[View source](https://www.github.com/wandb/client/tree/18a721ba0f880a64aea802ebd3e2862f394610f4/wandb/sdk/data_types.py#L2003-L2011)

```text
@classmethod
make_plot_media(
    val: Union['plotly.Figure', 'matplotlib.artist.Artist']
) -> Union[Image, 'Plotly']
```

| Class Variables |  |
| :--- | :--- |
|  artifact\_type |  `None` |

