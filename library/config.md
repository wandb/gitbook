---
description: 실험 구성 저장을 위한 사전과 비슷한 객체
---

# wandb.config

##  **개요**

[![](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/wandb/examples/blob/master/colabs/wandb-config/Configs_in_W%26B.ipynb)

훈련 구성\(training config\): 초매개변수, 데이터 세트 이름 또는 모델 유형과 같은 입력 설정 및 실험을 위한 기타 독립 변수를 저장하기 위해 스크립트에 `wandb.config` 객체를 설정하십시오. 이는 실험을 분석하고 추후 작업 재현에 유용합니다. 웹 인터페이스의 구성 값\(config values\)를 통해 그룹화 할 수 있으며, 다른 실행의 세팅 비교 및 이러한 것들이 어떻게 출력에 영향을 끼치는지 확인하실 수 있습니다. 출력 매트릭 또는 종속 변수\(손실 및 정확성\)sms `wandb.log`를 통해 저장하셔야 합니다.

구성\(config\) 내 중첩된 사전\(nested dictionary\)으로 저희에게 전송하실 수 있으며, 저희 백엔드\(backend\)에서 이름을 평면화합니다. . 구성 변수 이름\(config variable names\)에 점\(dot\) 사용을 하지 않으시는 것을 추천하며, 대시 또는 밑줄을 대신 사용해 주십시오. 일단 wandb config 사전을 생성하고, 여러분의 스크립트가 루트\(root\) 아래의 `wandb.config keys`에 액세스하는 경우, `.` 신택스\(syntax\)대신 \[ \] 신택스를 사용하십시오.

##  **단일 예시**

```python
wandb.config.epochs = 4
wandb.config.batch_size = 32
# you can also initialize your run with a config
wandb.init(config={"epochs": 4})
```

## **효율적인 초기 설정**

한 번에 여러 값을 업로드하여, `wandb.config`를 사전으로 취급하실 수 있습니다.

```python
wandb.init(config={"epochs": 4, "batch_size": 32})
# or
wandb.config.update({"epochs": 4, "batch_size": 32})
```

##  **Argparse 플래그**

 Argparse에서 인자 사전에 제출할 수 있습니다. 이를 통해 명령줄\(command line\)에서 신속하게 초매개변수 값을 실험할 수 있습니다.

```python
wandb.init()
wandb.config.epochs = 4

parser = argparse.ArgumentParser()
parser.add_argument('-b', '--batch-size', type=int, default=8, metavar='N',
                     help='input batch size for training (default: 8)')
args = parser.parse_args()
wandb.config.update(args) # adds all of the arguments as config variables
```

## **Absl 플래그**

또한 Absl 플래그를 제출할 수 있습니다.

```python
flags.DEFINE_string(‘model’, None, ‘model to run’) # name, default, help
wandb.config.update(flags.FLAGS) # adds all absl flags to config
```

##  **파일 기반 구성\(File-Based Configs\)**

**config-defaults.yaml** 라는 이름의 파일을 생성하실 수 있으며, `wandb.config`에 자동으로 로드 됩니다.

```yaml
# sample config defaults file
epochs:
  desc: Number of epochs to train over
  value: 100
batch_size:
  desc: Size of each mini-batch
  value: 32
```

 명령행 인자 `--configs special-configs.yaml` 를 통해 wandb에 다른 구성 파일\(config files\)을 로드하도록 지시할 수 있습니다. 이 인자는 파일 special-configs.yaml 에서 매개변수를 로드합니다

사용 케이스 예시: 실행을 위한 일부 메타 데이터가 포함된 YAML 파일이 있고, Python 스크립트에 초매개변수 사전이 있습니다. 중첩된 구성 객체\(nested config object\)에 둘 다 저장할 수 있습니다:  


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

##  **데이터 세트 식별자 \(Dataset Identifier\)**

 `wandb.config`를 사용하여 데이터 세트를 실험의 입력으로 추적함으로써, 여러분의 데이터 세트를 위한 실행 구성에 고유 식별자 \(예: 해시\(hash\) 또는 기타 식별자\)를 추가하실 수 있습니다.

```yaml
wandb.config.update({'dataset':'ab131'})
```

### **구성 파일 업데이트**

 공용 API를 사용하여 여러분의 config 파일을 업데이트할 수 있습니다

```yaml
import wandb
api = wandb.Api()
run = api.run("username/project/run_id")
run.config["foo"] = 32
run.update()
```

###  **키-값 쌍**

모든 키-값 쌍을 `wand.config`에 로그할 수 있습니다. 훈련하는 모델의 유형에 따라 다를 것입니다. 예:`wandb.config.update({"my_param": 10, "learning_rate": 0.3, "model_architecture": "B"})`

## **TensorFlow 플래그 \(Tensorflow v2에서는 중요하지 않음\)**

 TensorFlow를 구성 객체\(config object\)로 전달할 수 있습니다.

```python
wandb.init()
wandb.config.epochs = 4

flags = tf.app.flags
flags.DEFINE_string('data_dir', '/tmp/data')
flags.DEFINE_integer('batch_size', 128, 'Batch size.')
wandb.config.update(flags.FLAGS)  # adds all of the tensorflow flags as config
```

