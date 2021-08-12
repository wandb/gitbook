# finish



[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/v0.12.0/wandb/sdk/wandb_run.py#L2464-L2471)



Marks a run as finished, and finishes uploading all data.

```python
finish(
    exit_code: int = None
) -> None
```




This is used when creating multiple runs in the same process.
We automatically call this method when your script exits.