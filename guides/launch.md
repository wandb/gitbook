---
description: Reproduce runs and orchestrate experiments
---

# \[Beta] W\&B Launch

_W\&B Launch is very early in development, and we're actively working on building out the set of features we support for queueing and reproducing runs. Please reach out to us at support@wandb.com with any questions or suggestions._

W\&B Launch is the new beta tool for reproducing runs and orchestrating experiments. Launch provides a streamlined way to reproduce runs, create and manage queues of experiments through the UI or CLI, and have the experiments run automatically on your own infrastructure.

### What runs can be reproduced by W\&B Launch?

These are the prerequisites for using W\&B Launch to re-run an existing run with new settings:

* **Git repo**: The original run needs to have an associated git repo.
* **Git commit**: The git commit associated with the run must be pushed to the remote repo.
* **requirements.txt**: In your git repo, you need to have a `requirements.txt` file at the root of the repo, and this `requirements.txt` must include the most recent version of `wandb`
* **Docker**: The machine where you are launching the run must have a recent version of Docker
* **W\&B Authentication**: The machine used to launch the run must be logged into W\&B
* **Git Authentication**: The machine must have permissions for the associated git repo
* **Enable Launch**: add "instant replay" to your profile bio to activate launch UI features.
* **pip**: on the machine you're intending to launch the run, run `pip install --upgrade "wandb[launch]"`

You do not need:

* A clean git commit for the original run — W\&B Launch will automatically apply the`diff.patch` file, which captures any changes since the last git commit. If you change the git commit in your Launch configuration, this patch will not be used.
* If you used W\&B Artifacts `use_artifact` to pull in your dataset or other input files, your data doesn't need to be local or pre-loaded to launch the run with W\&B Launch.
* A run originally run with the most recent `wandb` library version — Launch is backwards compatible.
* A run with a Docker image — we use Docker under the hood in W\&B Launch to create a fully reproducible environment for you, but you don't need to have used Docker when running your original run.

## Reproduce runs using wandb launch

Use `wandb launch` to relaunch a run:

1. Find the URL of the run you intend on launching. `https://wandb.ai/<username>/<project>/runs/<run_name>`
2. From a machine that is able to run this run, use the command `wandb launch <https://wandb.ai/><username>/<project>/runs/<run_name>`
3. The new run will be created in your project using the exact same input arguments as the original run.

{% hint style="info" %}
When reproducing a run, if the original script is not at the root of the repo, you may need to specify the entry point using `--entry-point` or `-E` to point to the path of the script.
{% endhint %}

## Launch runs from queues

To automatically launch new runs:

1. **Add runs to the run queue** from the UI, either from the command line or from the UI. These runs will be automatically picked up and launched by your launch agent.
2. **Set up a launch agent** on your workstation. Launch agents are processes that listen for new runs in the run queue, and launch new runs on your own infrastructure.

### 1. Add runs to the run queue

You can add runs to the queue from the App UI or from the command line.

#### Add runs to Launch from the App UI

1. Open your [project page workspace](../ref/app/pages/project-page.md#workspace-tab) and look in the left sidebar, where the list of runs is
2. Hover over a run to see the dropdown menu button appear on the left, next to the eye
3. Click the menu, and select the option to **Push to runqueue**

![](<../.gitbook/assets/image (146).png>)

This will open a modal where you can edit the launch config that defines how a run is launched:

![](<../.gitbook/assets/image (147).png>)

In this modal, you can select the target entity, project and queue you'd like to add the queued run to. You can also set the overrides, as well as other information such as the name or git version.

#### Add to runs to Launch from the CLI

Runs can also be added to a run queue using the CLI. To do this simply add the `queue` flag when using `wandb launch` .

```
wandb launch --queue <uri>
```

You can specify user-created queues with `--queue <queuename>`. Without a specified queue name (as above), runs will be added to the default queue for the project.

As with launching an individual run, you can specify a config file, or JSON string as your launch config using the `-c, --config` flag.

### 2. Set up the launch agent

To run jobs scheduled on a run queue, start a launch agent on the machine where you intend on running your experiments. Use `wandb launch-agent <project>` to launch an agent.

The agent will pop items off of the queue(s) and run them one-at-a-time, on the local machine. This way you can queue up a sequence of jobs and have them run automatically.

## See the Status of Queued Runs

Queued runs can be seen in the Launch Tab. Runs can be deleted from the run queue using the triple dot menu for each run.

![](<../.gitbook/assets/image (150).png>)

## The Launch Config Specification

All configurable options definable in the launch config and in `wandb launch` arguments are discussed here:

| Launch config         | CLI Flag           | Type                       | Description                                                                                                                                                                                                                                 |
| --------------------- | ------------------ | -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| uri                   | Main argument      | string                     | A wandb URI, GitHub URI or local directory to run                                                                                                                                                                                           |
| project               | --project, -p      | string                     | Target project for launched run. Defaults to source project for wandb URIs. Uncategorized for GitHub and local directory URIs                                                                                                               |
| entity                | --entity, -e       | string                     | Target entity for launched run. Defaults to logged in user                                                                                                                                                                                  |
| name                  | --name, -n         | string                     | Name under which launched run will be viewable in wandb UI. Defaults to using wandb's name generator                                                                                                                                        |
| resource              | --resource, -r     | string                     | The resource to use to launch the run. For now, only local is accepted. Defaults to local                                                                                                                                                   |
| docker                |                    |                            | Controls for docker                                                                                                                                                                                                                         |
| docker.docker_image   | --docker-image, -d | string                     | The docker image to launch the run with. The supplied docker image is assumed to have the required python packages required to launch the run. Defaults to using repo2docker to generate an image                                           |
| docker.user_id        | None               | int                        | The user id to use within the docker container. Defaults to 1000.                                                                                                                                                                           |
| \[Future]             | --docker-args,-A   | key=val pairs              | Specify docker args to use while running the docker command                                                                                                                                                                                 |
| git                   |                    |                            | Controls for git                                                                                                                                                                                                                            |
| git.repo              | None               | string                     | \[Future] Allows you to specify a git repo to use instead of the default one associated with a run.                                                                                                                                         |
| git.git_version       | --git-version, -g  | string                     | Specify a commit hash to use. If specified, any diff.patch associated with the run will not be applied. Defaults to source run's commit, or HEAD in the case of git repos URIs.                                                             |
| overrides             |                    |                            |                                                                                                                                                                                                                                             |
| overrides.args        | --args-list, -a    | List\[str] / key=val pairs | A list of strings representing the command line arguments to use when launching a run. Defaults to using the source run's args for wandb URIs.                                                                                              |
| overrides.run_config  | None               | Dict\[str, Any]            | Override values to use in the wandb.run.config                                                                                                                                                                                              |
| overrides.entry_point | --entry-point, -E  | string                     | <p>Specify the relative path of the entrypoint to run for the launched run. If not specified will use:</p><ul><li>the previous runs entry point for wandb URI</li><li><code>main.py</code> for git repos and local directory URIs</li></ul> |
|                       | --config, -c       | string                     | specifies the launch_config to use for the run either as a file path or json string                                                                                                                                                         |
|                       | --queue, -q        | string                     | Use the argument provided to build a full launch_config for this run, and place it on the specified queue.                                                                                                                                  |

## Frequently Asked Questions

### Can I use launch to create new runs?

`wandb launch` supports running new runs using both git repos or local directories.

To launch from a git repo, run the same launch commands as above but with the URL to a Github, GItlab, or Bitbucket repo, e.g. `wandb launch <https://github.com/user/repo`>. We require that the repo contain either a `requirements.txt` configuration file for dependencies, and the code should be already instrumented with wandb for us to track it as normal.

To launch from a local directory, run with a local path, e.g. `wandb launch path/to/local`. As with git repos, we also require a `requirements.txt` file at the root of the provided path. Queueing is not currently supported for launching from a local directory.

In both cases, you'll want to specify an entry point using the `--entry-point` or `-E` flag.

### Can I reproduce experiments that weren't logged to W\&B?

We currently have limited support for launching new runs — runs that are not direct reproductions of existing wandb runs — from a git repo or a local directory. Agent queueing is only supported for existing wandb runs with git repos. However, this can be done using `wandb launch -E path/to/entrypoint.py <git url|local directory>`

### How can I slightly modify a previous run, that was logged to wandb?

When using `wandb launch <uri>` you can modify how the launched run behaves to be different from the original run.

* `-a param=val`: Modify the arguments used for your script
* `-c, --config`: Configure a newly launched run, using a file or JSON string to control the behavior of the launched run. Use the following structure in your config file to control how the launched run behaves:

```python
{
    "entity": String, // set the target entity of the launched run
  "project": String, // set the target project of the launched run
  "git": {
        "version": String // set the commit hash for the launched run
  },
    "name": String, // set the name of the launched run
    "overrides": {
        "args": [
            "name1", "val1", ...
        ],
        "run_config": {
            "key1": val1, // set the values used by wandb.run.config
            ...
        }
    }
}
```

### Does every agent have to work from the same queue?

No, different agents can listen to different queues. You can manage your run queues in the Launch Tab within a project workspace. This way you can set up a run queue for different machines or different users.

To create a new run queue:

1. Go to your [project page](https://docs.wandb.ai/ref/app/pages/project-page), and click on the Launch tab. Create, delete and view the runs in each queue within a [project workspace](../ref/app/pages/project-page.md#workspace-tab).
2. Click the "Create Queue" button to create a new run queue. If your project is inside a team, you can create a private queue, or you can create a queue that is available to the entire team.

![](<../.gitbook/assets/image (149).png>)

You can specify the name of the queue for an agent to use using the `wandb launch-agent --queues <queue-name> <project>`

### Can I connect agents to different queues?

An agent can run jobs from a single queue or multiple queues at a time, specified as a list of comma-separated names, e.g. `--queues q1,q2,q3`. If you don't specify a queue with the `--queues` flag, the agent will run jobs from the default queue for the project.

### How can I create a default run queue for my project through the CLI?

Start by adding a run to the default run queue for your project (`<project>`) belonging to entity (`<entity>`) by using:

`wandb launch --queue default https://wandb.ai/<entity>/<project>/runs/<run_id>`

This will create a default run queue, and push the run at the URI to it.

{% hint style="info" %}
This strategy will only work for creating the default run queue. Other queue names will not work.
{% endhint %}

Alternatively, visit the Launch tab in the project workspace to automatically create a default run queue.

### How can I delete a run queue?

Full run queues can be deleted as well. Select the queue or queues to be deleted, and click delete. This will delete queued runs that have not started, but not delete any runs that have been started.

![](<../.gitbook/assets/image (148).png>)

{% hint style="info" %}
The project's default queue cannot be deleted.
{% endhint %}
