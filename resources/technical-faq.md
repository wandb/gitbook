# Technical FAQ

## What does this do to my training process?

When `wandb.init()` is called from your training script an API call is made to create a run object on our servers. A new process is started to stream and collect metrics, thereby keeping all threads and logic out of your primary process. Your script runs normally and writes to local files, while the separate process streams them to our servers along with system metrics. You can always turn off streaming by running `wandb off` from your training directory, or setting the **WANDB\_MODE** environment variable to "dryrun".

## If wandb crashes, will it possibly crash my training run?

It is extremely important to us that we never interfere with your training runs. We run wandb in a separate process to make sure that if wandb somehow crashes, your training will continue to run. If the internet goes out, wandb will continue to retry sending data to wandb.com.

## Will wandb slow down my training?

Wandb should have negligible effect on your training performance if you use it normally. Normal use of wandb means logging less than once a second and logging less than a few megabytes of data at each step. Wandb runs in a separate process and the function calls don't block, so if the network goes down briefly or there are intermittent read write issues on disk it should not affect your performance. It is possible to log a huge amount of data quickly, and if you do that you might create disk I/O issues. If you have any questions, please don't hesitate to contact us.

## Can I run wandb offline?

If you're training on an offline machine and want to upload your results to our servers afterwards, we have a feature for you!

1. Set the environment variable `WANDB_MODE=dryrun` to save the metrics locally, no internet required.
2. When you're ready, run `wandb init` in your directory to set the project name.
3. Run `wandb sync YOUR_RUN_DIRECTORY` to push the metrics to our cloud service and see your results in our hosted web app.

## Does your tool track or store training data?

You can pass a SHA or other unique identifier to `wandb.config.update(...)` to associate a dataset with a training run. W&B does not store any data unless `wandb.save` is called with the local file name.

## How often are system metrics collected?

By default metrics are collected every 2 seconds and averaged over a 30 second period. If you need higher resolution metrics, email us a [contact@wandb.com](mailto:contact@wandb.com).

## Does this only work for Python?

Currently the library only works with Python 2.7+ & 3.6+ projects. The architecture mentioned above should enable us to integrate with other languages easily. If you have a need for monitoring other languages, send us a note at [contact@wandb.com](mailto:contact@wandb.com).

## Can I just log metrics, no code or dataset examples?

**Dataset Examples**

By default, we don't log any of your dataset examples. You can explicitly turn this feature on to see example predictions in our web interface.

**Code Logging**

There's two ways to turn off code logging:

1. Set **WANDB\_DISABLE\_CODE** to **true** to turn off all code tracking. We won't pick up the git SHA or the diff patch.
2. Set **WANDB\_IGNORE\_GLOBS** to **\*.patch** to turn off syncing the diff patch to our servers. You'll still have it locally and be able to apply it with the [wandb restore](../library/cli.md#restore-the-state-of-your-code) command.

## Is the log function lazy? I don't want to be dependent on the network to send the results to your servers and then carry on with my local operations.

Calling **wandb.log** writes a line to a local file; it does not block on any network calls. When you call wandb.init we launch a new process on the same machine that listens for filesystem changes and talks to our web service asynchronously from your training process.

## What formula do you use for your smoothing algorithm?

We use the same exponential moving average formula as TensorBoard. You can find an explanation here: [https://stackoverflow.com/questions/42281844/what-is-the-mathematics-behind-the-smoothing-parameter-in-tensorboards-scalar](https://stackoverflow.com/questions/42281844/what-is-the-mathematics-behind-the-smoothing-parameter-in-tensorboards-scalar).

## How is wandb different from TensorBoard?

First of all, we use and love TensorBoard and you can easily integrate wandb with TensorBoard \(see [TensorBoard](../library/integrations/tensorboard.md)\).

W&B is designed to be a central repository of every training run you or your organization does. Some of the difference that come from that are:

* We scale up to saving and comparing thousands or millions of runs. 
* We also make it easy to log things at any point in your code with wandb.log\(\)
* We have visualizations like scatter plots and parallel coordinates charts designed to compare many ML runs at once
* We make it easy to build and share reports with colleagues

## How can I configure the name of the run in my training code?

At the top of your training script when you call wandb.init, pass in an experiment name, like this: `wandb.init(name="my awesome run")`

## Is there an anaconda package?

We don't have an anaconda package but you should be able to install wandb using:

```text
conda activate myenv
pip install wandb
```

If you run into issues with this install, please let us know. The anaconda managing package resource at [https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-pkgs.html](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-pkgs.html) may be helpful.

## How do I stop wandb from writing to my terminal or my jupyter notebook output?

Set the environmental variable [WANDB\_SILENT](../library/advanced/environment-variables.md).

In a notebook:

```text
%env WANDB_SILENT true
```

In a python script:

```text
os.environ["WANDB_SILENT"] = "true"
```

## How do I kill a job with wandb?

Press ctrl+D on your keyboard to stop a script that is instrumented with wandb.

## How do I deal with network issues?

If you're seeing SSL or network errors:`wandb: Network error (ConnectionError), entering retry loop.` you can try a couple of different approaches to solving this issue:

1. Upgrade your SSL certificate. If you're running the script on an Ubuntu server, run `update-ca-certificates`  We can't sync training logs without a valid SSL certificate because it's a security vulnerability.
2. If your network is flakey, run training in [offline mode](https://docs.wandb.com/resources/technical-faq#can-i-run-wandb-offline) and sync the files to us from a machine that has Internet access.
3. Try running [W&B Local](../self-hosted/local.md), which operates on your machine and doesn't sync files to our cloud servers.

## How is W&B different from TensorBoard?

We were inspired to improve experiment tracking tools for everyone. When the cofounders started working on W&B, they were inspired to build a tool for the frustrated TensorBoard users at OpenAI. Here are a few things we focused on improving:

1. **Reproduce models**: Weights & Biases is good for experimentation, exploration, and reproducing models later. We capture not just the metrics, but also the hyperparameters and version of the code, and we can save your model checkpoints for you so your project is reproducible. 
2. **Automatic organization**: If you hand off a project to a collaborator or take a vacation, W&B makes it easy to see all the models you've tried so you're not wasting hours re-running old experiments.
3. **Fast, flexible integration**: Add W&B to your project in 5 minutes. Install our free open-source Python package and add a couple of lines to your code, and every time you run your model you'll have nice logged metrics and records.
4. **Persistent, centralized dashboard**: Anywhere you train your models, whether on your local machine, your lab cluster, or spot instances in the cloud, we give you the same centralized dashboard. You don't need to spend your time copying and organizing TensorBoard files from different machines.
5. **Powerful table**: Search, filter, sort, and group results from different models. It's easy to look over thousands of model versions and find the best performing models for different tasks. TensorBoard isn't built to work well on large projects.
6. **Tools for collaboration**: Use W&B to organize complex machine learning projects. It's easy to share a link to W&B, and you can use private teams to have everyone sending results to a shared project. We also support collaboration via reports— add interactive visualizations and describe your work in markdown. This is a great way to keep a work log, share findings with your supervisor, or present findings to your lab.

Get started with a [free personal account →](http://app.wandb.ai/)

