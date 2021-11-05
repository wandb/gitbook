# wandb.save

[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/v0.12.6/wandb/sdk/wandb\_run.py#L1233-L1322)

Ensure all files matching `glob_str` are synced to wandb with the policy specified. `wandb.save()` will symlink any file not in `wandb.run.dir` into that directory and the mark it with one of three policies for upload.

`wandb.save()`will be deprecated in future releases once all integrations and code flows that require it have been moved over to using [artifacts](https://docs.wandb.ai/ref/python/artifact). For new code, please use artifacts.

```python
save(
    glob_str: Optional[str] = None,
    base_path: Optional[str] = None,
    policy: str = "live"
) -> Union[bool, List[str]]
```

| Arguments   |                                                                                                                                                                                          |
| ----------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `glob_str`  | (string) a relative or absolute path to a unix glob or regular path. If this isn't specified the method is a noop.                                                                       |
| `base_path` | (string) the base path to run the glob relative to                                                                                                                                       |
| `policy`    | (string) on of `live`, `now`, or `end` - live: upload the file as it changes, overwriting the previous version - now: upload the file once now - end: only upload file when the run ends |
