# Define sweep configuration

A Weights & Biases Sweep combines a strategy for exploring hyperparameter values with the code that evaluates them. The strategy can be as simple as trying every option or as complex as Bayesian Optimization and Hyperband ([BOHB](https://arxiv.org/abs/1807.01774)).

Define your strategy in the form of a sweep configuration. Specify the configuration either in a:

1. Python nested dictionary data structure if you use a Jupyter Notebook or Python script.
2. YAML file if you use the command line (CLI).

The following code snippets demonstrate examples of how to define a Sweep configuration in a Jupyter Notebook or Python script or within a YAML file. Configuration keys are defined in detail in subsequent sections.

{% tabs %}
{% tab title="Python script or Jupyter Notebook" %}
Within your Jupyter Notebook or Python script, define a sweep in a dictionary Python data structure.

```python
sweep_configuration = {
    'method': 'random',
    'name': 'sweep',
    'metric': {
        'goal': 'minimize', 
        'name': 'validation_loss'
		},
    'parameters': {
        'batch_size': {'values': [16, 32, 64]},
        'epochs': {'values': [5, 10, 15]},
        'lr': {'max': 0.1, 'min': 0.0001}
     }
}
```
{% endtab %}

{% tab title="YAML" %}
Create a map in your YAML where the keys can have, as their values, further keys.

```yaml
program: train.py
method: bayes
metric:
  name: validation_loss
  goal: minimize
parameters:
  learning_rate:
    min: 0.0001
    max: 0.1
  optimizer:
    values: ["adam", "sgd"]
```
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
1. Ensure that you log (`wandb.log`) the _exact_ metric name that you defined the sweep to optimize within your Python script or Jupyter Notebook.
2. You cannot change the Sweep configuration once you start the W\&B Sweep agent.
{% endhint %}

For example, suppose you want W\&B Sweeps to maximize the validation accuracy during training. Within your Python script you store the validation accuracy in a variable `validation_loss`. In your YAML configuration file you define this as:

```yaml
metric:
  goal: maximize
  name: validation_loss
```

You must log the variable `validation_loss` (in this example) within your Python script or Jupyter Notebook to W\&B.

```python
wandb.log({
        'val_loss': validation_loss
      })
```

### Sweep configuration structure

Sweep configurations are nested; keys can have, as their values, further keys. The top-level keys are listed and briefly described below, and then detailed in the following section.

| Top-Level Key     | Description                                                                                                                   |
| ----------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| `program`         | (required) Training script to run.                                                                                            |
| `method`          | (required) Specify the [search strategy](define-sweep-configuration.md#configuration-keys).                                   |
| `parameters`      | (required) Specify [parameters](define-sweep-configuration.md#parameters) bounds to search.                                   |
| `name`            | The name of the sweep, displayed in the W\&B UI.                                                                              |
| `description`     | Text description of the sweep.                                                                                                |
| `metric`          | Specify the metric to optimize (only used by certain search strategies and stopping criteria).                                |
| `early_terminate` | Specify any [early stopping criteria](define-sweep-configuration.md#early\_terminate).                                        |
| `command`         | Specify [command structure ](define-sweep-configuration.md#command)for invoking and passing arguments to the training script. |
| `project`         | Specify the project for this sweep.                                                                                           |
| `entity`          | Specify the entity for this sweep.                                                                                            |

### Search type methods

The following list describes hyperparameter search methods. Specify the search strategy with the `method`:

* **`grid`**  – Iterate over every combination of hyperparameter values. Can be computationally costly.
* **`random`**  – Choose a random set of hyperparameter values on each iteration based on provided distributions.
* **`bayes`** – Create a probabilistic model of a metric score as a function of the hyperparameters, and choose parameters with high probability of improving the metric. Bayesian hyperparameter search method uses a Gaussian Process to model the relationship between the parameters and the model metric and chooses parameters to optimize the probability of improvement. This strategy requires the `metric`key to be specified. Works well for small numbers of continuous parameters but scales poorly.

{% tabs %}
{% tab title="Random search" %}
```yaml
method: random
```
{% endtab %}

{% tab title="Grid search" %}
```yaml
method: grid
```
{% endtab %}

{% tab title="Bayes search" %}
```yaml
method: bayes
metric:
  name: val_loss
  goal: minimize
```
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
Random and Bayesian searches will run forever -- until you stop the process from the command line, within your python script, or [the UI](../../ref/app/features/sweeps.md). Grid search will also run forever if it searches within in a continuous search space.
{% endhint %}

## Configuration keys

### `method`

Specify the search strategy with the `method` key in the sweep configuration.

| `method` | Description                                                                                                                                                                                                                                                         |
| -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `grid`   | Grid search iterates over all possible combinations of parameter values.                                                                                                                                                                                            |
| `random` | Random search chooses a random set of values on each iteration.                                                                                                                                                                                                     |
| `bayes`  | Our Bayesian hyperparameter search method uses a Gaussian Process to model the relationship between the parameters and the model metric and chooses parameters to optimize the probability of improvement. This strategy requires the `metric` key to be specified. |

### `parameters`

Describe the hyperparameters to explore during the sweep. For each hyperparameter, specify the name and the possible values as a list of constants (for any `method`) or specify a `distribution` for `random` or `bayes`.

| Values          | Description                                                                                                                                                                                                                                                                          |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `values`        | Specifies all valid values for this hyperparameter. Compatible with `grid`.                                                                                                                                                                                                          |
| `value`         | Specifies the single valid value for this hyperparameter. Compatible with `grid`.                                                                                                                                                                                                    |
| `distribution`  | (`str`) Selects a distribution from the distribution table below. If not specified, will default to `categorical` if `values` is set, to `int_uniform` if `max` and `min` are set to integers, to `uniform` if `max` and `min` are set to floats, or to`constant` if `value` is set. |
| `probabilities` | Specify the probability of selecting each element of `values` when using `random`.                                                                                                                                                                                                   |
| `min`, `max`    | (`int`or `float`) Maximum and minimum values. If `int`, for `int_uniform` -distributed hyperparameters. If `float`, for `uniform` -distributed hyperparameters.                                                                                                                      |
| `mu`            | (`float`) Mean parameter for `normal` - or `lognormal` -distributed hyperparameters.                                                                                                                                                                                                 |
| `sigma`         | (`float`) Standard deviation parameter for `normal` - or `lognormal` -distributed hyperparameters.                                                                                                                                                                                   |
| `q`             | (`float`) Quantization step size for quantized hyperparameters.                                                                                                                                                                                                                      |
| `parameters`    | Nest other parameters inside a root level parameter. It's parameter inception!                                                                                                                                                                                                       |

#### **Examples**

{% tabs %}
{% tab title="single value" %}
```yaml
parameter_name:
  value: 1.618
```
{% endtab %}

{% tab title="multiple values" %}
```yaml
parameter_name:
  values:
  - 8
  - 6
  - 7
  - 5
  - 3
  - 0
  - 9
```
{% endtab %}

{% tab title="probabilities" %}
```yaml
parameter_name:
    values: [1, 2, 3, 4, 5]
    probabilities: [0.1, 0.2, 0.1, 0.25, 0.35]
```
{% endtab %}

{% tab title="distribution" %}
```yaml
parameter_name:
  distribution: normal
  mu: 100
  sigma: 10
```
{% endtab %}

{% tab title="nested" %}
```yaml
optimizer:
    parameters:
        learning_rate:
            values: [0.01, 0.001]
        momentum:
            value: 0.9
```
{% endtab %}
{% endtabs %}

#### `distribution`

Specify how to distribute values if you choose a random (`random)` or Bayesian (`bayes)` search method.&#x20;

| Value                    | Description                                                                                                                                                                              |
| ------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `constant`               | Constant distribution. Must specify `value`.                                                                                                                                             |
| `categorical`            | Categorical distribution. Must specify `values`.                                                                                                                                         |
| `int_uniform`            | Discrete uniform distribution on integers. Must specify `max` and `min` as integers.                                                                                                     |
| `uniform`                | Continuous uniform distribution. Must specify `max` and `min` as floats.                                                                                                                 |
| `q_uniform`              | Quantized uniform distribution. Returns `round(X / q) * q` where X is uniform. `q` defaults to `1`.                                                                                      |
| `log_uniform`            | Log-uniform distribution. Returns a value `X` between `exp(min)` and `exp(max)`such that the natural logarithm is uniformly distributed between `min` and `max`.                         |
| `inv_log_uniform`        | Inverse log uniform distribution. Returns `X` , where `log(1/X)` is uniformly distributed between `min` and `max` .                                                                      |
| `log_uniform_values`     | Log-uniform distribution. Returns a value `X` between `min` and `max` such that `log(`X`)` is uniformly distributed between `log(min)` and `log(max)`.                                   |
| `q_log_uniform`          | Quantized log uniform. Returns `round(X / q) * q` where `X` is `log_uniform`. `q` defaults to `1`.                                                                                       |
| `q_log_uniform_values`   | Quantized log uniform. Returns `round(X / q) * q` where `X` is `log_uniform_values`. `q` defaults to `1`.                                                                                |
| `inv_log_uniform`        | Inverse log uniform distribution. Returns `X`, where  `log(1/X)` is uniformly distributed between `min` and `max`.                                                                       |
| `inv_log_uniform_values` | Inverse log uniform distribution. Returns `X`, where  `log(1/X)` is uniformly distributed between `log(1/max)` and `log(1/min)`.                                                         |
| `normal`                 | Normal distribution. Return value is normally-distributed with mean `mu` (default `0`) and standard deviation `sigma` (default `1`).                                                     |
| `q_normal`               | Quantized normal distribution. Returns `round(X / q) * q` where `X` is `normal`. Q defaults to 1.                                                                                        |
| `log_normal`             | Log normal distribution. Returns a value `X` such that the natural logarithm `log(X)` is normally distributed with mean `mu` (default `0`) and standard deviation `sigma` (default `1`). |
| `q_log_normal`           | Quantized log normal distribution. Returns `round(X / q) * q` where `X` is `log_normal`. `q` defaults to `1`.                                                                            |

#### **Examples**

{% tabs %}
{% tab title="constant" %}
```yaml
parameter_name:
  distribution: constant
  value: 2.71828
```
{% endtab %}

{% tab title="categorical" %}
```yaml
parameter_name:
  distribution: categorical
  values:
  - elu
  - celu
  - gelu
  - selu
  - relu
  - prelu
  - lrelu
  - rrelu
  - relu6
```
{% endtab %}

{% tab title="uniform" %}
```yaml
parameter_name:
  distribution: uniform
  min: 0
  max: 1
```
{% endtab %}

{% tab title="q_uniform" %}
```yaml
parameter_name:
  distribution: q_uniform
  min: 0
  max: 256
  q: 1
```
{% endtab %}
{% endtabs %}

### **`metric`**

Describes the metric to optimize. This metric should be logged explicitly to W\&B by your training script.

| Key      | Description                                                                                                                                                                                                                                                   |
| -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `name`   | Name of the metric to optimize.                                                                                                                                                                                                                               |
| `goal`   | Either `minimize` or `maximize` (Default is `minimize`).                                                                                                                                                                                                      |
| `target` | Goal value for the metric you're optimizing. When any run in the sweep achieves that target value, the sweep's state will be set to `finished`. This means all agents with active runs will finish those jobs, but no new runs will be launched in the sweep. |

&#x20;For example, if you want to minimize the validation loss of your model:

```python
# model training code that returns validation loss as valid_loss
wandb.log({"val_loss" : valid_loss})
```

#### **Examples**

{% tabs %}
{% tab title="Maximize" %}
```yaml
metric:
  name: val_acc
  goal: maximize
```
{% endtab %}

{% tab title="Minimize" %}
```yaml
metric:
  name: val_loss
  goal: minimize
```
{% endtab %}

{% tab title="Target" %}
```yaml
metric:
  name: val_acc
  goal: maximize
  target: 0.95
```
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
The metric you optimize must be a top-level metric.&#x20;
{% endhint %}

Do not log the metric for your sweep inside of a sub-directory. In the proceeding code example, we want to log the validation loss (`"loss": val_loss`). First we define it in a dictionary. However, the dictionary passed to `wandb.log` does not specify the key-value pair to track.

```
val_metrics = {
        "loss": val_loss, 
        "acc": val_acc
        }

# Incorrect. Dictionary key-value paired is not provided.
wandb.log({"val_loss", val_metrics})
```

Instead, log the metric at the top level. For example, after you create a dictionary, specify the key-value pair when you pass the dictionary to the `wandb.log` method:

```
val_metrics = {
        "loss": val_loss, 
        "acc": val_acc
        }

wandb.log({"val_loss", val_metrics["loss"]})
```

### `early_terminate`

Early termination is an optional feature that speeds up hyperparameter search by stopping poorly-performing runs. When the early stopping is triggered, the agent stops the current run and gets the next set of hyperparameters to try.

| Key    | Description                    |
| ------ | ------------------------------ |
| `type` | Specify the stopping algorithm |

We support the following stopping algorithm(s):

| `type`      | Description                                                   |
| ----------- | ------------------------------------------------------------- |
| `hyperband` | Use the [hyperband method](https://arxiv.org/abs/1603.06560). |

#### **`hyperband`**

[Hyperband](https://arxiv.org/abs/1603.06560) stopping evaluates if a program should be stopped or permitted to continue at one or more pre-set iteration counts, called "brackets". When a run reaches a bracket, its metric value is compared to all previous reported metric values and the [W\&B Run](https://docs.wandb.ai/ref/python/run) is terminated if its value is too high (when the goal is minimization) or low (when the goal is maximization).

Brackets are based on the number of logged iterations. The number of brackets corresponds to the number of times you log the metric you are optimizing. The iterations can correspond to steps, epochs, or something in between. The numerical value of the step counter is not used in bracket calculations.

{% hint style="danger" %}
Specify either `min_iter` or `max_iter` to create a bracket schedule.
{% endhint %}

| Key        | Description                                                    |
| ---------- | -------------------------------------------------------------- |
| `min_iter` | Specify the iteration for the first bracket                    |
| `max_iter` | Specify the maximum number of iterations.                      |
| `s`        | Specify the total number of brackets (required for `max_iter`) |
| `eta`      | Specify the bracket multiplier schedule (default: `3`).        |

{% hint style="info" %}
The hyperband early terminator checks what [W\&B Runs](https://docs.wandb.ai/ref/python/run) to terminate once every few minutes. The end run timestamp might differ from the specified brackets if your run or iteration are short.
{% endhint %}

#### **Examples**

{% tabs %}
{% tab title="Hyperband (min_iter)" %}
```yaml
early_terminate:
  type: hyperband
  min_iter: 3
```

The brackets for this example are: `[3, 3*eta, 3*eta*eta, 3*eta*eta*eta]`, which equals `[3, 9, 27, 81]`.
{% endtab %}

{% tab title="Hyperband (max_iter)" %}
```yaml
early_terminate:
  type: hyperband
  max_iter: 27
  s: 2
```

The brackets for this example are `[27/eta, 27/eta/eta]`, which equals `[9, 3]`.
{% endtab %}
{% endtabs %}

### `command` <a href="#command" id="command"></a>

Agents created with [`wandb agent`](../../ref/cli/wandb-agent.md) receive a command in the following format by default:

{% tabs %}
{% tab title="UNIX" %}
```python
/usr/bin/env python train.py --param1=value1 --param2=value2
```
{% endtab %}

{% tab title="Windows" %}
```python
python train.py --param1=value1 --param2=value2
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
On UNIX systems, `/usr/bin/env` ensures the right Python interpreter is chosen based on the environment.
{% endhint %}

The format and contents can be modified by specifying values under the `command` key. Fixed components of the command, such as filenames, can be included directly (see examples below).

We support the following macros for variable components of the command:

| Command Macro              | Description                                                                                                                                                           |
| -------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `${env}`                   | `/usr/bin/env` on UNIX systems, omitted on Windows.                                                                                                                   |
| `${interpreter}`           | Expands to `python`.                                                                                                                                                  |
| `${program}`               | Training script filename specified by the sweep configuration `program` key.                                                                                          |
| `${args}`                  | Hyperparameters and their values in the form `--param1=value1 --param2=value2`.                                                                                       |
| `${args_no_boolean_flags}` | Hyperparameters and their values in the form `--param1=value1` except boolean parameters are in the form `--boolean_flag_param` when `True` and omitted when `False`. |
| `${args_no_hyphens}`       | Hyperparameters and their values in the form `param1=value1 param2=value2`.                                                                                           |
| `${args_json}`             | Hyperparameters and their values encoded as JSON.                                                                                                                     |
| `${args_json_file}`        | The path to a file containing the hyperparameters and their values encoded as JSON.                                                                                   |

The default command format is defined as:

```yaml
command:
  - ${env}
  - ${interpreter}
  - ${program}
  - ${args}
```

#### Examples

{% tabs %}
{% tab title="Set python interpreter" %}
Remove the `{$interpreter}` macro and provide a value explicitly in order to hardcode the python interpreter. For example, the following code snippet demonstrates how to do this:

```yaml
command:
  - ${env}
  - python3
  - ${program}
  - ${args}
```
{% endtab %}

{% tab title="Add extra parameters" %}
To add extra command line arguments not specified by sweep configuration parameters:

```
command:
  - ${env}
  - ${interpreter}
  - ${program}
  - "--config"
  - "your-training-config.json"
  - ${args}
```
{% endtab %}

{% tab title="Omit arguments" %}
If your program does not use argument parsing you can avoid passing arguments all together and take advantage of `wandb.init` picking up sweep parameters into `wandb.config` automatically:

```
command:
  - ${env}
  - ${interpreter}
  - ${program}
```
{% endtab %}

{% tab title="Use the Hydra" %}
You can change the command to pass arguments the way tools like [Hydra](https://hydra.cc) expect. See [Hydra with W\&B](../integrations/other/hydra.md) for more information.&#x20;

```
command:
  - ${env}
  - ${interpreter}
  - ${program}
  - ${args_no_hyphens}
```
{% endtab %}
{% endtabs %}
