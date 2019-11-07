# Technical FAQ

### What does this do to my training process?

When `wandb.init()` is called from your training script an API call is made to create a run object on our servers. A new process is started to stream and collect metrics, thereby keeping all threads and logic out of your primary process. Your script runs normally and writes to local files, while the separate process streams them to our servers along with system metrics. You can always turn off streaming by running `wandb off` from your training directory, or setting the **WANDB\_MODE** environment variable to "dryrun".

### If wandb crashes, will it possibly crash my training run?

It is extremely important to us that we never interfere with your training runs. We run wandb in a separate process to make sure that if wandb somehow crashes, your training will continue to run. If the internet goes out, wandb will continue to retry sending data to wandb.com.

### Will wandb slow down my training?

Wandb should have negligible effect on your training performance if you use it normally.  Normal use of wandb means logging less than once a second and logging less than a few megabytes of data at each step.  Wandb runs in a separate process and the function calls don't block, so if the network goes down briefly or there are intermittent read write issues on disk it should not affect your performance.  It is possible to log a huge amount of data quickly, and if you do that you might create disk I/O issues.  If you have any questions, please don't hesitate to contact us..

### Does your tool track or store training data?

You can pass a SHA or other unique identifier to `wandb.config.update(...)` to associate a dataset with a training run. W&B does not store any data unless `wandb.save` is called with the local file name.

### How often are system metrics collected?

By default, metrics are collected every 2 seconds and averaged over a 30 second period. If you need higher resolution metrics, email us a [contact@wandb.com](mailto:contact@wandb.com).

### Does this only work for Python?

Currently the library only works with Python 2.7+ & 3.6+ projects. The architecture mentioned above should enable us to integrate with other languages easily. If you have a need for monitoring other languages, send us a note at [contact@wandb.com](mailto:contact@wandb.com).

### Can I just log metrics, no code or dataset examples?

**Dataset Examples**

By default, we don't log any of your dataset examples. You can explicitly turn this feature on to see example predictions in our web interface.

**Code Logging**

There's two ways to turn off code logging:

1. Set **WANDB\_DISABLE\_CODE** to **true** to turn off all code tracking. We won't pick up the git SHA or the diff patch.
2. Set **WANDB\_IGNORE\_GLOBS** to **\*.patch** to turn off syncing the diff patch to our servers. You'll still have it locally and be able to apply it with the [wandb restore](../library/cli.md#restore-the-state-of-your-code) command.

### Is the log function lazy? I don't want to be dependent on the network to send the results to your servers and then carry on with my local operations.

Calling **wandb.log** writes a line to a local file; it does not block on any network calls. When you call wandb.init we launch a new process on the same machine that listens for filesystem changes and talks to our web service asynchronously from your training process.

### What formula do you use for your smoothing algorithm?

We use the same exponential moving average formula as tensorboard.  You can find an explanation at [https://stackoverflow.com/questions/42281844/what-is-the-mathematics-behind-the-smoothing-parameter-in-tensorboards-scalar](https://stackoverflow.com/questions/42281844/what-is-the-mathematics-behind-the-smoothing-parameter-in-tensorboards-scalar).

### How is wandb different from tensorboard?

First of all, we use and love tensorboard and you can easily integrate wandb with tensorboard \(see [Tensorboard](../library/integrations/tensorboard.md)\).

Wandb is designed to be a central repository of every training run you or your organization does.  Some of the difference that come from that are:

* We scale up to saving and comparing thousands or millions of runs. 
* We also make it easy to log things at any point in your code with wandb.log\(\)
* We have visualizations like scatterplots and parallel coordinates charts designed to compare many ML runs at once
* We make it easy to build and share reports with colleagues

### How can I configure the name of the run in my training code?

Call `wandb.init(name="MY_NAME")`

