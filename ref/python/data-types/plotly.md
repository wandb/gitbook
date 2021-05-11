# wandb.data\_types.Plotly

[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/94c226afc4925535e6301c9bc9b9ee36061d99d4/wandb/sdk/data_types.py#L2000-L2049)

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

[View source](https://www.github.com/wandb/client/tree/94c226afc4925535e6301c9bc9b9ee36061d99d4/wandb/sdk/data_types.py#L2010-L2018)

```text
@classmethod
make_plot_media(
    val: Union['plotly.Figure', 'matplotlib.artist.Artist']
) -> Union[Image, 'Plotly']
```

