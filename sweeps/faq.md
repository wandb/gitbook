# Common Questions

## Setting the project and entity

When you run the command to start a sweep, you have to give that command a project and entity. If you want a different one it can be specified in 4 ways:

1. command line arguments to `wandb sweep` \(`--project` and `--entity` arguments\)
2. wandb settings file \(`project` and `entity` keys\)
3. [sweep configuration](configuration.md) \(`project` and `entity` keys\) 
4. [environment variables](../library/environment-variables.md) \(`WANDB_PROJECT` and `WANDB_ENTITY` variables\)

## **Sweeps agents stop after the first runs finish**

`wandb: ERROR Error while calling W&B API: anaconda 400 error: {"code":400,"message":"TypeError: bad operand type for unary -: 'NoneType'"}`

One common reason for this is that the metric your are optimizing in your configuration YAML file is not a metric that you are logging. For example, you could be optimizing the metric **f1**, but logging **validation\_f1**. Double check that you're logging the exact metric name that you're optimizing.

## Set a number of runs to try

Random search will run forever until you stop the sweep. You can set a target to automatically stop the sweep when it achieves a certain value for a metric, or you can specify the number of runs an agent should try: `wandb agent --count NUM SWEEPID`

wandb agent --count NUM SWEEPID

## Run a sweep on Slurm

We recommend running `wandb agent --count 1 SWEEP_ID` which will run a single training job and then exit.

## Rerun grid search

If you exhaust a grid search but want to rerun some of the runs, you can delete the ones you want to rerun, then hit the resume button on the sweep control page, then start new agents for that sweep ID.

## Sweeps and Runs must be in the same project

`wandb: WARNING Ignoring project='speech-reconstruction-baseline' passed to wandb.init when running a sweep`

You cant set a project with wandb.init\(\) when running a sweep. The sweep and the runs have to be in the same project, so the project is set by the sweep creation: wandb.sweep\(sweep\_config, project=“fdsfsdfs”\)

## Error uploading

If you're seeing **ERROR Error uploading &lt;file&gt;: CommError, Run does not exist**, you might be setting an ID for your run, `wandb.init(id="some-string")` . This ID needs to be unique in the project, and if it's not unique, it will throw and error. In the sweeps context, you can't set a manual ID for your runs because we're automatically generating random, unique IDs for the runs.

If you're trying to get a nice name to show up in the table and on the graphs, we recommend using **name** instead of **id**. For example:

```python
wandb.init(name="a helpful readable run name")
```

## Sweep with custom commands

If you normally run training with a command and arguments, for example:

```text
edflow -b <your-training-config> --batch_size 8 --lr 0.0001
```

You can convert this to a sweeps config like so:

```text
program:
  edflow
command:
  - ${env}
  - python
  - ${program}
  - "-b"
  - your-training-config
  - ${args}
```

The ${args} key expands to all the parameters in the sweep configuration file, expanded so they can be parsed by argparse: --param1 value1 --param2 value2

If you have extra args that you dont want to specify with argparse you can use:  
parser = argparse.ArgumentParser\(\)  
args, unknown = parser.parse\_known\_args\(\)

**Running Sweeps with Python 3**

If you're having an issue where the sweep is trying to use Python 2, it's easy to specify that it should use Python 3 instead. Just add this to your sweep config YAML file:

```text
program:
  script.py
command:
  - ${env}
  - python3
  - ${program}
  - ${args}
```

\*\*\*\*

## Bayesian optimization details

The Gaussian process model that's used for Bayesian optimization is defined in our [open source sweep logic](https://github.com/wandb/client/tree/master/wandb/sweeps). If you'd like extra configurability and control, try our support for [Ray Tune](https://docs.wandb.com/sweeps/ray-tune).

We use a [Matern kernel](https://scikit-learn.org/stable/modules/generated/sklearn.gaussian_process.kernels.Matern.html) which is a generalization of RBF— defined in our open source code [here](https://github.com/wandb/client/blob/541d760c5cb8776b1ad5fcf1362d7382811cbc61/wandb/sweeps/bayes_search.py#L30).

## Pausing sweeps vs. Stopping Sweeps wandb.agent

Is there anyway to get `wandb agent` to terminate when there are no more jobs available because I've paused a sweep?

If you stop the sweep instead of pausing it, then the agents will exit. For pause we want the agents to stay running so that the sweep can be restarted without having to launch agents again.

## Recommended way to set up config parameters in a sweep

`wandb.init(config=config_dict_that_could_have_params_set_by_sweep)`  
or:  
`experiment = wandb.init()    
experiment.config.setdefaults(config_dict_that_could_have_params_set_by_sweep)`

The advantage of doing this is that it will ignore setting any key that has already been set by the sweep.

## Is there a way to add an extra categorical value to a sweep, or do I need to start a new one?

Once a sweep has started you cannot change the sweep configuration, But you can go to any table view, and use the checkboxes to select runs, then use the "create sweep" menu option to a create a new sweep using prior runs

