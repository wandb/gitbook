# Ray Tune

 W&B通过提供两个轻量级的集成与[Ray](https://github.com/ray-project/ray) 进行集成。

一个是`WandbLogger`, 它可以自动将上报给Tune的指标记录到Wandb API中。另一个是`@wandb_mixin` 装饰器, 它可以和函数API一起使用。它自动用Tune的训练信息初始化Wandb API。你可以像平常一样使用Wandb API,例如使用`wandb.log()` 来记录你的训练过程。

## WandbLogger

```python
from ray.tune.integration.wandb import WandbLogger
```

Wandb配置是通过向`tune.run()` 的config参数传递wandb密匙（key） 来完成的（见下面的示例）。

wandb配置项的内容作为关键字参数传递给`wandb.init()` 。例外的情况是下面的设置，他们用于配置 `WandbLogger`本身。

### **参数**

`api_key_file (字符串)` – 包含`Wandb API KEY`的文件路径。

`api_key (字符串)` – Wandb API 密匙（Key）。设置`api_key_file`的替代选项。 

`excludes (列表)` – 排除在`log`之外的指标列表。

`log_config (布尔值)` –表示是否应该记录结果dict的配置参数的布尔值。如果在训练过程中参数会发生变化，例如使用`PopulationBasedTraining`，那么这会有用。默认为False。

**示例**

```python
from ray.tune.logger import DEFAULT_LOGGERS
from ray.tune.integration.wandb import WandbLogger
tune.run(
    train_fn,
    config={
        # define search space here
        "parameter_1": tune.choice([1, 2, 3]),
        "parameter_2": tune.choice([4, 5, 6]),
        # wandb configuration
        "wandb": {
            "project": "Optimization_Project",
            "api_key_file": "/path/to/file",
            "log_config": True
        }
    },
    loggers=DEFAULT_LOGGERS + (WandbLogger, ))
```

## wandb\_mixin

```python
ray.tune.integration.wandb.wandb_mixin(func)
```

这个Ray Tune Trainable `mixin` 帮助初始化Wandb API，以便通过`Trainable` 类或 `@wandb_mixin` 使用函数API对于基本的使用，只需在你的训练函数前加上 `@wandb_mixin` 装饰器即可:

```python
from ray.tune.integration.wandb import wandb_mixin

@wandb_mixin
def train_fn(config):
    wandb.log()
```

Wandb配置是通过向`tune.run()` 的 `config` 参数传递`wandb key`来完成的（见下面的示例）。

wandb配置项的内容作为关键字传递给 `wandb.init()` 。例外的是下面的设置，它们用于配置 `WandbTrainableMixin` 本身:

### **参数**

`api_key_file (字符串)` – 包含Wandb `API KEY`的文件路径。

`api_key (字符串)` – Wandb API Key。`api_key_file` 设置的替代选项。

Wandb的 `group`, `run_id` 和`run_name` 由Tune自动选择，但可以通过填写相应的配置值进行覆盖。

所有其他有效配置设置，请参见这里: [https://docs.wandb.com/library/init](https://docs.wandb.com/library/init)​

### **示例:**

```python
from ray import tune
from ray.tune.integration.wandb import wandb_mixin

@wandb_mixin
def train_fn(config):
    for i in range(10):
        loss = self.config["a"] + self.config["b"]
        wandb.log({"loss": loss})
        tune.report(loss=loss)

tune.run(
    train_fn,
    config={
        # define search space here
        "a": tune.choice([1, 2, 3]),
        "b": tune.choice([4, 5, 6]),
        # wandb configuration
        "wandb": {
            "project": "Optimization_Project",
            "api_key_file": "/path/to/file"
        }
    })
```

## **示例代码**

我们已经为你创建了一些示例，以了解集成的工作原理：

*  [Colab](https://colab.research.google.com/drive/1an-cJ5sRSVbzKVRub19TmmE4-8PUWyAi?usp=sharing): 一个集成的简单演示
*  [Dashboard](https://app.wandb.ai/authors/rayTune?workspace=user-cayush): 查看示例生成的仪表盘

