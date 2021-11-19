# Plotly



[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/v0.12.7/wandb/sdk/data_types.py#L2522-L2571)



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

[View source](https://www.github.com/wandb/client/tree/v0.12.7/wandb/sdk/data_types.py#L2532-L2540)

```python
@classmethod
make_plot_media(
    val: Union['plotly.Figure', 'matplotlib.artist.Artist']
) -> Union[Image, 'Plotly']
```






