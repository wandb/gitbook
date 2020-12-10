# Data Import/Export API

사용자 정의 분석을 위해 데이터프레임을 가져오거나 비동기식으로 데이터를 완료된 실행에 추가합니다. 자세한 사항은 [API 참조](https://docs.wandb.com/ref/api)를 확인하시기 바랍니다.

###  **인증**

 다음 두 가지 방법 중 하나를 통해 [API 키](https://wandb.ai/authorize)로 머신을 인증하실 수 있습니다:

1. 명령줄에서 `wandb login`를 실행하고 API키를 붙여 넣습니다.
2. **WANDB\_API\_KEY**  **환경 변수를 API 키로 설정합니다.**

###  **실행 데이터 내보내기**

완료된 활성화된 실행에서 데이터를 다운로드합니다. 일반적인 용도는 Jupyter notebook에서 사용자 정의 분석을 위한 데이터프레임 다운로드 또는 자동화된 환경에서 사용자 정의 로직\(logic\)을 사용하는 것을 포함합니다.

```python
import wandb
api = wandb.Api()
run = api.run("<entity>/<project>/<run_id>")
```

 가장 일반적으로 사용되는 실행 객체의 속성은 다음과 같습니다:

| 속성 | 의미 |
| :--- | :--- |
| run.config | 초매개변수와 같은 모델 입력\(model inputs\)에 대한 사전\(dictionary\) |
| run.history\(\) | 모델이 손실과 같은 훈련을 하는 동안, 변하는 값을 저장하도록 의도된 사전의 리스트. wandb.log\(\) 명령은 객체에 추가됩니다. |
| run.summary | 출력\(outputs\) 사전. 이는 정확도 또는 손실과 같은 스칼라\(scalars\)부터 대형 파일일 수 있습니다. 기본값으로 wandb.log\(\) 명령은 요약을 로그된 시계열\(timeseries\)의 타임최종값으로 설정합니다. 또한 직접 설정하실 수도 있습니다. |

 또한, 과거 실행의 데이터를 수정 및 업데이트할 수 있습니다. 기본값으로, api의 단일 인스턴스는 모든 네트워크 요청을 캐시\(cashe\)합니다. 실행중인 스크립트에서 실시간 정보를 요구하는 경우, api.flush\(\)를 호출하여 업데이트된 값을 가져옵니다.

###  **샘플링**

 기본값 히스토리 방법은 메트릭을 고정된 수의 샘플로 샘플링합니다 \(기본값은 500이며, samples 전달인자를 통해 이를 변경하실 수 있습니다\). 대형 실행의 모든 데이터를 내보내기 하시려면, run.scan\_history\(\) 방법을 사용하실 수 있습니다. 자세한 사항은 [API 참조](https://docs.wandb.com/ref/api)를 확인하시기 바랍니다.

###  **다중실행 쿼리하기**

{% tabs %}
{% tab title="MongoDB Style" %}
W&B API는 또한 api.runs\(\)를 사용하여 프로젝트의 실행에 걸쳐 쿼리를 할 수 있는 방법을 제공합니다. 가장 일반적인 사용 사례는 사용자 정의 분석을 위한 실행 데이터를 내보내기 하는 것입니다. 쿼리 인터페이스는 [MongoDB uses](https://docs.mongodb.com/manual/reference/operator/query)의 것과 같습니다.

```python
runs = api.runs("username/project", {"$or": [{"config.experiment_name": "foo"}, {"config.experiment_name": "bar"}]})
print("Found %i" % len(runs))
```
{% endtab %}

{% tab title="Dataframes and CSVs" %}
제 스크립트는 프로젝트를 찾고, 이름, 구성\(configs\) 및 요약 통계와 함께 실행의 CSV를 출력합니다.

```python
import wandb
api = wandb.Api()

# Change oreilly-class/cifar to <entity/project-name>
runs = api.runs("<entity>/<project>")
summary_list = [] 
config_list = [] 
name_list = [] 
for run in runs: 
    # run.summary are the output key/values like accuracy.  We call ._json_dict to omit large files 
    summary_list.append(run.summary._json_dict) 

    # run.config is the input metrics.  We remove special values that start with _.
    config_list.append({k:v for k,v in run.config.items() if not k.startswith('_')}) 

    # run.name is the name of the run.
    name_list.append(run.name)       

import pandas as pd 
summary_df = pd.DataFrame.from_records(summary_list) 
config_df = pd.DataFrame.from_records(config_list) 
name_df = pd.DataFrame({'name': name_list}) 
all_df = pd.concat([name_df, config_df,summary_df], axis=1)

all_df.to_csv("project.csv")
```
{% endtab %}
{% endtabs %}

 `api.runs(...)` 호출은 반복 가능하며 리스트와 같이 작동하는 **Runs** 객체를 반환합니다. 객체는 요청에 따라 시퀀스\(sequence\)에 50개의 실행을 한 번에 로드하며, **per\_page 키워드 전달인자를 통해 페이지당 로드되는 수를 변경하실 수도 있습니다.**

 `api.runs(...)`은 또한 **order** 키워드 전달인자를 허용합니다. 기본값 순서\(order\)는 created\_at이며, 오름차순으로 결과를 가져오려면 +created\_at를 지정하십시오. 또한 구성\(config\) 또는 요약 값\(summary values\)별 정렬을 하실 수 있습니다. 즉, `summary.val_acc` 또는 `config.experiment_name`입니다.

###  **오류 처리**

W&B 서버와 통신 하는 동안 오류가 발생하는 경우, `wandb.CommError`가 발생합니다. **exc** 속성을 통해 원래 예외\(original exception\)을 내부검사\(introspect\)를 할 수 있습니다.

###  **API를 통해 최신 git 커밋 가져오기**

  UI에서 run\(실행\)을 클릭하고 실행 페이지의 Overview\(개요\)탭을 클릭하여 최신 git 커밋을 확인합니다. `wandb-metadata.json` 파일에도 있습니다. 공용API를 사용하여 **run.commit**을 통해 git 해시\(hash\)를 가져올 수 있습니다.

##  **공통 질문**

###  **matplotlib 또는 seaborn에서 시각화 할 데이터를 내보내기**

 몇 가지 일반적인 내보내기 패턴에 대한 [API 예시](https://docs.wandb.com/v/master/library/api/examples)를 확인하시기 바랍니다. 또한 사용자 정의 플롯 또는 확장된 실행 테이블의 download\(다운로드\) 버튼을 클릭하여 브라우저에서 CSV를 다운로드 하실 수 있습니다.

###  **스크립트에서 임의의 실행 ID 및 실행 이름 가져오기**

 `wandb.init()` 호출 후, 다음과 같은 스크립트에서 임의의 실행 ID 또는 인간이 읽을 수 있는 실행 이름\(human readable run name\)에 액세스할 수 있습니다.

* 고유한 실행 ID \(8자 해시\): `wandb.run.id`
* 임의의 실행 이름 \(인간이 읽을 수 있는\):: `wandb.run.name`

 실행을 위한 유용한 식별자\(identifiers\)를 설정하는 방법을 생각중인 경우, 저희는 다음을 추천합니다:

* **Run ID**: 생성된 해시\(hash\)로 남겨 둡니다. 프로젝트의 모든 실행에서 고유해야 합니다.
* **Run name**: 차트의 서로 다른 선 간의 차이를 구별할 수 있도록, 짧고, 읽을 수 있고, 가급적 고유한 것이어야 합니다.
* **Run notes\(실행 노트\)**: 실행에서 여러분이 수행하는 것에 대한 간략한 설명을 적어두기에 좋습니다. `wandb.init(notes="your notes here")`를 통해 이를 설정할 수 있습니다.
* **Run tags\(실행 태그\)**: 실행 태그에서 동적으로 추적하고, UI에서 필터를 사용하여 여러분이 관심 있는 실행 까지 테이블을 필터링 합니다. 스크립트에서 태그를 설정하고, 그 다음 실행 테이블 및 실행 페이지의 개요 탭의 UI에서 편집하실 수 있습니다.

