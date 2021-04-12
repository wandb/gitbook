# Common Questions

## **프로젝트 및 개체\(entity\) 설정하기**

스윕을 시작하기 위해 명령을 실행하실 때, 해당 명령에 프로젝트와 개체\(entity\)를 제공해야 합니다. 다른 방식을 원하시는 경우, 다음의 4가지 방법으로 지정하실 수 있습니다:

1.  `wandb sweep`로의 명령줄 전달인자 \(`--project` 및 `--entity` 전달인자\)
2.  wandb 설정 파일 \(`project` 및 `entity` 키\)
3.   [스윕 구성\(sweep configuration\) ](https://docs.wandb.ai/v/ko/sweeps/configuration)\(`project` 및 `entity` 키\)
4. [환경 변수\(environment variables\)](https://docs.wandb.ai/v/ko/library/environment-variables) \(`WANDB_PROJECT` 및 `WANDB_ENTITY` 변수\)

### **첫 실행 완료 후 에이전트가 중단하는 스윕\(Sweeps agents stop after the first runs finish\)**

`wandb: ERROR Error while calling W&B API: anaconda 400 error: {"code":400,"message":"TypeError: bad operand type for unary -: 'NoneType'"}`

이 문제의 가장 일반적인 이유는 구성\(configuration\) YAML 파일에서 최적화하고 있는 메트릭이 여러분이 로그하고 있는 메트릭이 아니기 때문입니다. 예를 들면, 메트릭 f1을 최적화하고 있으나, **validation\_f1**을 로깅하는 중일 수 있습니다. 최적화하고 있는 바로 그 메트릭을 로깅하고 있는지 다시 한번 더 확인하시기 바랍니다.

##  **수행할 실행의 수 설정하기**

임의 검색은 사용자가 스윕을 중지할 때까지 영원히 실행됩니다. 메트릭에 대한 특정 값을 달성하면 자동으로 스윕을 중단하기 위해 목표\(target\)을 설정하거나, 에이전트가 수행할 실행의 수를 다음과 같이 지정할 수 있습니다:`wandb agent --count NUM SWEEPID`

wandb agent --count NUM SWEEPID

##  **Slurm에서 스윕 실행하기**

단일 훈련 작업을 실행한 다음 종료할 `wandb agent --count 1 SWEEP_ID`을 실행하실 것을 추천합니다.

## **그리드 검색 재실행** 

그리드 검색을 모두 사용했지만, 일부 실행을 다시 재실행하고 싶으신 경우, 재실행하려는 실행을 삭제한 다음 sweep control\(스윕 제어\) 페이지에서 resume\(재개\) 버튼을 클릭한 후, 해당 스윕 ID에 대한 새 에이전트를 시작합니다.

## **스윕과 실행은 반드시 동일 프로젝트에 존재해야 합니다\(Sweeps and Runs must be in the same project\).**

`wandb: WARNING Ignoring project='speech-reconstruction-baseline' passed to wandb.init when running a sweep`

스윕을 실행할 때 wandb.init\(\)으로 프로젝트를 설정하실 수 없습니다. 스윕과 실행은 반드시 동일 프로젝트에 위치해야 합니다. 따라서 프로젝트는 다음의 스윕 생성에 의해 설정됩니다: wandb.sweep\(sweep\_config, project=“fdsfsdfs”\)

## **업로드 시 오류 발생**

**ERROR Error uploading &lt;file&gt;: CommError, Run does not exist**가 표시되는 경우, 실행에 대한 ID를 설정중인 경우 일 수 있습니다. 이 ID는 프로젝트에서 고유\(unique\)해야 하며, 그렇지 않은 경우, 오류가 발생합니다. sweep context\(스윕 컨텍스트\)에서, 저희는 실행에 대한 자동으로 임의의, 고유한 ID를 생성하므로, 실행에 대한 수동으로 ID를 설정하실 수 없습니다

테이블과 그래프에 표시할 괜찮은 이름을 얻고 싶으시다면, id대신 name을 사용하시는 것이 좋습니다. 예를 들면 다음과 같습니다:

```python
wandb.init(name="a helpful readable run name")
```

##  **사용자 지정 명령으로 스윕하기**

일반적으로 명령과 전달인자로 훈련을 실행할 경우, 예를 들면 다음과 같습니다:

```text
edflow -b <your-training-config> --batch_size 8 --lr 0.0001
```

이것을 다음과 같은 스윕 구성\(sweeps config\)으로 변환할 수 있습니다:

```text
program:
  edflow
command:
  - ${env}
  - python
  - ${program}
  - "-b"
  - your-training-config
  - ${args}
```

${args} 키는 스윕 구성 파일의 모든 매개변수로 확장되며, argparse: --param1 value1 --param2에 의해 분석될 수 있도록 확장됩니다.

여러분이 사용할 수 있는 argparse와 함께 지정하고 싶지 않은 추가 args\(전달인자\)가 있는 경우: 다음을 사용하실 수 있습니다:  
parser = argparse.ArgumentParser\(\)  
args, unknown = parser.parse\_known\_args\(\)

 **Python 3으로 스윕 실행하기**

스윕\(sweep\)이 Python 2를 사용하려고 하는 문제가 있는 경우, Python 3을 대신 사용하도록 지정하는 법은 쉽고 간단합니다. 스윕 구성 YAML file\(sweep config YAML file\)에 다음을 추가합니다:

```text
program:
  script.py
command:
  - ${env}
  - python3
  - ${program}
  - ${args}
```

\*\*\*\*

##  **베이지안 최적화\(Byesian optimization\) 세부 정보**

베이지안 최적화\(Byesian optimization\)에 사용되는 가우시안 프로세스\(Gaussian process\) 모델은 저희 [오픈 소스 스윕 로직](https://github.com/wandb/client/tree/master/wandb/sweeps)에서 정의됩니다. 추가 구성 및 제어를 원하시는 경우 [Ray Tune](https://docs.wandb.com/sweeps/ray-tune)에 대한 지원을 받으시기 바랍니다.

저희는, [이곳](https://github.com/wandb/client/blob/541d760c5cb8776b1ad5fcf1362d7382811cbc61/wandb/sweeps/bayes_search.py#L30)의 오픈 소스 코드에서 정의된, RBF 일반화 버전인 Matern kernel을 사용합니다.

##  **스윕 일시 중지 vs. 스윕 wandb.agent 중지**

 스윕을 일시 중지해서 더 이상 사용 가능한 작업이 없을 때, `wandb agent`를 종료시키는 방법이 있나요?

일시 중지 대신 스윕을 중지한 경우, 에이전트는 종료됩니다. 일시 정지의 경우 저희는 에이전트가 계속 실행되도록 하여 에이전트를 재실행할 필요 없이 스윕이 재시작되도록 합니다.  


##  **스윕에서 구성 매개변수 설정을 위한 권장 방법**

`wandb.init(config=config_dict_that_could_have_params_set_by_sweep)`  
또는:  
`experiment = wandb.init()    
experiment.config.setdefaults(config_dict_that_could_have_params_set_by_sweep)`

이 방법의 이점으로 이렇게 할 경우 스윕에 의해 이미 설정된 키 설정이 무시된다는 점이 있습니다.

## **추가 범주형 변수를 스윕에 추가할 수 있는 방법 있나요? 아니면 새로 시작해야 되나요?**

일단 스윕이 시작되면, 스윕 구성을 변경하실 수 없습니다. 하지만, 다른 table view\(테이블 보기\)로 이동하여, 체크박스를 사용해 실행을 선택한 다음, “create sweep”\(스윕 생성\) 메뉴 옵션을 사용하여 이전 실행을 사용하는 새로운 스윕을 생성하실 수 있습니다.

