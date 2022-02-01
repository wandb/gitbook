# finish



[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/latest/wandb/sdk/wandb_run.py#L2837-L2848)



Marks a run as finished, and finishes uploading all data.

```python
finish(
    exit_code: int = None,
    quiet: bool = None
) -> None
```




This is used when creating multiple runs in the same process.
We automatically call this method when your script exits.

| Arguments |  |
| :--- | :--- |
|  `exit_code` |  Set to something other than 0 to mark a run as failed |
|  `quiet` |  Set to true to minimize log output |

