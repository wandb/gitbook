# wandb launch

**Usage**

`wandb launch [OPTIONS] [URI]`

**Summary**

Launch or queue a job from a uri (Experimental). A uri can be either a wandb
uri of the form https://wandb.ai/<entity>/<project>/runs/<run_id>, or a git
uri pointing to a remote repository, or path to a local directory.

**Options**

| **Option** | **Description** |
| :--- | :--- |
| -j, --job <str> | Name of the job to launch. If passed in,   launch does not require a uri. |
| --entry-point | Entry point within project. [default: main].   If the entry point is not found, attempts to |
| run the project file with the specified name | as a script, using 'python' to run .py files |
| and the default shell (specified by | environment variable $SHELL) to run .sh |
| files. If passed in, will override the | entrypoint value passed in using a config |
| -g, --git-version | Version of the project to run, as a Git   commit reference for Git projects. |
| -a, --args-list | An argument for the run, of the form -a   name=value. Provided arguments that are not |
| in the list of arguments for an entry point | will be passed to the corresponding entry |
| point as command-line arguments in the form | `--name value` |
| --name | Name of the run under which to launch the   run. If not specified, a random run name |
| will be used to launch run. If passed in, | will override the name passed in using a |
| -e, --entity <str> | Name of the target entity which the new run   will be sent to. Defaults to using the |
| entity set by local wandb/settings folder.If | passed in, will override the entity value |
| -p, --project <str> | Name of the target project which the new run   will be sent to. Defaults to using the |
| project name given by the source uri or for | github runs, the git repo name. If passed |
| in, will override the project value passed | in using a config file. |
| -r, --resource | Execution resource to use for run. Supported   values: 'local'. If passed in, will override |
| the resource value passed in using a config | file. Defaults to 'local'. |
| -d, --docker-image | Specific docker image you'd like to use. In |
| the form name:tag. If passed in, will | override the docker image value passed in |
| -c, --config | Path to JSON file (must end in '.json') or   JSON string which will be passed as a launch |
| config. Dictation how the launched run will | be configured. |
| -q, --queue | Name of run queue to push to. If none,   launches single run directly. If supplied |
| without an argument (`--queue`), defaults to | queue 'default'. Else, if name supplied, |
| specified run queue must exist under the | project and entity supplied. |
| --async | Flag to run the job asynchronously. Defaults   to false, i.e. unless --async is set, wandb |
| launch will wait for the job to finish. This | option is incompatible with --queue; |
| asynchronous options when running with an | agent should be set on wandb launch-agent. |
| --resource-args | Path to JSON file (must end in '.json') or   JSON string which will be passed as resource |
| args to the compute resource. The exact | content which should be provided is |
| different for each execution backend. See | documentation for layout of this file. |
| --cuda | Flag to build an image with CUDA enabled. If   reproducing a previous wandb run that ran on |
| a CUDA-enabled image will be built by | default and you must set --cuda=False to |
| --help | Show this message and exit. |

