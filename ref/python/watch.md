# watch



[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/v0.10.28/wandb/sdk/wandb_watch.py#L19-L98)




Hooks into the torch model to collect gradients and the topology.  Should be extended

<pre><code>watch(
    models, criterion=None, log=&#x27;gradients&#x27;, log_freq=1000, idx=None
)</code></pre>



to accept arbitrary ML models.

<!-- Tabular view -->
<table>
<tr><th>Args</th></tr>

<tr>
<td>
<code>models</code>
</td>
<td>
(torch.Module) The model to hook, can be a tuple
</td>
</tr><tr>
<td>
<code>criterion</code>
</td>
<td>
(torch.F) An optional loss value being optimized
</td>
</tr><tr>
<td>
<code>log</code>
</td>
<td>
(str) One of "gradients", "parameters", "all", or None
</td>
</tr><tr>
<td>
<code>log_freq</code>
</td>
<td>
(int) log gradients and parameters every N batches
</td>
</tr><tr>
<td>
<code>idx</code>
</td>
<td>
(int) an index to be used when calling wandb.watch on multiple models
</td>
</tr>
</table>



<!-- Tabular view -->
<table>
<tr><th>Returns</th></tr>
<tr>
<td>
<code>wandb.Graph</code> The graph object that will populate after the first backward pass
</td>
</tr>

</table>

