# finish



[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/v0.12.5/wandb/sdk/wandb_run.py#L2706-L2717)



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
|  exit_code (int): set to something other than 0 to mark a run as failed quite (bool): set to true to minimize log output |

