# Settings

[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_settings.py#L193-L996)

Settings Constructor

```text
settings(
    base_url: str = None,
    api_key: str = None,
    anonymous=None,
    mode: str = None,
    start_method: str = None,
    entity: str = None,
    project: str = None,
    run_group: str = None,
    run_job_type: str = None,
    run_id: str = None,
    run_name: str = None,
    run_notes: str = None,
    resume: str = None,
    magic: Union[Dict, str, bool] = False,
    run_tags: Sequence = None,
    sweep_id=None,
    allow_val_change: bool = None,
    force: bool = None,
    relogin: bool = None,
    problem='fatal',
    system_sample_seconds=2,
    system_samples=15,
    heartbeat_seconds=30,
    config_paths=None,
    sweep_param_path=None,
    _config_dict=None,
    root_dir=None,
    settings_system_spec='~/.config/wandb/settings',
    settings_workspace_spec='{wandb_dir}/settings',
    sync_dir_spec='{wandb_dir}/{run_mode}-{timespec}-{run_id}',
    sync_file_spec='run-{run_id}.wandb',
    sync_symlink_latest_spec='{wandb_dir}/latest-run',
    log_dir_spec='{wandb_dir}/{run_mode}-{timespec}-{run_id}/logs',
    log_user_spec='debug.log',
    log_internal_spec='debug-internal.log',
    log_symlink_user_spec='{wandb_dir}/debug.log',
    log_symlink_internal_spec='{wandb_dir}/debug-internal.log',
    resume_fname_spec='{wandb_dir}/wandb-resume.json',
    files_dir_spec='{wandb_dir}/{run_mode}-{timespec}-{run_id}/files',
    symlink=None,
    program=None,
    notebook_name=None,
    disable_code=None,
    ignore_globs=None,
    save_code=None,
    program_relpath=None,
    git_remote=None,
    dev_prod=None,
    host=None,
    username=None,
    email=None,
    docker=None,
    sagemaker_disable: Optional[bool] = None,
    _start_time=None,
    _start_datetime=None,
    _cli_only_mode=None,
    _disable_viewer=None,
    console=None,
    disabled=None,
    reinit=None,
    _save_requirements=True,
    show_colors=None,
    show_emoji=None,
    silent=None,
    show_info=None,
    show_warnings=None,
    show_errors=None,
    summary_errors=None,
    summary_warnings=None,
    _internal_queue_timeout=2,
    _internal_check_process=8,
    _disable_meta=None,
    _disable_stats=None,
    _jupyter_path=None,
    _jupyter_name=None,
    _jupyter_root=None,
    _executable=None,
    _cuda=None,
    _args=None,
    _os=None,
    _python=None,
    _kaggle=None,
    _except_exit=None
)
```

| Arguments |  |
| :--- | :--- |
|  `entity` |  personal user or team to use for Run. |
|  `project` |  project name for the Run. |

| Raises |  |
| :--- | :--- |
|  `Exception` |  if problem. |

| Attributes |  |
| :--- | :--- |
|  `files_dir` |  |
|  `log_internal` |  |
|  `log_symlink_internal` |  |
|  `log_symlink_user` |  |
|  `log_user` |  |
|  `resume_fname` |  |
|  `settings_system` |  |
|  `settings_workspace` |  |
|  `sync_file` |  |
|  `sync_symlink_latest` |  |
|  `wandb_dir` |  |

## Methods

### `duplicate` <a id="duplicate"></a>

[View source](https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_settings.py#L697-L698)

```text
duplicate()
```

### `freeze` <a id="freeze"></a>

[View source](https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_settings.py#L890-L892)

```text
freeze()
```

### `is_frozen` <a id="is_frozen"></a>

[View source](https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_settings.py#L894-L895)

```text
is_frozen() -> bool
```

### `keys` <a id="keys"></a>

[View source](https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_settings.py#L881-L882)

```text
keys()
```

### `load` <a id="load"></a>

[View source](https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_settings.py#L847-L848)

```text
load(
    fname
)
```

### `save` <a id="save"></a>

[View source](https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_settings.py#L844-L845)

```text
save(
    fname
)
```

### `setdefaults` <a id="setdefaults"></a>

[View source](https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_settings.py#L836-L842)

```text
setdefaults(
    _Settings__d=None
)
```

### `update` <a id="update"></a>

[View source](https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_settings.py#L744-L745)

```text
update(
    _Settings__d=None, **kwargs
)
```

### `__getitem__` <a id="__getitem__"></a>

[View source](https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_settings.py#L884-L888)

```text
__getitem__(
    k
)
```

| Class Variables |  |
| :--- | :--- |
|  base\_url |  \`None\` |
|  console |  \`'auto'\` |
|  disabled |  \`False\` |
|  email |  \`None\` |
|  entity |  \`None\` |
|  files\_dir\_spec |  \`None\` |
|  log\_dir\_spec |  \`None\` |
|  log\_internal\_spec |  \`None\` |
|  log\_symlink\_internal\_spec |  \`None\` |
|  log\_symlink\_user\_spec |  \`None\` |
|  log\_user\_spec |  \`None\` |
|  mode |  \`'online'\` |
|  program\_relpath |  \`None\` |
|  project |  \`None\` |
|  resume\_fname\_spec |  \`None\` |
|  root\_dir |  \`None\` |
|  run\_group |  \`None\` |
|  run\_job\_type |  \`None\` |
|  run\_name |  \`None\` |
|  run\_notes |  \`None\` |
|  run\_tags |  \`None\` |
|  sagemaker\_disable |  \`None\` |
|  save\_code |  \`None\` |
|  settings\_system\_spec |  \`None\` |
|  settings\_workspace\_spec |  \`None\` |
|  show\_errors |  \`True\` |
|  show\_info |  \`True\` |
|  show\_warnings |  \`True\` |
|  silent |  \`False\` |
|  start\_method |  \`'spawn'\` |
|  sync\_dir\_spec |  \`None\` |
|  sync\_file\_spec |  \`None\` |
|  sync\_symlink\_latest\_spec |  \`None\` |

