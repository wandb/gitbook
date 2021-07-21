# wandb.data\_types.Plotly

[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/v0.11.0/wandb/sdk/data_types.py#L2227-L2276)

Wandb class for plotly plots.

```python
Plotly(
    val: Union['plotly.Figure', 'matplotlib.artist.Artist']
)
```

| Arguments |  |
| :--- | :--- |
| `val` | matplotlib or plotly figure |

## Methods

### `make_plot_media` <a id="make_plot_media"></a>

[View source](https://www.github.com/wandb/client/tree/v0.11.0/wandb/sdk/data_types.py#L2237-L2245)

```python
@classmethod
make_plot_media(
    val: Union['plotly.Figure', 'matplotlib.artist.Artist']
) -> Union[Image, 'Plotly']
```

