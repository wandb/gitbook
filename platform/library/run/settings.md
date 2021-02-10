# settings

<!-- Insert buttons and diff -->


[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_settings.py#L193-L996)




Settings Constructor

<pre><code>settings(
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
    problem=&#x27;fatal&#x27;,
    system_sample_seconds=2,
    system_samples=15,
    heartbeat_seconds=30,
    config_paths=None,
    sweep_param_path=None,
    _config_dict=None,
    root_dir=None,
    settings_system_spec=&#x27;~/.config/wandb/settings&#x27;,
    settings_workspace_spec=&#x27;{wandb_dir}/settings&#x27;,
    sync_dir_spec=&#x27;{wandb_dir}/{run_mode}-{timespec}-{run_id}&#x27;,
    sync_file_spec=&#x27;run-{run_id}.wandb&#x27;,
    sync_symlink_latest_spec=&#x27;{wandb_dir}/latest-run&#x27;,
    log_dir_spec=&#x27;{wandb_dir}/{run_mode}-{timespec}-{run_id}/logs&#x27;,
    log_user_spec=&#x27;debug.log&#x27;,
    log_internal_spec=&#x27;debug-internal.log&#x27;,
    log_symlink_user_spec=&#x27;{wandb_dir}/debug.log&#x27;,
    log_symlink_internal_spec=&#x27;{wandb_dir}/debug-internal.log&#x27;,
    resume_fname_spec=&#x27;{wandb_dir}/wandb-resume.json&#x27;,
    files_dir_spec=&#x27;{wandb_dir}/{run_mode}-{timespec}-{run_id}/files&#x27;,
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
)</code></pre>



<!-- Placeholder for "Used in" -->


<!-- Tabular view -->
<table>
<tr><th>Arguments</th></tr>

<tr>
<td>
<code>entity</code>
</td>
<td>
personal user or team to use for Run.
</td>
</tr><tr>
<td>
<code>project</code>
</td>
<td>
project name for the Run.
</td>
</tr>
</table>



<!-- Tabular view -->
<table>
<tr><th>Raises</th></tr>

<tr>
<td>
<code>Exception</code>
</td>
<td>
if problem.
</td>
</tr>
</table>





<!-- Tabular view -->
<table>
<tr><th>Attributes</th></tr>

<tr>
<td>
<code>files_dir</code>
</td>
<td>

</td>
</tr><tr>
<td>
<code>log_internal</code>
</td>
<td>

</td>
</tr><tr>
<td>
<code>log_symlink_internal</code>
</td>
<td>

</td>
</tr><tr>
<td>
<code>log_symlink_user</code>
</td>
<td>

</td>
</tr><tr>
<td>
<code>log_user</code>
</td>
<td>

</td>
</tr><tr>
<td>
<code>resume_fname</code>
</td>
<td>

</td>
</tr><tr>
<td>
<code>settings_system</code>
</td>
<td>

</td>
</tr><tr>
<td>
<code>settings_workspace</code>
</td>
<td>

</td>
</tr><tr>
<td>
<code>sync_file</code>
</td>
<td>

</td>
</tr><tr>
<td>
<code>sync_symlink_latest</code>
</td>
<td>

</td>
</tr><tr>
<td>
<code>wandb_dir</code>
</td>
<td>

</td>
</tr>
</table>



## Methods

<h3 id="duplicate"><code>duplicate</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_settings.py#L697-L698">View source</a>

<pre><code>duplicate()</code></pre>




<h3 id="freeze"><code>freeze</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_settings.py#L890-L892">View source</a>

<pre><code>freeze()</code></pre>




<h3 id="is_frozen"><code>is_frozen</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_settings.py#L894-L895">View source</a>

<pre><code>is_frozen() -> bool</code></pre>




<h3 id="keys"><code>keys</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_settings.py#L881-L882">View source</a>

<pre><code>keys()</code></pre>




<h3 id="load"><code>load</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_settings.py#L847-L848">View source</a>

<pre><code>load(
    fname
)</code></pre>




<h3 id="save"><code>save</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_settings.py#L844-L845">View source</a>

<pre><code>save(
    fname
)</code></pre>




<h3 id="setdefaults"><code>setdefaults</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_settings.py#L836-L842">View source</a>

<pre><code>setdefaults(
    _Settings__d=None
)</code></pre>




<h3 id="update"><code>update</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_settings.py#L744-L745">View source</a>

<pre><code>update(
    _Settings__d=None, **kwargs
)</code></pre>




<h3 id="__getitem__"><code>__getitem__</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_settings.py#L884-L888">View source</a>

<pre><code>__getitem__(
    k
)</code></pre>








<!-- Tabular view -->
<table>
<tr><th>Class Variables</th></tr>

<tr>
<td>
base_url<a id="base_url"></a>
</td>
<td>
`None`
</td>
</tr><tr>
<td>
console<a id="console"></a>
</td>
<td>
`'auto'`
</td>
</tr><tr>
<td>
disabled<a id="disabled"></a>
</td>
<td>
`False`
</td>
</tr><tr>
<td>
email<a id="email"></a>
</td>
<td>
`None`
</td>
</tr><tr>
<td>
entity<a id="entity"></a>
</td>
<td>
`None`
</td>
</tr><tr>
<td>
files_dir_spec<a id="files_dir_spec"></a>
</td>
<td>
`None`
</td>
</tr><tr>
<td>
log_dir_spec<a id="log_dir_spec"></a>
</td>
<td>
`None`
</td>
</tr><tr>
<td>
log_internal_spec<a id="log_internal_spec"></a>
</td>
<td>
`None`
</td>
</tr><tr>
<td>
log_symlink_internal_spec<a id="log_symlink_internal_spec"></a>
</td>
<td>
`None`
</td>
</tr><tr>
<td>
log_symlink_user_spec<a id="log_symlink_user_spec"></a>
</td>
<td>
`None`
</td>
</tr><tr>
<td>
log_user_spec<a id="log_user_spec"></a>
</td>
<td>
`None`
</td>
</tr><tr>
<td>
mode<a id="mode"></a>
</td>
<td>
`'online'`
</td>
</tr><tr>
<td>
program_relpath<a id="program_relpath"></a>
</td>
<td>
`None`
</td>
</tr><tr>
<td>
project<a id="project"></a>
</td>
<td>
`None`
</td>
</tr><tr>
<td>
resume_fname_spec<a id="resume_fname_spec"></a>
</td>
<td>
`None`
</td>
</tr><tr>
<td>
root_dir<a id="root_dir"></a>
</td>
<td>
`None`
</td>
</tr><tr>
<td>
run_group<a id="run_group"></a>
</td>
<td>
`None`
</td>
</tr><tr>
<td>
run_job_type<a id="run_job_type"></a>
</td>
<td>
`None`
</td>
</tr><tr>
<td>
run_name<a id="run_name"></a>
</td>
<td>
`None`
</td>
</tr><tr>
<td>
run_notes<a id="run_notes"></a>
</td>
<td>
`None`
</td>
</tr><tr>
<td>
run_tags<a id="run_tags"></a>
</td>
<td>
`None`
</td>
</tr><tr>
<td>
sagemaker_disable<a id="sagemaker_disable"></a>
</td>
<td>
`None`
</td>
</tr><tr>
<td>
save_code<a id="save_code"></a>
</td>
<td>
`None`
</td>
</tr><tr>
<td>
settings_system_spec<a id="settings_system_spec"></a>
</td>
<td>
`None`
</td>
</tr><tr>
<td>
settings_workspace_spec<a id="settings_workspace_spec"></a>
</td>
<td>
`None`
</td>
</tr><tr>
<td>
show_errors<a id="show_errors"></a>
</td>
<td>
`True`
</td>
</tr><tr>
<td>
show_info<a id="show_info"></a>
</td>
<td>
`True`
</td>
</tr><tr>
<td>
show_warnings<a id="show_warnings"></a>
</td>
<td>
`True`
</td>
</tr><tr>
<td>
silent<a id="silent"></a>
</td>
<td>
`False`
</td>
</tr><tr>
<td>
start_method<a id="start_method"></a>
</td>
<td>
`'spawn'`
</td>
</tr><tr>
<td>
sync_dir_spec<a id="sync_dir_spec"></a>
</td>
<td>
`None`
</td>
</tr><tr>
<td>
sync_file_spec<a id="sync_file_spec"></a>
</td>
<td>
`None`
</td>
</tr><tr>
<td>
sync_symlink_latest_spec<a id="sync_symlink_latest_spec"></a>
</td>
<td>
`None`
</td>
</tr>
</table>

