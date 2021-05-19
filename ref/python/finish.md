# finish



[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/7e65d3b4f98261a70a14749af55a46433d6881c2/wandb/sdk/wandb_run.py#L2434-L2442)




Marks a run as finished, and finishes uploading all data.

<pre><code>finish(
    exit_code: int = None
) -> None</code></pre>




This is used when creating multiple runs in the same process.
We automatically call this method when your script exits.