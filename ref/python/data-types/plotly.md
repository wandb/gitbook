# wandb.data\_types.Plotly

[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/c129c32964aca6a8509d98a0cc3c9bc46f2d8a4c/wandb/sdk/data_types.py#L1995-L2042)

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

[View source](https://www.github.com/wandb/client/tree/c129c32964aca6a8509d98a0cc3c9bc46f2d8a4c/wandb/sdk/data_types.py#L2003-L2011)

```text
@classmethod
make_plot_media(
    val: Union['plotly.Figure', 'matplotlib.artist.Artist']
) -> Union[Image, 'Plotly']
```

| Class Variables |  |
| :--- | :--- |
|  artifact\_type |  `None` |

