# Technical FAQ

### What does this do to my training process?

When `wandb.init()` is called from your training script an API call is made to create a run object on our servers. A new process is started to stream and collect metrics, thereby keeping all threads and logic out of your primary process. Your script runs normally and writes to local files, while the separate process streams them to our servers along with system metrics. You can always turn off streaming by running `wandb off` from your training directory, or setting the `WANDB_MODE` environment variable to `offline`.

### If wandb crashes, will it possibly crash my training run?

It is extremely important to us that we never interfere with your training runs. We run wandb in a separate process to make sure that if wandb somehow crashes, your training will continue to run. If the internet goes out, wandb will continue to retry sending data to [wandb.ai](https://wandb.ai).

### **Why is a run marked crashed in W\&B when it’s training fine locally?**

This is likely a connection problem — if your server loses internet access and data stops syncing to W\&B, we mark the run as crashed after a short period of retrying.

### Will wandb slow down my training?

Wandb should have negligible effect on your training performance if you use it normally. Normal use of wandb means logging less than once a second and logging less than a few megabytes of data at each step. Wandb runs in a separate process and the function calls don't block, so if the network goes down briefly or there are intermittent read write issues on disk it should not affect your performance. It is possible to log a huge amount of data quickly, and if you do that you might create disk I/O issues. If you have any questions, please don't hesitate to contact us.

### Can I run wandb offline?

If you're training on an offline machine and want to upload your results to our servers afterwards, we have a feature for you!

1. Set the environment variable `WANDB_MODE=offline` to save the metrics locally, no internet required.
2. When you're ready, run `wandb init` in your directory to set the project name.
3. Run `wandb sync YOUR_RUN_DIRECTORY` to push the metrics to our cloud service and see your results in our hosted web app.

### Can I disable wandb when testing my code?

By using `wandb.init(mode="disabled")` or by setting `WANDB_MODE=disabled` you will make wandb act like a NOOP which is perfect for testing your code.

**Note**: Setting `wandb.init(mode=“disabled”)` does not prevent `wandb` from saving artifacts to `WANDB_CACHE_DIR`

### Does your tool track or store training data?

You can pass a SHA or other unique identifier to `wandb.config.update(...)` to associate a dataset with a training run. W\&B does not store any data unless `wandb.save` is called with the local file name.

### How often are system metrics collected?

By default metrics are collected every 2 seconds and averaged over a 30 second period. If you need higher resolution metrics, email us a [contact@wandb.com](mailto:contact@wandb.com).

### Does this only work for Python?

Currently the library only works with Python 2.7+ & 3.6+ projects. The architecture mentioned above should enable us to integrate with other languages easily. If you have a need for monitoring other languages, send us a note at [contact@wandb.com](mailto:contact@wandb.com).

### Can I just log metrics, no code or dataset examples?

**Dataset Examples**

By default, we don't log any of your dataset examples. You can explicitly turn this feature on to see example predictions in our web interface.

**Code Logging**

There's two ways to turn off code logging:

1. Set `WANDB_DISABLE_CODE` to `true` to turn off all code tracking. We won't pick up the git SHA or the diff patch.
2. Set `WANDB_IGNORE_GLOBS` to `*.patch` to turn off syncing the diff patch to our servers. You'll still have it locally and be able to apply it with the [wandb restore](../track/advanced/save-restore.md) command.

### Does logging block my training?

"Is the logging function lazy? I don't want to be dependent on the network to send the results to your servers and then carry on with my local operations."

Calling `wandb.log` writes a line to a local file; it does not block on any network calls. When you call `wandb.init` we launch a new process on the same machine that listens for filesystem changes and talks to our web service asynchronously from your training process.

### What formula do you use for your smoothing algorithm?

We use the same exponential moving average formula as TensorBoard. You can find an explanation here: [https://stackoverflow.com/questions/42281844/what-is-the-mathematics-behind-the-smoothing-parameter-in-tensorboards-scalar](https://stackoverflow.com/questions/42281844/what-is-the-mathematics-behind-the-smoothing-parameter-in-tensorboards-scalar).

### How is W\&B different from TensorBoard?

We love the TensorBoard folks, and we have a [TensorBoard integration](../integrations/tensorboard.md)! We were inspired to improve experiment tracking tools for everyone. When the cofounders started working on W\&B, they were inspired to build a tool for the frustrated TensorBoard users at OpenAI. Here are a few things we focused on improving:

1. **Reproduce models**: Weights & Biases is good for experimentation, exploration, and reproducing models later. We capture not just the metrics, but also the hyperparameters and version of the code, and we can save your model checkpoints for you so your project is reproducible.
2. **Automatic organization**: If you hand off a project to a collaborator or take a vacation, W\&B makes it easy to see all the models you've tried so you're not wasting hours re-running old experiments.
3. **Fast, flexible integration**: Add W\&B to your project in 5 minutes. Install our free open-source Python package and add a couple of lines to your code, and every time you run your model you'll have nice logged metrics and records.
4. **Persistent, centralized dashboard**: Anywhere you train your models, whether on your local machine, your lab cluster, or spot instances in the cloud, we give you the same centralized dashboard. You don't need to spend your time copying and organizing TensorBoard files from different machines.
5. **Powerful table**: Search, filter, sort, and group results from different models. It's easy to look over thousands of model versions and find the best performing models for different tasks. TensorBoard isn't built to work well on large projects.
6. **Tools for collaboration**: Use W\&B to organize complex machine learning projects. It's easy to share a link to W\&B, and you can use private teams to have everyone sending results to a shared project. We also support collaboration via reports— add interactive visualizations and describe your work in markdown. This is a great way to keep a work log, share findings with your supervisor, or present findings to your lab.

Get started with a [free personal account →](http://app.wandb.ai)

### How can I configure the name of the run in my training code?

At the top of your training script when you call `wandb.init`, pass in an experiment name, like this: `wandb.init(name="my awesome run")`

### How do I get the random run name in my script?

Call `wandb.run.save()` and then get the name with `wandb.run.name` .

### Is there an anaconda package?

Yes! You can either install with `pip` or with `conda`. For the latter, you'll need to get the package from the [conda-forge](https://conda-forge.org) channel.

{% tabs %}
{% tab title="pip" %}
```bash
# Create a conda env
conda create -n wandb-env python=3.8 anaconda
# Activate created env
conda activate wandb-env
# install wandb with pip in this conda env
pip install wandb
```
{% endtab %}

{% tab title="conda" %}
```
conda activate myenv
conda install wandb --channel conda-forge
```
{% endtab %}
{% endtabs %}

If you run into issues with this install, please let us know. This Anaconda [doc on managing packages](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-pkgs.html) has some helpful guidance.

### How do I stop wandb from writing to my terminal or my jupyter notebook output?

Set the environment variable [`WANDB_SILENT`](../track/advanced/environment-variables.md) to `true`.

{% tabs %}
{% tab title="Python" %}
```python
os.environ["WANDB_SILENT"] = "true"
```
{% endtab %}

{% tab title="Jupyter Notebook" %}
```python
%env WANDB_SILENT=true
```
{% endtab %}

{% tab title="Command Line" %}
```python
WANDB_SILENT=true
```
{% endtab %}
{% endtabs %}

### How do I kill a job with wandb?

Press ctrl+D on your keyboard to stop a script that is instrumented with wandb.

### How do I deal with network issues?

If you're seeing SSL or network errors:`wandb: Network error (ConnectionError), entering retry loop.` you can try a couple of different approaches to solving this issue:

1. Upgrade your SSL certificate. If you're running the script on an Ubuntu server, run `update-ca-certificates` We can't sync training logs without a valid SSL certificate because it's a security vulnerability.
2. If your network is flaky, run training in [offline mode](https://docs.wandb.ai/guides/track/launch#is-it-possible-to-save-metrics-offline-and-sync-them-to-w-and-b-later) and sync the files to us from a machine that has Internet access.
3. Try running [W\&B Private Hosting](../self-hosted/), which operates on your machine and doesn't sync files to our cloud servers.

`SSL CERTIFICATE_VERIFY_FAILED`: this error could be due to your company's firewall. You can set up local CAs and then use:

`export REQUESTS_CA_BUNDLE=/etc/ssl/certs/ca-certificates.crt`

### What happens if internet connection is lost while I'm training a model?

If our library is unable to connect to the internet it will enter a retry loop and keep attempting to stream metrics until the network is restored. During this time your program is able to continue running.

If you need to run on a machine without internet, you can set `WANDB_MODE=offline` to only have metrics stored locally on your hard drive. Later you can call `wandb sync DIRECTORY` to have the data streamed to our server.

### Can I log metrics on two different time scales? (For example I want to log training accuracy per batch and validation accuracy per epoch.)

Yes, you can do this by logging your indices (e.g. `batch` and `epoch`) whenever you log your other metrics. So in one step you could call `wandb.log({'train_accuracy': 0.9, 'batch': 200})` and in another step call `wandb.log({'val_acuracy': 0.8, 'epoch': 4})`. Then, in the UI, you can set the appropriate value as the x-axis for each chart. If you want to set the default x-axis of particular index you can do so using by using [Run.define\_metric()](https://docs.wandb.ai/ref/python/run#define\_metric). In our above example we could do the following:

```python
wandb.init()

wandb.define_metric("batch")
wandb.define_metric("epoch")

wandb.define_metric("train_accuracy", step_metric="batch")
wandb.define_metric("val_accuracy", step_metric="epoch")
```

### How can I log a metric that doesn't change over time such as a final evaluation accuracy?

Using `wandb.log({'final_accuracy': 0.9}` will work fine for this. By default `wandb.log({'final_accuracy'})` will update `wandb.settings['final_accuracy']`, which is the value shown in the runs table.

### How can I log additional metrics after a run completes?

There are several ways to do this.

For complicated workflows, we recommend using multiple runs and setting group parameter in [`wandb.init`](../track/launch.md) to a unique value in all the processes that are run as part of a single experiment. The [runs table](../../ref/app/pages/run-page.md) will automatically group the table by the group ID and the visualizations will behave as expected. This will allow you to run multiple experiments and training runs as separate processes log all the results into a single place.

For simpler workflows, you can call `wandb.init` with `resume=True` and `id=UNIQUE_ID` and then later call `wandb.init` with the same `id=UNIQUE_ID`. Then you can log normally with [`wandb.log`](../track/log/) or `wandb.summary` and the runs values will update.

At any point you can always use the [API](https://docs.wandb.ai/library/public-api-guide#update-metrics-for-a-run-after-the-run-has-finished) to add additional evaluation metrics.

### What is the difference between `.log()` and `.summary`?

The summary is the value that shows in the table while log will save all the values for plotting later.

For example you might want to call `wandb.log` every time the accuracy changes. Usually you can just use .log. `wandb.log()` will also update the summary value by default unless you have set summary manually for that metric

The scatterplot and parallel coordinate plots will also use the summary value while the line plot plots all of the values set by .log

The reason we have both is that some people like to set the summary manually because they want the summary to reflect for example the optimal accuracy instead of the last accuracy logged.

### How do I install the wandb Python library in environments without gcc?

If you try to install `wandb` and see this error:

```
unable to execute 'gcc': No such file or directory
error: command 'gcc' failed with exit status 1
```

You can install `psutil` directly from a pre-built wheel. Find your Python version and OS here: [https://pywharf.github.io/pywharf-pkg-repo/psutil](https://pywharf.github.io/pywharf-pkg-repo/psutil)

For example, to install `psutil` on Python 3.8 in Linux:

```bash
WHEEL_URL=https://github.com/pywharf/pywharf-pkg-repo/releases/download/psutil-5.7.0-cp38-cp38-manylinux2010_x86_64.whl/psutil-5.7.0-cp38-cp38-manylinux2010_x86_64.whl#sha256=adc36dabdff0b9a4c84821ef5ce45848f30b8a01a1d5806316e068b5fd669c6d
pip install $WHEEL_URL
```

After `psutil` has been installed, you can install wandb with `pip install wandb`

### How does wandb stream logs and writes to disk?

W\&B queues in memory but also [write the events to disk](https://github.com/wandb/client/blob/7cc4dd311f3cdba8a740be0dc8903075250a914e/wandb/sdk/internal/datastore.py) asynchronously to handle failures and for the `WANDB_MODE=offline` case where you can sync the data after it's been logged.

In your terminal, you can see a path to the local run directory. This directory will contain a `.wandb` file that is the datastore above. If you're also logging images, we write them to `media/images` in that directory before uploading them to cloud storage.

### How to get multiple charts with different selected runs?

With wandb reports the procedure is as follows:

* Have multiple panel grids.
* Add filters to filter the run sets of each panel grid. This will help in selecting the runs that you want to portray in the respective panels.
* Create the charts you want in the panel grids.

### How is access to the API controlled?

For simplicity W\&B uses API keys for authorization when accessing the API. You can find your API keys in your [settings](https://app.wandb.ai/settings). Your API key should be stored securely and never checked into version control. In addition to personal API keys, you can add Service Account users to your team.

### What is a service account, and why is it useful?

A service account is an API key that has permissions to write to your team, but is not associated with a particular user. Among other things, service accounts are useful for tracking automated jobs logged to wandb, like periodic retraining, nightly builds, and so on. If you'd like, you can associate a username with one of these machine-launched runs with the [environment variable](../track/advanced/environment-variables.md) `WANDB_USERNAME`.

You can get the API key in your Team Settings page `/teams/<your-team-name>` where you invite new team members. Select service and click create to add a service account.

![Create a service account on your team settings page for automated jobs](<../../.gitbook/assets/image (167).png>)

### How can I rotate or revoke access?

Both personal and service account keys can be rotated or revoked. Simply create a new API Key or Service Account user and reconfigure your scripts to use the new key. Once all processes are reconfigured, you can remove the old API key from your profile or team.

### How do I switch between accounts on the same machine?

If you have two W\&B accounts working from the same machine, you'll need a nice way to switch between your different API keys. You can store both API keys in a file on your machine then add code like the following to your repos. This is to avoid checking your secret key into a source control system, which is potentially dangerous.

```python
if os.path.exists("~/keys.json"):
   os.environ["WANDB_API_KEY"] = json.loads("~/keys.json")["work_account"]
```

### Does the W\&B client support Python 2? <a href="#eol-python27" id="eol-python27"></a>

The W\&B client library supported both Python 2.7 and Python 3 through version 0.10. Due to the Python 2 end of life, support for Python 2.7 was discontinued as of version 0.11. Users who run`pip install --upgrade wandb` on a Python 2.7 system will get new releases of the 0.10.x series only. Support for the 0.10.x series will be limited to critical bugfixes and patches. Currently, version 0.10.33 is the last version of the 0.10.x series that supports Python 2.7.

### Does the W\&B client support Python 3.5? <a href="#eol-python35" id="eol-python35"></a>

The W\&B client library supported both Python 3.5 through version 0.11. Due to the Python 3.5 end of life, support was discontinued as of version 0.12.

### Is there a dark mode?

Yes, this is accessible by going into your profile by clicking on your icon on the top right of your dashboard. Under your profile name, you can type in `night` where it says `I teach robots how to learn`. &#x20;
