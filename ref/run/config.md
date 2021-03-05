# Config

[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)](https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_config.py#L24-L263)**​**[**​GitHub에서 소스 확인하기**](https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_config.py#L24-L263)**​**

Config 객체

```text
config() -> None
```

Config 객체는 wandb run\(실행\)과 관련된 모든 초매개변수를 수용하기 위한 것으로 `wandb.init`가 호출된 경우 실행 객체와 함께 저장됩니다.

훈련 실험 상단에서 `wandb.config`를 한 번 설정하거나 config를 init의 매개변수로 설정하시기 바랍니다. 즉, 다음과 같습니다: `wandb.init(config=my_config_dict)`

config-defaults.yaml이라는 파일을 생성할 수 있으며, 이는 자동으로 `wandb.config`에 로드됩니다. [https://docs.wandb.com/library/config\#file-based-configs](https://docs.wandb.com/library/config#file-based-configs)를 참조하시기 바랍니다.

또한 여러분의 사용자 지정 이름의 config YAML 파일을 로드하여 파일 이름을 `wandb.init(config="special_config.yaml")`에 전달할 수 있습니다. [https://docs.wandb.com/library/config\#file-based-configs](https://docs.wandb.com/library/config#file-based-configs)를 참조하시기 바랍니다.

## **예시:**

 기본 사용법

```text
wandb.config.epochs = 4
wandb.init()
for x in range(wandb.config.epochs):
    # train
```

wandb.init을 사용하여 config 설정

```text
wandb.init(config={"epochs": 4, "batch_size": 32})
for x in range(wandb.config.epochs):
    # train
```

 중첩된 config

```text
wandb.config['train']['epochs] = 4
wandb.init()
for x in range(wandb.config['train']['epochs']):
    # train
```

absl 플래그 사용

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

TensorFlow 플래그 사용 \(tensorflow v2에서는 사용할 수 없음\)

```text
flags = tf.app.flags
flags.DEFINE_string('data_dir', '/tmp/data')
flags.DEFINE_integer('batch_size', 128, 'Batch size.')
wandb.config.update(flags.FLAGS)  # adds all of the tensorflow flags to config
```

