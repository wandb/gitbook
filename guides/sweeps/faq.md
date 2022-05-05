# Common Questions

## Do I need to provide values for all hyperparameters as part of the sweep, or can I set defaults?

The hyperparameter names and values specified as part of the sweep configuration are accessible in `wandb.config`, a dictionary-like object.

For runs that are not part of a sweep, the values of `wandb.config` are usually set by providing a dictionary to the `config` argument of `wandb.init`. During a sweep, however, any configuration information passed to `wandb.init` is instead treated as a default value, which might be over-ridden by the sweep.

You can also be more explicit about the intended behavior by using `config.setdefaults`. Code snippets for both methods appear below:

{% tabs %}
{% tab title="wandb.init" %}
```python
# set default values for hyperparameters
config_defaults = {"lr": 0.1, "batch_size": 256}

# start a run, providing defaults
#   that can be over-ridden by the sweep
with wandb.init(config=config_default) as run:
    # add your training code here
```
{% endtab %}

{% tab title="config.setdefaults" %}
```python
# set default values for hyperparameters
config_defaults = {"lr": 0.1, "batch_size": 256}

# start a run
with wandb.init() as run:
    # update any values not set by sweep
    run.config.setdefaults(config_defaults)
    
    # add your training code here
```
{% endtab %}
{% endtabs %}

## Why are my sweep agents running forever? Is there a way to set a maximum number of runs?

Random and Bayesian searches will run forever -- until you stop the process from the command line or [the UI](../../ref/app/features/sweeps.md). You can set a target to automatically stop the sweep when it achieves a certain value for a metric, or you can specify the number of runs an agent should try:

{% tabs %}
{% tab title="Command Line" %}
```python
NUM=10
SWEEPID="dtzl1o7u"
wandb agent --count $NUM $SWEEPID
```
{% endtab %}

{% tab title="Python" %}
```python
sweep_id, count = "dtzl1o7u", 10
wandb.agent(sweep_id, count)
```
{% endtab %}
{% endtabs %}

## How do I set the project and entity where the sweep is logged?

Every sweep is associated with an `entity` (a user or a team) and a `project`.

These values can be set in four ways: as command-line arguments to [`wandb sweep`](../../ref/cli/wandb-sweep.md), as part of the [sweep configuration](configuration.md) YAML file, as [environment variables](../track/advanced/environment-variables.md), or via the `wandb/settings` file.

{% tabs %}
{% tab title="CLI" %}
```python
wandb sweep --entity geoff --project capsules
```
{% endtab %}

{% tab title="sweep_config.yaml" %}
```python
# inside of sweep_config.yaml
entity: geoff
project: capsules
```
{% endtab %}

{% tab title="Environment Variables" %}
```python
# in the shell
WANDB_ENTITY="geoff"
WANDB_PROJECT="capsules"

# pure Python
import os
os.environ[WANDB_ENTITY] = "geoff"
os.environ[WANDB_PROJECT] = "capsules"

# IPython/Jupyter
%env WANDB_ENTITY="geoff"
%env WANDB_PROJECT="capsules"
```
{% endtab %}

{% tab title="wandb/settings" %}
```
[default]
entity: geoff
project: capsules
```
{% endtab %}
{% endtabs %}

## What's with this warning about ignoring the project? Why's my sweep not logging where I expect it?

If you get this warning:

`wandb: WARNING Ignoring project='speech-reconstruction-baseline' passed to wandb.init when running a sweep`

then your `wandb.init` call includes the `project` argument. That's invalid because sweep and the runs have to be in the same project. The project is set during sweep creation, e.g. by`wandb.sweep(sweep_config, project="cat-detector")`

## Why are my agents stopping after the first run finishes?

If the error message is a `400` code from the W\&B `anaconda` API, like this one:

`wandb: ERROR Error while calling W&B API: anaconda 400 error: {"code": 400, "message": "TypeError: bad operand type for unary -: 'NoneType'"}`

then the most likely reason is that the `metric` you are optimizing in your configuration YAML file is not a metric that you are logging. For example, you could be optimizing the metric `f1`, but logging `validation_f1`. Double-check that you're logging the _exact_ metric name that you've asked the sweep to optimize.

## How should I run sweeps on SLURM?

When using sweeps with the [SLURM scheduling system](https://slurm.schedmd.com/documentation.html), we recommend running `wandb agent --count 1 SWEEP_ID` in each of your scheduled jobs, which will run a single training job and then exit. This makes it easier to predict runtimes when requesting resources and takes advantage of the parallelism of hyperparameter search.

## Can I rerun a grid search?

Yes! If you exhaust a grid search but want to rerun some of the runs (for example because some crashed), you can delete the ones you want to rerun, then hit the resume button on the [sweep control page](../../ref/app/features/sweeps.md), then start new agents for that sweep ID. Parameter combinations with completed runs will not be retried.

## What do I do if I get the error message `CommError, Run does not exist`?

If you're seeing that error message, plus `ERROR Error uploading`, you might be setting an ID for your run, e.g. `wandb.init(id="some-string")` . This ID needs to be unique in the project, and if it's not unique, the error above will be thrown. In the context of sweeps, you can't set a manual ID for your runs because we're automatically generating random, unique IDs for the runs.

If you're trying to get a nice name to show up in the table and on the graphs, we recommend using `name` instead of `id.` For example:

```python
wandb.init(name="a helpful readable run name")
```

## What do I do if I get the error message `Cuda out of memory`?

Refactoring the code to use process-based executions should address this issue. Assuming you can rewrite your code to a file named `train.py`, then the following should work:

1. Add the `program` key to your sweep config, for example, `program: train.py`
2. Add the following to the end of your script:

```
if _name_ == "_main_":
    train()
```

Then instead of calling `wandb.agent(...)` inside of python, call it from the command line: `wandb agent SWEEP_ID`. This will launch each trial in a separate process which should guarantee you don't hold onto any memory.

## How do I use custom commands with sweeps?

If you normally configure some aspects of training by passing command line arguments, for example:

```bash
/usr/bin/env python edflow.py -b \
    your-training-config \
    --batchsize 8 \
    --lr 0.00001
```

you can still use sweeps. You just need to edit the `command` key in the YAML file, like so:

```yaml
program:
  edflow.py
method: grid
parameters:
  batch_size:
    value: 8
  lr:
    value: 0.0001
command:
  - ${env}
  - python
  - ${program}
  - "-b"
  - your-training-config
  - ${args}
```

The `${args}` key expands to all the parameters in the sweep configuration file, expanded so they can be parsed by `argparse: --param1 value1 --param2 value2`

If you have extra arguments that you don't want to specify with `argparse` you can use:

```python
parser = argparse.ArgumentParser()
args, unknown = parser.parse_known_args()
```

{% hint style="info" %}
Depending on the environment, `python` might point to Python 2. To ensure Python 3 is invoked, just use `python3` instead of `python` when configuring the command:

```yaml
program:
  script.py
command:
  - ${env}
  - python3
  - ${program}
  - ${args}
```
{% endhint %}

## How does the Bayesian search work?

The Gaussian process model that's used for Bayesian optimization is defined in our [open source sweep logic](https://github.com/wandb/client/blob/master/wandb/sweeps/bayes\_search.py). If you'd like extra configurability and control, try our support for [Ray Tune](https://docs.wandb.com/sweeps/ray-tune).

We use scikit-learn's [Matern kernel](https://scikit-learn.org/stable/modules/generated/sklearn.gaussian\_process.kernels.Matern.html) with the `nu` parameter set to `1.5` -- this corresponds to much a weaker smoothness assumption than for the radial basis function (RBF) kernel. For details on kernels in Gaussian processes, see [Chapter 4 of Rasmussen and Williams](http://www.gaussianprocess.org/gpml/chapters/RW4.pdf) or the scikit-learn docs linked above.

## What's the difference between "stopping" and "pausing" a sweep? Why isn't the `wandb agent` command terminating when I pause the sweep?

"Stopping" a sweep in the [Sweeps UI](../../ref/app/features/sweeps.md) indicates that the hyperparameter search is over. "Pausing" it merely means that new jobs should not be launched until the sweep is resumed.

If you stop the sweep instead of pausing it, then the agents will exit -- their work is done. If the sweep is merely paused, the agents will stay running in case the sweep is resumed.

## Is there a way to add extra values to a sweep, or do I need to start a new one?

Once a sweep has started you cannot change the sweep configuration. But you can go to any table view, and use the checkboxes to select runs, then use the "create sweep" menu option to create a new sweep configuration using prior runs.

## Can we flag boolean variables as hyperparameters?

To pass hyperparameters as boolean flags you can use the `${args_no_boolean_flags}` macro in the [command section of the config](configuration.md#command). This will automatically pass in any boolean parameters as flags: when `param` is `True` the command will receive `--param`, when `param` is `False` the flag will be omitted.&#x20;

## How to use sweeps with cloud infrastructures such as AWS Batch, ECS, etc.?

In general, you would need a way to publish `sweep_id` to a place that any potential agent can read and a way for these agents to consume this `sweep_id` and start running.

In other words, you would need something that can invoke `wandb agent`. For instance, bring up an EC2 instance and then call `wandb agent` on it. In this case, you might use an SQS queue to broadcast `sweep_id` to a few EC2 instances and then have them consume the `sweep_id` from the queue and start running.

## How can I change the directory my sweep logs to locally?

You can change the path of the directory where W\&B will log your run data by setting an environment variable `WANDB_DIR`. For example:

```python
os.environ["WANDB_DIR"] = os.path.abspath("your/directory")
```

## Optimizing multiple metrics

If you want to optimize multiple metrics in the same run, you can use a weighted sum of the individual metrics.

```python
metric_combined = 0.3*metric_a + 0.2*metric_b + ... + 1.5*metric_n
wandb.log({"metric_combined": metric_combined})
```

Remember to log your new combined metric and set it as the optimization objective:

```yaml
metric:
  name: metric_combined
  goal: minimize
```

## How to launch agents on multiple GPUs?

You can launch sweep agents on multiple GPUs.

* First specify the hyperparameters you’re sweeping over in a YAML file, as detailed further in the [sweep docs](https://docs.wandb.com/sweeps).
* Get the sweep id by running the wandb sweep command and passing the YAML file as an argument
*   Run the wandb agent with the sweep id you just got. You will also need to specify the GPU like this:

    ```
     CUDA_VISIBLE_DEVICES=0 wandb agent sweep_id
     CUDA_VISIBLE_DEVICES=1 wandb agent sweep_id
    ```

## How do I enable code logging with Sweeps?

To enable code logging for sweeps, simply add `wandb.log_code()` after you have initialised your wandb run. This is necessary even when you have enabled code logging in the settings page of your W\&B profile in the app. For more advanced code logging, see the [docs for `wandb.log_code()` here](https://docs.wandb.ai/ref/python/run#log\_code)

