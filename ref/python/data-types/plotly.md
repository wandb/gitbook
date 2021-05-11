# Plotly



[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/94c226afc4925535e6301c9bc9b9ee36061d99d4/wandb/sdk/data_types.py#L2000-L2049)




Wandb class for plotly plots.

<pre><code>Plotly(
    val: Union['plotly.Figure', 'matplotlib.artist.Artist']
)</code></pre>





<!-- Tabular view -->
<table>
<tr><th>Arguments</th></tr>

<tr>
<td>
<code>val</code>
</td>
<td>
matplotlib or plotly figure
</td>
</tr>
</table>



## Methods

<h3 id="make_plot_media"><code>make_plot_media</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/94c226afc4925535e6301c9bc9b9ee36061d99d4/wandb/sdk/data_types.py#L2010-L2018">View source</a>

<pre><code>@classmethod</code>
<code>make_plot_media(
    val: Union['plotly.Figure', 'matplotlib.artist.Artist']
) -> Union[Image, 'Plotly']</code></pre>






