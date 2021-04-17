# Sweeps Quickstart

 임의의 머신 러닝 모델에서 시작해서 수 분내에 초매개변수 스윕을 실행하실 수 있습니다. 작업 예시를 보고 싶으신가요? 다음은 [예시 코드](https://github.com/wandb/examples/tree/master/examples/pytorch/pytorch-cnn-fashion)와 [예시 대시보드](https://app.wandb.ai/carey/pytorch-cnn-fashion/sweeps/v8dil26q)입니다.

![](../.gitbook/assets/image%20%2847%29%20%282%29%20%283%29%20%284%29%20%283%29%20%283%29.png)

{% hint style="info" %}
이미 Weights & Biases 프로젝트가 있으신가요? [다음 스윕 튜토리얼로 건너뛰기 →](https://docs.wandb.ai/v/ko/sweeps/existing-project)​
{% endhint %}

## 1. **wandb 추가하기**

###  **계정 설정하기**

1.  W&B 계정으로 시작합니다. [지금 계정 생성하기 →](http://app.wandb.ai/)
2. 터미널\(terminal\)의 프로젝트 폴더로 이동해서 저희 라이브러리를 설치합니다: `pip install wandb`
3. 프로젝트 폴더 내에서, W&B에 로그인합니다:`wandb login`

###  **Python 훈련 스크립트 설정하기**

1. . 라이브러리 `wandb`를 가져옵니다
2. 스윕이 초매개변수를 올바르게 설정했는지 확인하십시오. 스크립트 상단의 사전에 정의하고, wandb.init으로 전달합니다.
3. 메트릭을 로그하여 라이브 대시보드에서 확인합니다.

```python
import wandb

# Set up your default hyperparameters before wandb.init
# so they get properly set in the sweep
hyperparameter_defaults = dict(
    dropout = 0.5,
    channels_one = 16,
    channels_two = 32,
    batch_size = 100,
    learning_rate = 0.001,
    epochs = 2,
    )

# Pass your defaults to wandb.init
wandb.init(config=hyperparameter_defaults)
config = wandb.config

# Your model here ...

# Log metrics inside your training loop
metrics = {'accuracy': accuracy, 'loss': loss}
wandb.log(metrics)
```

 ​[전체 코드 예시 보기 →](https://github.com/wandb/examples/tree/master/examples/pytorch/pytorch-cnn-fashion)​

## 2. **스윕 구성\(Sweep Config\)**

YAML 파일을 설정하여 훈련 스크립트, 매개변수 범위\(parameter ranges\), 검색 전략\(search strategy\) 및 중지 기준\(stopping criteria\)을 지정합니다. W&B는 이러한 매개변수와 해당 값을 명령줄 전달인자\(command line arguments\)로 훈련 스크립트에 전달하며, 에서 여러분이 설정한 config 객체를 사용해 자동으로 분석합니다.

다음은 몇 가지 구성 리소스\(config resources\)입니다:

1.  [예시 YAML \(Example YAML\)](https://github.com/wandb/examples/blob/master/examples/pytorch/pytorch-cnn-fashion/sweep-grid-hyperband.yaml): 스크립트의 코드 예시 및 스윕을 수행하는 YAML 파일
2. [구성\(Configuration\)](https://docs.wandb.ai/v/ko/sweeps/configuration): 스윕 구성\(sweep config\)을 설정하는 전체 스펙\(spec\)
3. [Jupyter Notebook](https://docs.wandb.ai/v/ko/sweeps/python-api): YAML 파일 대신 Python 사전을 사용해 스윕 구성\(sweep config\)을 설정
4. [UI에서 구성\(config\) 생성하기](https://docs.wandb.ai/v/ko/sweeps/existing-project): 기존 W&B 프로젝트를 가져와 구성\(config\) 파일 생성하기
5.  [이전 실행의 피드\(Feed\)](https://docs.wandb.com/sweeps/overview/add-to-existing#seed-a-new-sweep-with-existing-runs): 이전 실행 가져와 새 스윕\(sweep\)에 추가하기

다음은 **sweep.yaml**이라고 불리는 스윕 구성 YAML 파일 예시입니다:

```text
program: train.py
method: bayes
metric:
  name: val_loss
  goal: minimize
parameters:
  learning_rate:
    min: 0.001
    max: 0.1
  optimizer:
    values: ["adam", "sgd"]
```

{% hint style="warning" %}
최적화할 메트릭을 지정하는 경우, 메트릭을 로그했는지 확인하시기 바랍니다. 이 예시에서, 구성\(config\) 파일에 val\_loss가 있으므로, 스크립트에 정확한 메트릭 이름을 로그해야 합니다:

`wandb.log({"val_loss": validation_loss})`
{% endhint %}

내부에서 이 예시 구성은 베이즈 최적화\(Bayes optimization\) 방법을 사용하여 프로그램 호출 시에 사용할 초매개변수 값 세트를 선택합니다. 이는 다음의 신택스\(syntax\)를 사용하여 실험을 시작합니다.

```text
python train.py --learning_rate=0.005 --optimizer=adam
python train.py --learning_rate=0.03 --optimizer=sgd
```

{% hint style="info" %}
스크립트에서 argparse를 사용하는 경우, 하이픈\(-\)대신 변수 이름에 밑줄\(\_\)을 사용하시는 것이 좋습니다.
{% endhint %}

## 3.  **스윕 초기화**

저희 중앙 서버는 스윕을 수행하는 모든 에이전트를 조정합니다. 스윕 구성 파일\(sweep config file\)을 설정하고 다음의 명령을 실행하여 시작합니다:

```text
wandb sweep sweep.yaml
```

이 명령은 개체이름\(entity name\) 및 프로젝트 이름을 포함한 **sweep ID**를 출력합니다. 다음 단계에서 사용할 내용을 복사하세요!

## 4. **에이전트\(들\) 실행\(Launch agent\(s\)\)**

스윕을 수행할 각 머신에서 스윕 ID\(sweep ID\)로 에이전트를 시작합니다. 동일 스윕을 수행하는 모든 에이전트에 대해 동일한 스윕 ID를 사용하는 것이 좋습니다.

여러분 머신의 셀\(shell\)에서 서버에 실행할 명령을 요청하는 wandb 에이전트 명령\(agent commend\)를 실행하십시오:

```text
wandb agent your-sweep-id
```

여러 머신 또는 동일 머신의 여러 프로세스에서 wandb 에이전트 할 수 있으며, 각 에이전트는 실행할 다음 초매개변수 세트를 위해 중앙 W&B 스윕 서버를 폴링\(poll\)합니다.

## 5.  **결과 시각화하기\(Visualize results\)**

프로젝트를 열어 스윕 대시보드에서 라이브 결과를 확인합니다

[예시 대시보드 →](https://app.wandb.ai/carey/pytorch-cnn-fashion)​

![](../.gitbook/assets/image%20%2888%29%20%282%29%20%283%29%20%283%29%20%283%29%20%283%29%20%283%29%20%283%29.png)

