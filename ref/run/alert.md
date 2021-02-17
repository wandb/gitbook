# alert

<!-- Insert buttons and diff -->


[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_run.py#L2052-L2088)




Launch an alert with the given title and text.

<pre><code>alert(
    title: str,
    text: str,
    level: Union[str, None] = None,
    wait_duration: Union[int, float, timedelta, None] = None
) -> None</code></pre>



<!-- Placeholder for "Used in" -->


<!-- Tabular view -->
<table>
<tr><th>Arguments</th></tr>

<tr>
<td>
<code>title</code>
</td>
<td>
(str) The title of the alert, must be less than 64 characters long.
</td>
</tr><tr>
<td>
<code>text</code>
</td>
<td>
(str) The text body of the alert.
</td>
</tr><tr>
<td>
<code>level</code>
</td>
<td>
(str or wandb.AlertLevel, optional) The alert level to use, either: `INFO`, `WARN`, or `ERROR`.
</td>
</tr><tr>
<td>
<code>wait_duration</code>
</td>
<td>
(int, float, or timedelta, optional) The time to wait (in seconds) before sending another
alert with this title.
</td>
</tr>
</table>

