# wandb launch

**Usage**

`wandb launch [OPTIONS] [URI]`

**Summary**

_Experimental feature._ Run or queue a job to be run via [W\&B Launch](https://app.gitbook.com/o/-Lr2SEfv2R3GSuF1kZCt/s/eBMXEnBcc69nbJyR73F6/). `URI` can be one of: a URL to an existing wandb run, a URL to a public git repository (e.g. on GitHub), or a path to a local directory. If `URI` is not specified, option `--docker-image` must be set (see below).

**Options**

| **Option**         | **Description**                                                                                                                                                                                                                                                 |
| ------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| --entry-point      | Entry point of the job, given as the executable and path to the script, eg. `python main.py` in the case of wandb run URI's this defaults to the entry point of the given run, otherwise defaults to `python main.py`                                           |
| -g, --git-version  | Version of the project to run, as a Git commit reference for Git projects.                                                                                                                                                                                      |
| -a, --args-list    | An argument for the run, of the form -a name=value. Provided arguments that are not in the list of arguments for an entry point will be passed to the corresponding entry point as command-line arguments in the form `--name value`                            |
| --name             | Name of the run under which to launch the run. If not specified, a random run name will be used to launch run.                                                                                                                                                  |
| -e, --entity       | Name of the target entity which the new run will be sent to. Defaults to using the entity set by local wandb/settings folder. If passed in, will override the entity value passed in using a config file.                                                       |
| -p, --project      | Name of the target project which the new run will be sent to. Defaults to using the project name given by the source uri or for github runs, the git repo name. If passed in, will override the project value passed in using a config file.                    |
| -r, --resource     | Execution resource to use for run. Supported values: 'local'. If passed in, will override the resource value passed in using a config file. Defaults to 'local'.                                                                                                |
| -d, --docker-image | Specific docker image you'd like to use. In the form name:tag. If passed in, will override the docker image value passed in                                                                                                                                     |
| -c, --config       | Path to JSON file (must end in '.json') or JSON string which will be passed as config to the compute resource. The exact content which should be provided is different for each execution backend.                                                              |
| -q, --queue        | Name of run queue to push to. If none, launches single run directly. If supplied without an argument (`--queue`), defaults to queue 'default'. Else, if name supplied, specified run queue must exist under the project and entity supplied.                    |
| --async            | Flag to run the job asynchronously. Defaults to false, i.e. unless --async is set, wandb launch will wait for the job to finish. This option is incompatible with --queue; asynchronous options when running with an agent should be set on wandb launch-agent. |
| --resource-args    | A resource argument for launching runs with cloud providers, of the form -R name=value.                                                                                                                                                                         |
| --help             | Show this message and exit.                                                                                                                                                                                                                                     |
