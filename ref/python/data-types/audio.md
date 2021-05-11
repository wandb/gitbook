# Audio



[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/v0.10.30/wandb/data_types.py#L877-L1021)




Wandb class for audio clips.

<pre><code>Audio(
    data_or_path, sample_rate=None, caption=None
)</code></pre>





<!-- Tabular view -->
<table>
<tr><th>Arguments</th></tr>

<tr>
<td>
<code>data_or_path</code>
</td>
<td>
(string or numpy array) A path to an audio file
or a numpy array of audio data.
</td>
</tr><tr>
<td>
<code>sample_rate</code>
</td>
<td>
(int) Sample rate, required when passing in raw
numpy array of audio data.
</td>
</tr><tr>
<td>
<code>caption</code>
</td>
<td>
(string) Caption to display with audio.
</td>
</tr>
</table>



## Methods

<h3 id="durations"><code>durations</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.30/wandb/data_types.py#L979-L981">View source</a>

<pre><code>@classmethod</code>
<code>durations(
    audio_list
)</code></pre>




<h3 id="path_is_reference"><code>path_is_reference</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.30/wandb/data_types.py#L922-L924">View source</a>

<pre><code>@classmethod</code>
<code>path_is_reference(
    path
)</code></pre>




<h3 id="resolve_ref"><code>resolve_ref</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.30/wandb/data_types.py#L995-L1007">View source</a>

<pre><code>resolve_ref()</code></pre>




<h3 id="sample_rates"><code>sample_rates</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.30/wandb/data_types.py#L983-L985">View source</a>

<pre><code>@classmethod</code>
<code>sample_rates(
    audio_list
)</code></pre>






