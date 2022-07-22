---
description: Reproducibility, orchestration, and CI/CD
---

# Launch Jobs

{% hint style="danger" %}
This new product is in beta and under active development. Please message support@wandb.com with questions and suggestions.
{% endhint %}

Use W\&B Launch to kick off jobs on your own infrastructure from the W\&B UI or CLI.

* Execute runs in reproducible, containerized environments
* Queue and launch jobs across machines, and across blended infrastructure
* Easily tweak hyperparameters or input data and retrain models
* \[Coming Soon] Automatically run evaluation jobs on newly trained models

## Quickstart

{% hint style="success" %}
First, go to [**Settings**](https://wandb.ai/settings) and turn on the **W\&B Launch** toggle to enable this feature in the UI.

Then on the command line run **`pip install --upgrade wandb`** to get the latest version of our SDK.
{% endhint %}

The standard use case for W\&B Launch is to integrate with [Kubernetes](integrations/kubernetes.md) or [SageMaker](integrations/sagemaker.md), then easily launch jobs on machines in a remote cluster. In this quickstart, we run a [Local Agent](integrations/local.md) for simplicity:

1. Open a project and pick a run from your Runs Workspace.
2. Click the menu to `Add to Launch Queue`
3. Update the config to tweak hyperparameters
4. On your command line, go to the machine where you want to launch that job, and run the command `wandb launch-agent <your-project-name>`

This will start an agent running on your machine, which will pull down queued jobs and run them automatically. See the live results of the new run stream in to your project page.

Here's a short screen video of what it looks like to pick an existing run from the project page, update config, and see it get added to the Launch queue:

![](<../../.gitbook/assets/2022-06-10 09.25.31.gif>)

## Documentation

* CLI reference for [`wandb launch`](../../ref/cli/wandb-launch.md) and [`wandb launch-agent`](../../ref/cli/wandb-launch-agent.md)``
* Resource documentation for supported Launch integrations:
  * [Local](integrations/local.md)
  * [Amazon SageMaker](integrations/sagemaker.md)
  * [Kubernetes](integrations/kubernetes.md)
* Details on [Launch containerization](containers.md) and bring-your-own-container options
* Details on [Launch agents](agents.md), including how to deploy an agent in your own infrastructure

## Frequently asked questions

### Can I use launch to create new runs?

`wandb launch` supports running new runs (i.e. runs not based on an existing wandb run) from both remote git repositories or local directories.

To launch from a git repo, run the same launch commands as above but with the URL to a Github, GitLab, or Bitbucket repo, e.g. `wandb launch https://github.com/user/repo`. We require that the repo contain either a `requirements.txt` or `environment.yml` configuration file for dependencies, and the code should be already instrumented with wandb for us to track it as normal.

To launch from a local directory, run with a local path, e.g. `wandb launch path/to/local`. As with git repos, we also require a `requirements.txt` or `environment.yml` file at the root of the provided path. Queueing is not currently supported for launching from a local directory.

In both cases, you'll want to specify an entry point using the `--entry-point` or `-E` flag.

### Does every agent have to work from the same queue?

No, different agents can listen to different queues. You can manage your run queues in the Launch Tab within a project workspace. This way you can set up a run queue for different machines or different users.

To create a new run queue:

1. Go to your [project page](https://docs.wandb.ai/ref/app/pages/project-page), and click on the Launch tab. Create, delete and view the runs in each queue within a [project workspace](../../ref/app/pages/project-page.md#workspace-tab).
2. Click the "Create Queue" button to create a new run queue. If your project is inside a team, you can create a private queue, or you can create a queue that is available to the entire team.

![](<../../.gitbook/assets/image (149).png>)

You can specify the name of the queue for an agent to use using the `wandb launch-agent --queues <queue-name> <project>`

### Can I connect agents to different queues?

An agent can run jobs from a single queue or multiple queues at a time, specified as a list of comma-separated names, e.g. `--queues q1,q2,q3`. If you don't specify a queue with the `--queues` flag, the agent will run jobs from the default queue for the project.

### How can I delete a run queue?

Full run queues can be deleted as well. Select the queue or queues to be deleted, and click delete. This will delete queued runs that have not started, but not delete any runs that have been started.

![](<../../.gitbook/assets/image (151).png>)

{% hint style="info" %}
The project's default queue cannot be deleted.
{% endhint %}
