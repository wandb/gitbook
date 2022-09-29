# FAQ

### Do I need to provide values for all hyperparameters as part of the W\&B Sweep. Can I set defaults?

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

### How should I run sweeps on SLURM?

When using sweeps with the [SLURM scheduling system](https://slurm.schedmd.com/documentation.html), we recommend running `wandb agent --count 1 SWEEP_ID` in each of your scheduled jobs, which will run a single training job and then exit. This makes it easier to predict runtimes when requesting resources and takes advantage of the parallelism of hyperparameter search.

### Can I rerun a grid search?

Yes. If you exhaust a grid search but want to re-execute some of the W\&B Runs (for example because some crashed). Delete the W\&B Runs ones you want to re-execute, then choose the **Resume** button on the [sweep control page](../../ref/app/features/sweeps.md). Finally, start new W\&B Sweep agents with the new Sweep ID.&#x20;

Parameter combinations with completed W\&B Runs are not re-executed.

### How do I use custom CLI commands with sweeps?

You can use W\&B Sweeps with custom CLI commands if you normally configure some aspects of training by passing command line arguments.&#x20;

For example, the proceeding code snippet demonstrates a bash terminal where the user is training a Python script named train.py. The user passes in values that are then parsed within the Python script:

```bash
/usr/bin/env python train.py -b \
    your-training-config \
    --batchsize 8 \
    --lr 0.00001
```

To use custom commands, edit the `command` key in your YAML file. For example, continuing the example above, that might look like so:

```yaml
program:
  train.py
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
Depending on the environment, `python` might point to Python 2. To ensure Python 3 is invoked, use `python3` instead of `python` when configuring the command:

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

### Is there a way to add extra values to a sweep, or do I need to start a new one?

You cannot change the Sweep configuration once a W\&B Sweep has started. But you can go to any table view, and use the checkboxes to select runs, then use the **Create sweep** menu option to create a new Sweep configuration using prior runs.

### Can we flag boolean variables as hyperparameters?

You can use the `${args_no_boolean_flags}` macro in the [command section of the config](broken-reference) to pass hyperparameters as boolean flags. This will automatically pass in any boolean parameters as flags. When `param` is `True` the command will receive `--param`, when `param` is `False` the flag will be omitted.&#x20;

### Can you use W\&B Sweeps with cloud infrastructures such as AWS Batch, ECS, etc.?

In general, you would need a way to publish `sweep_id` to a location that any potential W\&B Sweep agent can read and a way for these Sweep agents to consume this `sweep_id` and start running.

In other words, you need something that can invoke `wandb agent`. For instance, bring up an EC2 instance and then call `wandb agent` on it. In this case, you might use an SQS queue to broadcast `sweep_id` to a few EC2 instances and then have them consume the `sweep_id` from the queue and start running.

### How can I change the directory my sweep logs to locally?

You can change the path of the directory where W\&B will log your run data by setting an environment variable `WANDB_DIR`. For example:

```python
os.environ["WANDB_DIR"] = os.path.abspath("your/directory")
```

### Optimizing multiple metrics

If you want to optimize multiple metrics in the same run, you can use a weighted sum of the individual metrics.

```python
metric_combined = 0.3*metric_a + 0.2*metric_b + ... + 1.5*metric_n
wandb.log({"metric_combined": metric_combined})
```

Ensure to log your new combined metric and set it as the optimization objective:

```yaml
metric:
  name: metric_combined
  goal: minimize
```

### How do I enable code logging with Sweeps?

To enable code logging for sweeps, simply add `wandb.log_code()` after you have initialized your W\&B Run. This is necessary even when you have enabled code logging in the settings page of your W\&B profile in the app. For more advanced code logging, see the [docs for `wandb.log_code()` here](https://docs.wandb.ai/ref/python/run#log\_code).
