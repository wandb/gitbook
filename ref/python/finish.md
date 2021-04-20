# wandb.finish

[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/c129c32964aca6a8509d98a0cc3c9bc46f2d8a4c/wandb/sdk/wandb_run.py#L2387-L2395)

Marks a run as finished, and finishes uploading all data.

```text
finish(
    exit_code: int = None
) -> None
```

This is used when creating multiple runs in the same process. We automatically call this method when your script exits.

