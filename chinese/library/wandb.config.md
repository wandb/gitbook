---
description: 为字典类对象，用于保存实验配置。
---

# wandb.config

### **概述**

在脚本开头要设置一次`wandb.config`，用以保存训练配置：超参数、输入设置（如数据集名称和模型类型），及其他用于实验的独立变量。这有利于以后分析实验、重新制作模型。你可以在网页界面根据配置值进行分组、比较不同运行项的设置、查看配置值如何影响输出。要注意，输出指标和独立变量（如损失和准确率）应当用`wandb.log`保存。config只能在训练实验开头设置**一次**。你可以用config给我们发送嵌套字典，我们会在后台用点号`.`把名称整理好。我们建议你，要避免在config的变量名中使用点号，应当用破折号或下划线。一旦创建好了`wandb.config`字典，如果你的脚本在root后面获取wandb.config键值，就用`[ ]`句法，不要用点号.句法。

### **简单示例**

```python
wandb.config.epochs = 4
wandb.config.batch_size = 32
# you can also initialize your run with a config
wandb.init(config={"epochs": 4})
```

### **高效初始化**

你可以把`wandb.config`视为一个字典，一次就能更新多个值。

```python
wandb.init(config={"epochs": 4, "batch_size": 32})
# or
wandb.config.update({"epochs": 4, "batch_size": 32})
```

## TensorFlow Flags

可以把TensorFlow flags传递给config对象。

```python
wandb.init()
wandb.config.epochs = 4

flags = tf.app.flags
flags.DEFINE_string('data_dir', '/tmp/data')
flags.DEFINE_integer('batch_size', 128, 'Batch size.')
wandb.config.update(flags.FLAGS)  # adds all of the tensorflow flags as config
```

## Argparse Flags

你可以从Argparse传入参数字典。这便于从命令行快速测试不同的超参数值。

```python
wandb.init()
wandb.config.epochs = 4

parser = argparse.ArgumentParser()
parser.add_argument('-b', '--batch-size', type=int, default=8, metavar='N',
                     help='input batch size for training (default: 8)')
args = parser.parse_args()
wandb.config.update(args) # adds all of the arguments as config variables
```

## File-Based Configs

你可以创建一个文件并命名为**config-defaults.yaml**，它会自动加载到`wandb.config`

```yaml
# sample config defaults file
epochs:
  desc: Number of epochs to train over
  value: 100
batch_size:
  desc: Size of each mini-batch
  value: 32
```

你可以让wandb加载不同的config文件，其方法为，在命令行利用参数`--configs special-configs.yaml`，将从文件special-configs.yaml载入参数。

举个使用案例：你有一个.yaml文件，文件中含有运行项的元数据；另外在Python脚本中还有一个超参数字典。你可以把两者一起保存到嵌套的config对象

```python
hyperparameter_defaults = dict(
    dropout = 0.5,
    batch_size = 100,
    learning_rate = 0.001,
    )

config_dictionary = dict(
    yaml=my_yaml_file,
    params=hyperparameter_defaults,
    )

wandb.init(config=config_dictionary)
```

### **数据集标识符**

你可以在运行配置中为数据集添加一个唯一标识符（如哈希或其他标识符），其方法就是利用`wandb.config`跟踪数据集并作为实验的输入。

```yaml
wandb.config.update({'dataset':'ab131'})
```

**更新配置文件**

你可以用公共接口API更新config文件

```yaml
import wandb
api = wandb.Api()
run = api.run("username/project/run_id")
run.config["foo"] = 32
run.update()
```

**键值对**

任何键值对你都可以记录到wandb.config。对于所训练的每种模型，对应的键值对也不相同。例如`wandb.config.update({"my_param": 10, "learning_rate": 0.3, "model_architecture": "B"})`

