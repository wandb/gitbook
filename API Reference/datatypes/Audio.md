# Audio

<!-- Insert buttons and diff -->


[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/master/wandb/data_types.py#L752-L866)




Wandb class for audio clips.

<pre class="devsite-click-to-copy prettyprint lang-py tfo-signature-link">
<code>datatypes.Audio(
    data_or_path, sample_rate=None, caption=None
)
</code></pre>



<!-- Placeholder for "Used in" -->


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

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/data_types.py#L842-L844">View source</a>

<pre class="devsite-click-to-copy prettyprint lang-py tfo-signature-link">
<code>@classmethod</code>
<code>durations(
    audio_list
)
</code></pre>




<h3 id="sample_rates"><code>sample_rates</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/data_types.py#L846-L848">View source</a>

<pre class="devsite-click-to-copy prettyprint lang-py tfo-signature-link">
<code>@classmethod</code>
<code>sample_rates(
    audio_list
)
</code></pre>








<!-- Tabular view -->
<table>
<tr><th>Class Variables</th></tr>

<tr>
<td>
artifact_type<a id="artifact_type"></a>
</td>
<td>
`'audio-file'`
</td>
</tr>
</table>

