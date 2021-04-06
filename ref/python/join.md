# join



[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/18a721ba0f880a64aea802ebd3e2862f394610f4/wandb/sdk/wandb_run.py#L2387-L2395)




Marks a run as finished, and finishes uploading all data.

<pre><code>join(
    exit_code: int = None
) -> None</code></pre>




This is used when creating multiple runs in the same process.
We automatically call this method when your script exits.