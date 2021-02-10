# login

<!-- Insert buttons and diff -->


[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_login.py#L22-L43)




Log in to W&B.

<pre><code>login(
    anonymous=None, key=None, relogin=None, host=None, force=None
)</code></pre>



<!-- Placeholder for "Used in" -->


<!-- Tabular view -->
<table>
<tr><th>Arguments</th></tr>

<tr>
<td>
<code>anonymous</code>
</td>
<td>
(string, optional) Can be "must", "allow", or "never".
If set to "must" we'll always login anonymously, if set to
"allow" we'll only create an anonymous user if the user
isn't already logged in.
</td>
</tr><tr>
<td>
<code>key</code>
</td>
<td>
(string, optional) authentication key.
</td>
</tr><tr>
<td>
<code>relogin</code>
</td>
<td>
(bool, optional) If true, will re-prompt for API key.
</td>
</tr><tr>
<td>
<code>host</code>
</td>
<td>
(string, optional) The host to connect to.
</td>
</tr>
</table>



<!-- Tabular view -->
<table>
<tr><th>Returns</th></tr>

<tr>
<td>
<code>bool</code>
</td>
<td>
if key is configured
</td>
</tr>
</table>



<!-- Tabular view -->
<table>
<tr><th>Raises</th></tr>
<tr>
<td>
UsageError - if api_key can not configured and no tty
</td>
</tr>

</table>

