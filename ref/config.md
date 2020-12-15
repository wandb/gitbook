# Config

## wandb.sdk.wandb\_config

 [\[소스\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_config.py#L3)​

config.

### Config Objects

```python
class Config(object)
```

​[\[소스\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_config.py#L32)​

Config는 초매개변수와 같은 스크립트에 대한 입력을 추적에 유용한 사전과 같은 객체입니다. 다음과 같이 작업을 초기화할 때 작업 시작 시에 이것을 한 번 설정하는 것이 좋습니다: wandb.init\(config={"key": "value"}\). 예를 들어, ML 모델을 훈련 중인 경우, 구성\(config\)에서 learning\_rate 및 batch\_size를 추적할 수 있습니다.

config 객체를 사용하여 실행의 초매개변수를 저장합니다. 새 추적된 실행을 시작하기 위해 wandb.init\(\)를 호출한 경우, 실행 객체는 저장됩니다. config 객체를 실행과 동시에 저장하는 것을 추천하며, 다음과 같습니다: wandb.init\(config=my\_config\_dict\) .

onfig-defaults.yaml 파일을 생성할 수 있으며, wandb는 cofig를 wandb.config로 자동으로 로드합니다. 또는 사용자 지정 이름의 YAML 파일을 사용하여 파일이름을 전달할 수 있습니다. wandb.init\(config="my\_config\_file.yaml"\) [https://docs.wandb.com/library/config\#file-based-configs](https://docs.wandb.com/library/config#file-based-configs)를 참조하시기 바랍니다.

 **예시**:

 기본 사용법

```text
wandb.config.epochs = 4
wandb.init()
for x in range(wandb.config.epochs):
# train
```

wandb.init를 사용하여 config 설정

```text
- `wandb.init(config={"epochs"` - 4, "batch_size": 32})
for x in range(wandb.config.epochs):
# train
```

 중첩 구성

```text
wandb.config['train']['epochs] = 4
wandb.init()
for x in range(wandb.config['train']['epochs']):
# train
```

 absl flags 사용

```text
flags.DEFINE_string(‘model’, None, ‘model to run’) # name, default, help
wandb.config.update(flags.FLAGS) # adds all absl flags to config
```

Argparse 플래그

```text
wandb.init()
wandb.config.epochs = 4

parser = argparse.ArgumentParser()
parser.add_argument('-b', '--batch-size', type=int, default=8, metavar='N',
help='input batch size for training (default: 8)')
args = parser.parse_args()
wandb.config.update(args)
```

 TensorFlow 플래그 사용 \(tensorflow v2에서는 더 이상 사용되지 않음\)

```text
flags = tf.app.flags
flags.DEFINE_string('data_dir', '/tmp/data')
flags.DEFINE_integer('batch_size', 128, 'Batch size.')
wandb.config.update(flags.FLAGS)  # adds all of the tensorflow flags to config
```

**persist**

```python
 | persist()
```

​[\[소스\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_config.py#L162)​

 콜백이 설정

