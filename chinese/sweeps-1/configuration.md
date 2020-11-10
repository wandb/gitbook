---
description: 用于设置超参数范围，搜索策略以及其他扫描相关的语法。
---

# Configuration

使用这些配置字段来自定义扫描。 有两种方法可以指定您的配置：

1. [文件](https://docs.wandb.com/sweeps/quickstart#2-sweep-config)：最适合分布式扫描。 在此[处查](https://github.com/wandb/examples/tree/master/examples/keras/keras-cnn-fashion)看示例。
2. [Python数据结构](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MKaPhwzNIegNuInaekR/sweeps/python-api)：最适合从Jupyter Notebook运行扫描

| Top-level key | 含义 |
| :--- | :--- |
| name | 扫描名称，显示在W＆B UI中 |
| description | 文字说明（注释） |
| program | 要运行的训练脚本（必需） |
| metric | 指定要优化的指标（由某些搜索策略和停止条件使用） |
| method | 指定[搜索策略](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MKaPhwzNIegNuInaekR/sweeps/configuration#search-strategy)（必填） |
| early\_terminate | 指[定停止条](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MKaPhwzNIegNuInaekR/sweeps/configuration#stopping-criteria)件（可选，默认为不提前停止） |
| parameters | 指定要搜索的[参数](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MKaPhwzNIegNuInaekR/sweeps/configuration#parameters)范围（必填） |
| project | 指定此扫描的项目 |
| entity | 指定此扫描的实体 |
| command | 指定如何运行训练脚本的[命令行](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MKaPhwzNIegNuInaekR/sweeps/configuration#command) |

**指标**

指定要优化的指标。 该指标应通过您的训练脚本明确记录到W＆B。 例如，如果要最大程度地减少模型的验证损失\(validation loss\)，请执行以下操作：

```python
# [model training code that returns validation loss as valid_loss]
wandb.log({"val_loss" : valid_loss})
```

| `metric` sub-key | 含义 |
| :--- | :--- |
| name | 要优化的指标名称 |
| goal | 最小化或最大化（默认为最小化） |
| target | 您想要优化的指标目标值。 当扫描中的任何运行达到该目标值时，扫描的状态将设置为“完成”。 这意味着所有代理都将完成这些运行，但不会在扫描中启动新的运行 |

{% hint style="danger" %}
指定的指标必须是“顶级”指标：

这将**不**起作用：  
Sweep configuration:  
metric:  
name: my\_metric.nested  
Code:  
`nested_metrics = {"nested": 4}    
wandb.log({"my_metric", nested_metrics}`

要解决此限制，脚本应在顶层记录嵌套指标，如下所示：

扫描配置：  
metric:  
name: my\_metric\_nested  
Code:  
`nested_metrics = {"nested": 4}    
wandb.log{{"my_metric", nested_metric}    
wandb.log({"my_metric_nested", nested_metric["nested"]})`
{% endhint %}

**示例**

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
```text
metric:
  name: val_loss
  goal: maximize
  target: 0.1
```
{% endtab %}
{% endtabs %}

**搜索策略**

在扫描配置中用`method`指定搜索策略

| `method` | 含义 |
| :--- | :--- |
| grid | 网格搜索会迭代所有可能的参数值组合。 |
| random | 随机搜索选择随机的值集。 |
| bayes | 贝叶斯优化使用高斯过程对该函数建模，然后选择参数以优化改进概率。 此策略需要指定指标密钥 |

**示例**

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

**停止条件**

提前终止是一项可选功能，可通过停止性能不佳的运行来加快超参数搜索。 触发提前停止时，代理将停止当前运行并获取下一组要尝试的超参数。

| `early_terminate` sub-key | 含义 |
| :--- | :--- |
| type | 指定终止算法 |

我们支持以下终止算法：

| `type` | 含义 |
| :--- | :--- |
| hyperband | 使用[hyperband方法](https://arxiv.org/abs/1603.06560) |

Hyperband终止算法在执行程序期间评估是否应停止程序，还是应允许程序在一个或多个括号中继续执行。 括号是在静态迭代中针对指定指标配置的（其中迭代是指标的记录次数——如果在每个训练epoch都记录了指标，则存在epoch迭代）。

为了指定括号里的安排，需要定义`min_iter`或`max_iter`。

| `early_terminate` sub-key | 含义 |
| :--- | :--- |
| min\_iter | 指定第一个括号的迭代 |
| max\_iter | 指定该程序的最大迭代次数 |
| s | 指定括号数量（`max_iter`需要这个参数） |
| eta | 指定括号里的乘数（默认为3 |

**示例**

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

**参数**

描述要探索的超参数。 对于每个超参数，将名称和可能的值指定为常量列表（对于任何method适用），或者指定分布（对于`random`或`bayes`适用）。

| Values | 含义 |
| :--- | :--- |
| values: \[\(type1\), \(type2\), ...\] | 指定此超参数的所有有效值。 与网格兼容。 |
| value: \(type\) | 指定此超参数的单个有效值。 与网格兼容。 |
| distribution: \(distribution\) | 从下面的分布表中选择一个分布。 如果未指定分布：如果设置了`value`，则默认为`categorical`；如果将`max`和`min`设置为整数，则默认为`int_uniform`；如果将`max`和`min`设置为float，则默认为`int_uniform`；如果设置为`value`，则默认为`constant`。 |
| min: \(float\) max: \(float\) | 均匀分布`uniform`的超参数的最大和最小有效值。 |
| min: \(int\) max: \(int\) | `int_uniform`分布式超参数的最大值和最小值。 |
| mu: \(float\) | 正态分布`normal`或对数正态分布`lognormal`超参数的平均参数。 |
| sigma: \(float\) | 正态分布`normal`或对数正态分布`lognormal`的超参数的标准偏差参数。 |
| q: \(float\) | 量化超参数的量化步长 |

**示例**

{% tabs %}
{% tab title="grid - single value" %}
```text
parameter_name:
  value: 1.618
```
{% endtab %}

{% tab title="grid - multiple values" %}
```text
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

{% tab title="random or bayes - normal distribution" %}
```text
parameter_name:
  distribution: normal
  mu: 100
  sigma: 10
```
{% endtab %}
{% endtabs %}

**分布**

| Name | 含义 |
| :--- | :--- |
| constant | 分类分布。必须指定`value`。 |
| categorical | 分类分布。必须指定`value`。. |
| int\_uniform | 整数上的离散均匀分布。必须将`max`和`min`指定为整数。 |
| uniform | 连续均匀分布。必须将`max`和`min`指定为浮点数。 |
| q\_uniform | 量化均匀分布。返回`round（X / q）* q`，其中`X`是统一的。 `q`默认为`1`。 |
| log\_uniform | 对数均匀分布。返回介于`exp（min）`和`exp（max`）之间的值，以使自然对数在`min`和`max`之间均匀分布。 |
| q\_log\_uniform | 量化对数统一。返回`round（X / q）* q`，其中`X`是log\_uniform。 `q`默认为`1`。 |
| normal | 正态分布。返回值是均值为`mu`的正态分布（默认值为`0`）和标准偏差`sigma`（默认值为`1`）的正态分布。 |
| q\_normal | 量化正态分布。返回`round（X / q）* q`，其中`X`是normal。 `Q`默认为`1`。 |
| log\_normal | 记录正态分布。返回值`X`，以使自然对数`log（X）`正态分布为均值`mu`（默认值为`0`）和标准偏差`sigma`（默认值为`1`）。 |
| q\_log\_normal | 量化的对数正态分布。返回`round（X / q）* q`，其中X为`log_normal`。 `q`默认为`1`。 |

**示例**

{% tabs %}
{% tab title="constant" %}
```text
parameter_name:
  distribution: constant
  value: 2.71828
```
{% endtab %}

{% tab title="categorical" %}
```text
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
```text
parameter_name:
  distribution: uniform
  min: 0
  max: 1
```
{% endtab %}

{% tab title="q\_uniform" %}
```text
parameter_name:
  distribution: q_uniform
  min: 0
  max: 256
  q: 1
```
{% endtab %}
{% endtabs %}

**命令行**

扫描代理默认情况下以以下格式构造命令行：

```text
/usr/bin/env python train.py --param1=value1 --param2=value2
```

{% hint style="info" %}
在Windows计算机上，/ usr / bin / env将被省略。 在UNIX系统上，它确保根据环境选择正确的python解释器。
{% endhint %}

可以通过在配置文件中指定`command`键来修改此命令行。

默认情况下，该命令定义为：

```text
command:
  - ${env}
  - ${interpreter}
  - ${program}
  - ${args}
```

| Command Macro | Expansion |
| :--- | :--- |
| ${env} | UNIX 系统：/usr/bin/env on systems； Windows省略 |
| ${interpreter\| | 扩展至python |
| ${program} | Sweep配置中的`program`指定的训练脚本 |
| ${args} | 扩展参数的形式：param1=value1 --param2=value2 |
| ${args\_no\_hyphens} | Expanded arguments in the form param1=value1 param2=value2 |
| ${json} | Arguments encoded as JSON |
| ${json\_file} | The path to a file containing the args encoded as JSON |

**示例**

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

{% tab title="Use with Hydra" %}
You can change the command to pass arguments they way tools like Hydra expect.

```text
command:
  - ${env}
  - ${interpreter}
  - ${program}
  - ${args_no_hyphens}
```
{% endtab %}
{% endtabs %}

**常见问题**

**嵌套配置**

目前，扫描不支持嵌套值，但是我们计划在不久的将来对此进行支持。

