---
description: 为类字典对象，用于保存你的实验配置。
---

# wandb.config

### **概述**

在你的脚本中设置`wandb.config`对象，以保存你的训练配置：超参数（Hyperparameter）、输入设置（如数据集名称和模型类型），及其他用于实验的独立变量。这对于分析你的实验以及在将来重现你的工作很有用。你可以在Web界面根据配置值进行分组、比较不同运行的设置以及查看这些配置如何影响输出。要注意，输出指标（Metric）或独立变量（如损失和准确率）应当用`wandb.log`保存而不是`wandb.config`。

你可以用config给我们发送嵌套字典，我们会在后台用点号`.将`名称扁平化。我们建议你，要避免在config的变量名中使用点号，应当用连字符或下划线。一旦创建好了`wandb.config`字典， 如果你的脚本访问wandb.config根节点下的键，请使用`[ ]代替` . 语法。

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

## TensorFlow Flags **（在tensorflow v2中已被标为deprecated不建议使用）**

可以把TensorFlow flags传递给config对象。

```python
wandb.init()
wandb.config.epochs = 4

flags = tf.app.flags
flags.DEFINE_string('data_dir', '/tmp/data')
flags.DEFINE_integer('batch_size', 128, 'Batch size.')
wandb.config.update(flags.FLAGS)  # adds all of the tensorflow flags as config
```

##  Argparse Flags

你也可以传入 absl flags

```python
wandb.init()
wandb.config.epochs = 4

parser = argparse.ArgumentParser()
parser.add_argument('-b', '--batch-size', type=int, default=8, metavar='N',
                     help='input batch size for training (default: 8)')
args = parser.parse_args()
wandb.config.update(args) # adds all of the arguments as config variables
```

###  **基于文件的配置**

 你可以创建一个名为**config-defaults.yaml 的文件**，它会被自动加载到`wandb.config`

```yaml
# sample config defaults file
epochs:
  desc: Number of epochs to train over
  value: 100
batch_size:
  desc: Size of each mini-batch
  value: 32
```

你也可以通过命令行参数 --configs 让wandb 加载不同的配置文件，例如.

`--configs special-configs.yaml`将从文件special-configs.yaml中加载参数。

例如： 你有一个包含运行元数据的 YAML文件；另外在你的Python脚本中还有一个超参数字典。你可以将两者一起保存到嵌套的config对象中：

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

你可以在你的运行配置中为你的数据集添加一个唯一标识符（如哈希或其他标识符），通过使用`wandb.config`跟踪它作为你的实验输入。

```yaml
wandb.config.update({'dataset':'ab131'}) 
```

**更新配置文件**

你可以用公共API来更新你的config文件

```yaml
import wandb
api = wandb.Api()
run = api.run("username/project/run_id")
run.config["foo"] = 32
run.update()
```

**键值对**

  
任何键值对你都可以记录到wandb.config 中。对于所训练的每种模型，对应的键值对也不相同。例如`wandb.config.update({"my_param": 10, "learning_rate": 0.3, "model_architecture": "B"})`

### **TensorFlow Flags（在tensorflow v2中已被标为deprecated不建议使用）**

可以把TensorFlow flags传递给config对象。

```python
wandb.init()
wandb.config.epochs = 4

flags = tf.app.flags
flags.DEFINE_string('data_dir', '/tmp/data')
flags.DEFINE_integer('batch_size', 128, 'Batch size.')
wandb.config.update(flags.FLAGS)  # adds all of the tensorflow flags as config
```

