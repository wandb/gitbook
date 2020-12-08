---
description: Jupyterノートブックからスイープを実行します
---

# Sweep from Jupyter Notebook

## スイープを初期化します

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
スイープのエンティティまたはプロジェクトを指定するには、次の方法を使用します。

* wandb.sweep（）への引数例：`wandb.sweep(sweep_config, entity="user", project="my_project")`
* [環境変数](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MNdlQobOrN8f63KfkRZ/v/japanese/library/environment-variables)WANDB\_ENTITYおよびWANDB\_PROJECT
* wandb initコマンドを使用した[コマンドラインインターフェイス](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MNdlQobOrN8f63KfkRZ/v/japanese/library/cli)
*  wandb initコマンドを使用した[コマンドラインインターフェイス](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MNdlQobOrN8f63KfkRZ/v/japanese/sweeps/configuration)
{% endhint %}

## エージェントを実行します

Pythonからエージェントを実行する場合、エージェントはスイープ構成ファイルのプログラムキーを使用する代わりに、指定された関数を実行します。

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

* 概要：[colabで実行](https://github.com/wandb/examples/blob/master/examples/wandb-sweeps/sweeps-python/notebook.ipynb)
* プロジェクトでスイープを使用するための完全なウォークスルー：[colabで実行](https://github.com/wandb/examples/blob/master/examples/wandb-sweeps/sweeps-python/notebook.ipynb)

### wandb.agent\(\)

{% hint style="danger" %}
Using wandb.agent\(\) with jupyter notebook environments can hang when using GPUs.

There can be a bad interaction between wandb.agent\(\) and jupyter environments due to how GPU/CUDA resources are initialized by frameworks.

A temporary workaround \(until we can fix these interactions\) is to avoid using the python interface for running the agent. Instead, use the command line interface by setting the `program` key in the sweep configuration, and execute: `!wandb agent SWEEP_ID` in your notebook.
{% endhint %}

**引数**• **sweep\_id \(dict\)**：UI、CLI、またはスイープAPIによって生成されたスイープID

* **entity \(str, optional\)**：ランを送信するユーザー名またはチーム
* **project \(str, optional\)**：実行を送信するプロジェクト
* **function \(dir, optional\)**：スイープ機能を設定します

##  ローカルコントローラーを実行します

独自のパラメーター検索アルゴリズムを開発したい場合は、Pythonからコントローラーを実行できます。

 コントローラを実行する最も簡単な方法：

```python
sweep = wandb.controller(sweep_id)
sweep.run()
```

コントローラループをさらに制御したい場合：

```python
import wandb
sweep = wandb.controller(sweep_id)
while not sweep.done():
    sweep.print_status()
    sweep.step()
    time.sleep(5)
```

または、提供されるパラメーターをさらに制御します。

```python
import wandb
sweep = wandb.controller(sweep_id)
while not sweep.done():
    params = sweep.search()
    sweep.schedule(params)
    sweep.print_status()
```

スイープをコードで完全に指定したい場合は、次のようにすることができます。

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

