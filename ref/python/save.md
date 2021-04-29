# save



[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/v0.10.28/wandb/sdk/wandb_run.py#L1089-L1178)




Ensure all files matching *glob_str* are synced to wandb with the policy specified.

<pre><code>save(
    glob_str: Optional[str] = None,
    base_path: Optional[str] = None,
    policy: str = &#x27;live&#x27;
) -> Union[bool, List[str]]</code></pre>





<!-- Tabular view -->
<table>
<tr><th>Arguments</th></tr>

<tr>
<td>
<code>glob_str</code>
</td>
<td>
(string) a relative or absolute path to a unix glob or regular
path.  If this isn't specified the method is a noop.
</td>
</tr><tr>
<td>
<code>base_path</code>
</td>
<td>
(string) the base path to run the glob relative to
</td>
</tr><tr>
<td>
<code>policy</code>
</td>
<td>
(string) on of <code>live</code>, <code>now</code>, or <code>end</code>
- live: upload the file as it changes, overwriting the previous version
- now: upload the file once now
- end: only upload file when the run ends
</td>
</tr>
</table>

