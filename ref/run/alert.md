# alert

<!-- Insert buttons and diff -->


[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_run.py#L2049-L2078)




Launch an alert with the given title and text.

<pre><code>alert(
    title, text, level=None, wait_duration=None
)</code></pre>



<!-- Placeholder for "Used in" -->


<!-- Tabular view -->
<table>
<tr><th>Arguments</th></tr>
<tr>
<td>
title (str): The title of the alert, must be less than 64 characters long
text (str): The text body of the alert
level (str or wandb.AlertLevel, optional): The alert level to use, either: `INFO`, `WARN`, or `ERROR`
wait_duration (int, float, or timedelta, optional): The time to wait (in seconds) before sending another alert
with this title
</td>
</tr>

</table>

