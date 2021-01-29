# Video

<!-- Insert buttons and diff -->


[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/master/wandb/data_types.py#L1212-L1380)




Wandb representation of video.

<pre class="devsite-click-to-copy prettyprint lang-py tfo-signature-link">
<code>datatypes.Video(
    data_or_path, caption=None, fps=4, format=None
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
(numpy array, string, io)
Video can be initialized with a path to a file or an io object.
The format must be "gif", "mp4", "webm" or "ogg".
The format must be specified with the format argument.
Video can be initialized with a numpy tensor.
The numpy tensor must be either 4 dimensional or 5 dimensional.
Channels should be (time, channel, height, width) or
(batch, time, channel, height width)
</td>
</tr><tr>
<td>
<code>caption</code>
</td>
<td>
(string) caption associated with the video for display
</td>
</tr><tr>
<td>
<code>fps</code>
</td>
<td>
(int) frames per second for video. Default is 4.
</td>
</tr><tr>
<td>
<code>format</code>
</td>
<td>
(string) format of video, necessary if initializing with path or io object.
</td>
</tr>
</table>



## Methods

<h3 id="encode"><code>encode</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/data_types.py#L1273-L1307">View source</a>

<pre class="devsite-click-to-copy prettyprint lang-py tfo-signature-link">
<code>encode()
</code></pre>








<!-- Tabular view -->
<table>
<tr><th>Class Variables</th></tr>

<tr>
<td>
EXTS<a id="EXTS"></a>
</td>
<td>

</td>
</tr><tr>
<td>
artifact_type<a id="artifact_type"></a>
</td>
<td>
`'video-file'`
</td>
</tr>
</table>

