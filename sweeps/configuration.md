---
description: >-
  Syntax to set the hyperparameter ranges, search strategy, and other aspects of
  your sweep.
---

# Configuration

Use these configuration fields to customize your sweep. There are two ways to specify your configuration:

1. [YAML file](https://docs.wandb.com/sweeps/overview/quickstart#2-sweep-config): best for distributed sweeps
2. [Python data structure](python-api.md): best for running a sweep from a Jupyter Notebook 

| Top-level key | Meaning |
| :--- | :--- |
| name | The name of the sweep, displayed in the W&B UI |
| description | Text description of the sweep \(notes\) |
| program | Training script to run \(required\) |
| metric | Specify the metric to optimize \(used by some search strategies and stopping criteria\) |
| method | Specify the [search strategy](configuration.md#search-strategy) \(required\) |
| early\_terminate | Specify the [stopping criteria](configuration.md#stopping-criteria) |
| parameters | Specify [parameters](configuration.md#parameters) bounds to search \(required\) |
| project | Specify the project for this sweep |
| entity | Specify the entity for this sweep |
| command | Specify command line for how the training script should be run |

### Metric

Specify the metric to optimize. This metric should be logged explicitly to W&B by your training script. For example, if you want to minimize the validation loss of your model:

```python
# [model training code that returns validation loss as valid_loss]
wandb.log({"val_loss" : valid_loss})
```

| `metric` sub-key | Meaning |
| :--- | :--- |
| name | Name of the metric to optimize |
| goal | `minimize` or `maximize` \(Default is `minimize`\) |
| target | Value that you'd like to achieve for the metric you're optimizing. When any run in the sweep achieves that target value, the sweep's state will be set to "Finished." This means all agents with active runs will finish those jobs, but no new runs will be launched in the sweep. |

**Examples**

{% tabs %}
{% tab title="Maximize" %}
```text
metric:
  name: val_loss
  goal: maximize
```
{% endtab %}

{% tab title="Minimize" %}
```text
metric:
  name: val_loss
```
{% endtab %}

{% tab title="Target" %}
```
metric:
  name: val_loss
  goal: maximize
  target: 0.1
```
{% endtab %}
{% endtabs %}

### Search Strategy

Specify the search strategy with the `method` key in the sweep configuration.

| `method` | Meaning |
| :--- | :--- |
| grid | Grid search iterates over all possible combinations of parameter values. |
| random | Random search chooses random sets of values. |
| bayes | Bayesian Optimization uses a gaussian process to model the function and then chooses parameters to optimize probability of improvement. This strategy requires a metric key to be specified. |

**Examples**

{% tabs %}
{% tab title="Random search" %}
```text
method: random
```
{% endtab %}

{% tab title="Grid search" %}
```text
method: grid
```
{% endtab %}

{% tab title="Bayes search" %}
```text
method: bayes
metric:
  name: val_loss
  goal: minimize
```
{% endtab %}
{% endtabs %}

### Stopping Criteria

Early Termination speeds up hyperparameter search by stopping any poorly-performing runs.

| `early_terminate` sub-key | Meaning |
| :--- | :--- |
| type | specify the stopping algorithm |

We support the following stopping algorithm\(s\):

| `type` | Meaning |
| :--- | :--- |
| hyperband | Use the [hyperband method](https://arxiv.org/abs/1603.06560) |

Hyperband stopping evaluates whether a program should be stopped or permitted to continue at one or more brackets during the execution of the program.  Brackets are configured at static iterations for a specified `metric` \(where an iteration is the number of times a metric has been logged -- if the metric is logged every epoch, then there are epoch iterations\).

In order to specify the bracket schedule, either`min_iter` or `max_iter` needs to be defined. 

| `early_terminate` sub-key | Meaning |
| :--- | :--- |
| min\_iter | specify the iteration for the first bracket |
| max\_iter | specify the maximum number of iterations for the program |
| s | specify the total number of brackets \(required for `max_iter`\) |
| eta | specify the bracket multiplier schedule \(default: 3\) |

**Examples**

{% tabs %}
{% tab title="Hyperband \(min\_iter\)" %}
```text
early_terminate:
  type: hyperband
  min_iter: 3
```

Brackets: 3, 9 \(3\*eta\), 27 \(9 \* eta\), 81 \(27 \* eta\) 
{% endtab %}

{% tab title="Hyperband \(max\_iter\)" %}
```text
early_terminate:
  type: hyperband
  max_iter: 27
  s: 2
```

Brackets: 9 \(27/eta\), 3 \(9/eta\)
{% endtab %}
{% endtabs %}

### Parameters

Describe the hyperparameters to explore. For each hyperparameter, specify the name and the possible values as a list of constants or a range with a certain distribution.

| Values | Meaning |
| :--- | :--- |
| distribution: \(distribution\) | A distribution from the distribution table below. If not specified, the sweep will set to uniform if max and min are set, categorical if values are set and constant if value is set. |
| min: \(float\) max: \(float\) | Continuous values between min and max |
| min: \(int\) max: \(int\) | Integers between min and max |
| values: \[\(float\), \(float\), ...\] | Discrete values |
| value: \(float\) | A constant |
| mu: \(float\) | Mean for normal or lognormal distributions |
| sigma: \(float\) | Standard deviation for normal or lognormal distributions |
| q: \(float\) | Quantization parameter for quantized distributions |

### Distributions

| Name | Meaning |
| :--- | :--- |
| constant | Constant distribution. Must specify value. |
| categorical | Categorical distribution. Must specify values. |
| int\_uniform | Uniform integer. Must specify max and min as integers. |
| uniform | Uniform continuous. Must specify max and min as floats. |
| q\_uniform | Quantized uniform. Returns round\(X / q\) \* q where X is uniform. Q defaults to 1. |
| log\_uniform | Log uniform. Number between exp\(min\) and exp\(max\) so that the logarithm of the return value is uniformly distributed. |
| q\_log\_uniform | Quantized log uniform. Returns round\(X / q\) \* q where X is log\_uniform. Q defaults to 1. |
| normal | Normal distribution. Value is chosen from normal distribution. Can set mean mu \(default 0\) and std dev sigma \(default 1\). |
| q\_normal | Quantized normal distribution. Returns round\(X / q\) \* q where X is normal. Q defaults to 1. |
| log\_normal | Log normal distribution. Value is chosen from log normal distribution. Can set mean mu \(default 0\) and std dev sigma \(default 1\). |
| q\_log\_normal | Quantized log normal distribution. Returns round\(X / q\) \* q where X is log\_normal. Q defaults to 1. |

**Example**

```text
parameters:
  my-parameter:
    min: 1
    max: 20
```

### Command Line <a id="command"></a>

The sweep agent constructs a command line in the following format by default:

```text
/usr/bin/env python train.py --param1=value1 --param2=value2
```

{% hint style="info" %}
On Windows machines the /usr/bin/env will be omitted.  On UNIX systems it ensures the right python interpreter is chosen based on the environment.
{% endhint %}

This command line can be modified by specifying a `command` key in the configuration file.

By default the command is defined as:

```text
command:
  - ${env}
  - ${interpreter}
  - ${program}
  - ${args}
```

| Command Macro | Expansion |
| :--- | :--- |
| ${env} | /usr/bin/env on UNIX systems, omitted on Windows |
| ${interpreter\| | Expands to "python". |
| ${program} | Training script specified by the sweep configuration `program` key |
| ${args} | Expanded arguments in the form --param1=value1 --param2=value2 |

 Examples:

{% tabs %}
{% tab title="Set python interpreter" %}
In order to hardcode the python interpreter you can can specify the interpreter explicitly:

```text
command:
  - ${env}
  - python3
  - ${program}
  - ${args}
```
{% endtab %}

{% tab title="Add extra parameters" %}
Add extra command line arguments not specified by sweep configuration parameters:

```text
command:
  - ${env}
  - ${interpreter}
  - ${program}
  - "-config"
  - your-training-config
  - ${args}
```
{% endtab %}

{% tab title="Omit arguments" %}
If your program does not use argument parsing you can avoid passing arguments all together and take advantage of `wandb.init()` picking up sweep parameters automatically:

```text
command:
  - ${env}
  - ${interpreter}
  - ${program}
```
{% endtab %}
{% endtabs %}

