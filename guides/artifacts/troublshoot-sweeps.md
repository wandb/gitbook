# Troublshoot sweeps

Follow the instructions below to resolve common errors and warnings.

### `Run does not exist`

If a `Run does not exist` and `ERROR Error uploading` is returned, you might be setting an ID for your run. For example:

```
wandb.init(id="some-string")
```

This ID needs to be unique in the project. If the ID is not unique the error above will be thrown. In the context of sweeps, you can not set a manual ID for your runs because Weights & Biases automatically generating random, unique IDs for the runs.

If you are trying to get a custom name to appear in the table and on graphs, we recommend using `name` instead of `id.` For example:

```python
wandb.init(name="a helpful readable run name")
```

### `Cuda out of memory`

Refactoring the code to use process-based executions should address this issue. Assuming you can rewrite your code to a file named `train.py`, then the following should work:

1. Add the `program` key to your sweep config, for example, `program: train.py`
2. Add the following to the end of your script:

```
if _name_ == "_main_":
    train()
```

Instead of calling `wandb.agent(...)` inside of python, call it from the command line:

```bash
 wandb agent SWEEP_ID
```

This will launch each trial in a separate process. This should guarantee you do not hold onto unnecessary memory.

### `WARNING Ignoring project`

If you get this warning:

```
wandb: WARNING Ignoring project='speech-reconstruction-baseline' passed \
to wandb.init when running a sweep
```

then your `wandb.init` call includes the `project` argument. This is invalid because both the sweep and the run must be in the same project. The project is defined when you initialize a sweep. For more information about how to initialize a sweep, see Initialize a sweep\[LINK].&#x20;

### Agents stop after the first run finishes

If the error message is a `400` code from the W\&B `anaconda` API, like the proceeding code snippet:

```
wandb: ERROR Error while calling W&B API: anaconda 400 error: \
{"code": 400, "message": "TypeError: bad operand type for unary -: 'NoneType'"}
```

then most likely the `metric` you are optimizing in your configuration YAML file is not a metric that you are logging.&#x20;

For example, you might be optimizing the metric `f1`, but you log `validation_f1`.

```
wandb.log({'validation f1', validation_f1})
```

Instead, you should log:

```
wandb.log({'validation f1', f1})
```

&#x20;Ensure that you log the _exact_ metric name that you defined the sweep to optimize
