---
description: Juypter notebooks에서 스윕을 실행합니다
---

# Sweep from Jupyter Notebook

##  **스윕 초기화 하기**

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
스윕에 대한 개체\(entity\) 또는 프로젝트를 지정하시려면 다음의 방법을 사용하시기 바랍니다:

* Arguments to wandb.sweep\(\) For example: `wandb.sweep(sweep_config, entity="user", project="my_project")`
* [Environment Variables](https://docs.wandb.ai/v/ko/library/environment-variables) `WANDB_ENTITY` and `WANDB_PROJECT`
* [Command Line Interface](https://docs.wandb.ai/v/ko/library/cli) using the `wandb init` command
* [Sweep configuration](https://docs.wandb.ai/v/ko/sweeps/configuration) using the keys "entity" and "project"
{% endhint %}

##  **에이전트 실행하기**

python에서 에이전트를 실행할 때, 에이전트는 스윕 구성 파일의 program 키를 사용하는 것 대신 지정된 함수를 사용합니다.

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

* 개요: [colab에서 실행](https://github.com/wandb/examples/blob/master/examples/wandb-sweeps/sweeps-python/notebook.ipynb)
* 프로젝트에서 스윕을 사용에 대한 전체 설명: [colab에서 실행](https://colab.research.google.com/drive/181GCGp36_75C2zm7WLxr9U2QjMXXoibt)

### wandb.agent\(\)

{% hint style="danger" %}
GPU를 사용할 때 Jupyter notebook 환경과 함께 wandb.agent\(\)를 사용하시면 수행 중 멈출 수 있습니다.

프레임워크에 의한 GPU/CUDA 리소스 초기화 방식 때문에 wandb.agent\(\) 및 jupyter 환경에 나쁜 상호 작용이 있을 수 있습니다

임시 해결책\(이 상호 작용을 수정할 때까지\)으로 에이전트 실행을 위해 python 인터페이스를 사용하지 않는 것입니다. 대신, 스윕 구성에서 program 키를 설정하여 명령줄 인터페이스를 사용하고 다음을 notebook에서 수행합니다: 
{% endhint %}

 **전달인자\(Arguments\)**

* **sweep\_id \(dict\):** UI, CLI, 또는 sweep API가 생성한 스윕 ID
* **entity \(str, optional\):** 실행을 전송할 사용자이름 또는 팀
* **project \(str, optional\):** 실행을 전송할 프로젝트
* **function \(dir, optional\):** 스윕 함수\(sweep function\) 구성하기

##  **로컬 컨트롤러 실행하기**

여러분의 고유한 매개변수 검색 알고리즘을 개발하고 싶으신 경우, python에서 컨트롤러를 실행하실 수 있습니다.

컨트롤러를 실행하는 가장 간단한 방법은 다음과 같습니다:

```python
sweep = wandb.controller(sweep_id)
sweep.run()
```

컨트롤러 루프\(controller loop\)를 좀 더 제어하고 싶으신 경우 다음과 같습니다:

```python
import wandb
sweep = wandb.controller(sweep_id)
while not sweep.done():
    sweep.print_status()
    sweep.step()
    time.sleep(5)
```

또는 제공되는 매개변수를 더욱 제어하고 싶으신 경우 다음과 같습니다:

```python
import wandb
sweep = wandb.controller(sweep_id)
while not sweep.done():
    params = sweep.search()
    sweep.schedule(params)
    sweep.print_status()
```

코드로 스윕을 완전히 지정하고 싶으신 경우, 다음과 같은 작업을 수행할 수 있습니다:

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

