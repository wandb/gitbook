# wandb.finish

[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/94c226afc4925535e6301c9bc9b9ee36061d99d4/wandb/sdk/wandb_run.py#L2428-L2436)

Marks a run as finished, and finishes uploading all data.

```text
finish(
    exit_code: int = None
) -> None
```

This is used when creating multiple runs in the same process. We automatically call this method when your script exits.

