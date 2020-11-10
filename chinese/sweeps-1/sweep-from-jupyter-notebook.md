---
description: 从Jupyter Notebook中运行sweep
---

# Sweep from Jupyter Notebook

## **初始化一个sweep**

```python
import wandb

sweep_config = {
  "name": "My Sweep",
  "method": "grid",
  "parameters": {
        "param1": {
            "values": [1, 2, 3]
        }
    }
}

sweep_id = wandb.sweep(sweep_config)
```

{% hint style="info" %}
使用以下方法以指定扫描的实体或项目：

* Arguments to wandb.sweep\(\) For example: `wandb.sweep(sweep_config, entity="user", project="my_project")`
* [Environment Variables](../../library/environment-variables.md) `WANDB_ENTITY` and `WANDB_PROJECT`
* [Command Line Interface](../../library/cli.md) using the `wandb init` command
* [Sweep configuration ](../../sweeps/configuration.md)using the keys "entity" and "project"
{% endhint %}

## **运行代理**

从python运行代理程序时，该代理程序将运行指定的功能，而不是使用扫描配置文件中的`program`密钥。

```python
import wandb
import time

def train():
    run = wandb.init()
    print("config:", dict(run.config))
    for epoch in range(35):
        print("running", epoch)
        wandb.log({"metric": run.config.param1, "epoch": epoch})
        time.sleep(1)

wandb.agent(sweep_id, function=train)
```

* 快速预览：[在colab中运行](https://github.com/wandb/examples/blob/master/examples/wandb-sweeps/sweeps-python/notebook.ipynb)
* 完整教程：[在colab中运行](https://colab.research.google.com/drive/181GCGp36_75C2zm7WLxr9U2QjMXXoibt)

## wandb.agent\(\)

{% hint style="danger" %}
在使用GPU的情况下，将wandb.agent（）与jupyter notebook一起使用可能会导致环境变量挂起。

wandb.agent（）与jupyter环境之间可能存在不良交互，这是由初始化GPU / CUDA资源的初始化方式导致的。

暂时的解决方法（直到我们可以解决这些交互问题）是避免使用python接口运行代理。 而是通过在扫描配置中设置program键来使用命令行界面，然后在笔记本中执行：!wandb agent SWEEP\_ID。
{% endhint %}

**参数**

* **sweep\_id \(dict\)**: 由UI，CLI或扫描API生成的扫描ID
* **entity \(str, optional\):** 您要发送运行的用户名或团队
* **project \(str, optional\):** 要发送运行的项目
* **function \(dir, optional\):** 配置扫描功能

## **运行本地控制器**

如果要开发自己的参数搜索算法，您可以从python运行控制器

运行控制器的最简单方法：

```python
sweep = wandb.controller(sweep_id)
sweep.run()
```

如果要对控制器循环进行更多控制：

```python
import wandb
sweep = wandb.controller(sweep_id)
while not sweep.done():
    sweep.print_status()
    sweep.step()
    time.sleep(5)
```

甚至可以进一步控制所提供的参数：

```python
import wandb
sweep = wandb.controller(sweep_id)
while not sweep.done():
    params = sweep.search()
    sweep.schedule(params)
    sweep.print_status()
```

如果要使用代码完全指定扫描，则可以执行以下操作：

```python
import wandb
from wandb.sweeps import GridSearch,RandomSearch,BayesianSearch

sweep = wandb.controller()
sweep.configure_search(GridSearch)
sweep.configure_program('train-dummy.py')
sweep.configure_controller(type="local")
sweep.configure_parameter('param1', value=3)
sweep.create()
sweep.run()
```

