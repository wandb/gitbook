# Plotly



[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/v0.12.11/wandb/sdk/data_types.py#L2568-L2618)



Wandb class for plotly plots.

```python
Plotly(
    val: Union['plotly.Figure', 'matplotlib.artist.Artist']
)
```





| Arguments |  |
| :--- | :--- |
|  `val` |  matplotlib or plotly figure |



## Methods

<h3 id="make_plot_media"><code>make_plot_media</code></h3>

[View source](https://www.github.com/wandb/client/tree/v0.12.11/wandb/sdk/data_types.py#L2578-L2586)

```python
@classmethod
make_plot_media(
    val: Union['plotly.Figure', 'matplotlib.artist.Artist']
) -> Union[Image, 'Plotly']
```






