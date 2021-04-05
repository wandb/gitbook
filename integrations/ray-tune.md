# Ray Tune

 W&B는 두 개의 가벼운 통합을 제공하여 [Ray](https://github.com/ray-project/ray)와 통합됩니다.

 첫째로 `WandbLogger`가 있으며, 이는 Tune to the Wandb API\(Wandb API으로 조정\)에 보고된 메트릭을 자동으로 로그합니다. 나머지 하나는 `@wandb_mixin` 데코레이터\(decorator\)로, 함수 API와 함께 사용하실 수 있습니다. 이것은 Tune의 훈련 정보와 함께 Wandb API를 자동 초기화합니다. 평소에 사용하시는 것처럼 Wandb API를 사용하시기만 하면 됩니다. \(예: `wandb.log()`를 상용하여 훈련 프로세스를 로그 하는 것\)

## WandbLogger

```python
from ray.tune.integration.wandb import WandbLogger
```

 Wandb 구성은 wandb 키를 `tune.run()`의 구성\(config\) 매개변수에 전달함으로써 수행됩니다. \(아래 예시 참조\)

 wandb config 엔트리\(entry\)의 콘텐츠는 키워드 전달인자\(keyword arguments\)로 `wandb.init()`에 전달됩니다. 다음의 설정은 예외로, `WandbLogger` 자체를 구성할 때 사용됩니다.

###  **매개변수**

`api_key_file (str)` – `Wandb API KEY`를 포함하는 파일의 경로.

`api_key (str)` – Wandb API Key. `api_key_file` 설정 대신 사용할 수 있습니다.

`excludes (list)` – 로그에서 제외되어야 하는 메트릭의 리스트.

`log_config (bool)` – results dict의 구성 매개변수가 로그 되어야 하는지에 대해 나타내는 불린. 이는, 같은 훈련 중에 예를 들어 `PopulationBasedTraining`와 함께 매개변수가 변경되는 경우에 이해하기 쉽습니다. 기본값은 False입니다.

###  **예시**

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

 이 Ray Tune Trainable `mixin`은 `Trainable` 클래스 또는 함수 API에 대한 `@wandb_mixin`과 함께 사용할 Wandb API 초기화에 도움이 됩니다.

기본 사용법으로는, 여러분의 훈련 함수를 `@wandb_mixin` 데코레이터\(decorator\)앞에 추가하시기만 하면 됩니다.

```python
from ray.tune.integration.wandb import wandb_mixin

@wandb_mixin
def train_fn(config):
    wandb.log()
```

 Wandb 구성은 wandb 키를 `tune.run()`의 `config` 매개변수에 전달함으로써 수행됩니다 \(아래 예시 참조\).

 wandb 구성 엔트리\(config entry\)의 콘텐츠는 키워드 전달인자로써 `wandb.init()`로 전달됩니다. `WandbTrainableMixin` 자체를 구성하는데 사용되는 다음의 설정은 예외입니다:

### **매개변수**

`api_key_file (str)` – Wandb `API KEY`를 포함하는 파일의 경로.

`api_key (str)` – Wandb API Key. `api_key_file` 설정 대신 사용할 수 있습니다.

Wandb의 `group`, `run_id` 및 `run_name`은 Tune에 의해서 자동으로 선택되지만, 각각의 구성 값을 입력하여 덮어쓸 수 있습니다.

다른 모든 유효한 구성 설정을 확인하시려면 다음의 링크를 참조하십시오: [https://docs.wandb.com/library/init](https://docs.wandb.com/library/init)​

**예시:**

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

##  **예시 코드**

통합의 작동 방식을 보여드리기 위한 몇 가지 예시를 만들어보았습니다:

* [Colab](https://colab.research.google.com/drive/1an-cJ5sRSVbzKVRub19TmmE4-8PUWyAi?usp=sharing): 통합을 시도하는 간단한 데모
* [대시보드\(Dashboard\)](https://app.wandb.ai/authors/rayTune?workspace=user-cayush): 예시에서 생성된 대시보드 보기

