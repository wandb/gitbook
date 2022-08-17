---
description: >-
  Containerized Reproducible Workflows, Connect to W&B to Cloud Infrastructure,
  CI/CD
---

# Launch Jobs

{% hint style="danger" %}
### <mark style="color:red;">**Beta product in active development**</mark>

<mark style="color:red;">**Interested in Launch? Reach out to your account team to talk about joining the customer pilot program for W\&B Launch.**</mark>

<mark style="color:red;">**Pilot customers need to use AWS EKS or SageMaker to qualify for the beta program. We ultimately plan to support additional platforms.**</mark>
{% endhint %}

Connect your own SageMaker or Kubernetes cluster, then easily queue and manage jobs using W\&B Launch. Kick off jobs on your own infrastructure from the W\&B UI or CLI.

* Execute runs in reproducible containerized environments
* Queue and launch jobs across your own clusters, locally or in the cloud
* Easily tweak hyperparameters or input data and retrain models
* \[Coming Soon] Automatically trigger evaluation jobs on newly trained models

## Launch Quickstart

{% hint style="success" %}
**Before we get started**

Go to [Settings](https://wandb.ai/settings) and turn on the **W\&B Launch** toggle, then upgrade your SDK with **`pip install --upgrade wandb`**. Make sure you have Docker installed and running.
{% endhint %}

The standard use case for W\&B Launch is to integrate with [Kubernetes](integrations/kubernetes.md) or [SageMaker](integrations/sagemaker.md), then easily launch jobs on machines in a remote cluster.

In this Quickstart, we will instead build and run a job locally, for simplicity. You can get started by cloning our examples repository!

```bash
git clone https://github.com/wandb/examples
cd ./examples/examples/launch/launch-quickstart
```

The `launch-quickstart` example contains a script `train.py` that trains a simple neural net with keras and then logs metrics and predictions back to Weights & Biases. There is also a Dockerfile so that you can build the training script into a container image. To do so, run:

```
docker build . -t fmnist-training
```

Now that the training container is built, you can train the model and see results in a Weights & Biases dashboard by running:

```
wandb launch -d fmnist-training -p fmnist
```

The run will be logged to a newly created `fmnist` project in your Weights & Biases account!

#### **Next, train another model from the UI!**

In addition to launching jobs with the `wandb` command line interface, you can also submit jobs through the Weights & Biases UI.&#x20;

Step 1 is to run:

&#x20;`wandb launch-agent -p fmnist`&#x20;

on your machine. Now, head back your `fmnist` project on the W\&B site. Follow the video below to navigate to the launch menu:

![Opening the launch menu](<../../.gitbook/assets/2022-08-05 17.48.04.gif>)

You can copy and paste the following `JSON` snippet into the editor that appears, then click `Push Run`.

```json
{
  "overrides": {
    "run_config": {
      "batch_size": 128, 
      "optimizer": "adam"
    }
  },
  "docker": {
    "docker_image": "fmnist-training"
  }
}
```

The agent you started on your machine earlier will pull the run from the queue you pushed it to and run it on your machine! The `run_config` `overrides` that we passed in will actually modify the contents of our `wandb.config` when `wandb.init` is called, so you modify any hyperparameter and relaunch an experiment without leaving your dashboard.

## Documentation

* CLI reference for [`wandb launch`](../../ref/cli/wandb-launch.md) and [`wandb launch-agent`](../../ref/cli/wandb-launch-agent.md)\`\`
* Resource documentation for supported Launch integrations:
  * [Local](integrations/local.md)
  * [Amazon SageMaker](integrations/sagemaker.md)
  * [Kubernetes](integrations/kubernetes.md)
* Details on [Launch containerization](containers.md) and bring-your-own-container options
* Details on [Launch agents](agents.md), including how to deploy an agent in your own infrastructure

## Frequently asked questions

### I use Google Kubernetes Engine. Can i still use Launch?

Weights and Biases currently does not support GKE, but will in subsequent quarters. Reach out to your account team if you are interested in connecting with the product team and providing feedback.&#x20;

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

### How do I log code to use with W\&B Launch?

If you try to use Launch to re-run an existing run, and you get the error:

> Error: Reproducing a run requires either an associated git repo or a code artifact logged with `run.log_code()`

You have two options to log code so that the run is reproducible:

1. **Git**: Move your script to a git repo. We can automatically use the git commit and diff patch to restore the exact state of the code.
2. **Code Logging**: You can also use the built in feature `wandb.log_code()` in your script to save the code as an artifact with your run.
