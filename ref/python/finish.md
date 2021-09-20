# finish



[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/v0.12.2/wandb/sdk/wandb_run.py#L2521-L2528)



Marks a run as finished, and finishes uploading all data.

```python
finish(
    exit_code: int = None
) -> None
```




This is used when creating multiple runs in the same process.
We automatically call this method when your script exits.