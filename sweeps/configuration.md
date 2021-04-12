---
description: '초매개변수 범위, 검색 전략 및 기타 swe 측면 설정을 위한 신택스'
---

# Configuration

다음의 구성 영역\(configuration fields\)를 사용하여 스윕을 사용자 정의합니다. 구성을 지정하는 방법으로 두 가지가 있습니다.

1.  [YAML 파일](https://docs.wandb.com/sweeps/overview/quickstart#2-sweep-config): 분산된 스윕\(distributed sweeps\)에 가장 적합합니다. [여기서](https://github.com/wandb/examples/tree/master/examples/keras/keras-cnn-fashion) 예시를 확인하세요. 
2. [Python 데이터 스트럭처](https://docs.wandb.ai/v/ko/sweeps/python-api): **​:** Jupyter Notebook에서 스윕을 실행하기에 가장 적합합니다.

| 최상위 수준 키 | 의미 |
| :--- | :--- |
| name | W&B UI에 표시되는 스윕의 이름 |
| description | 스윕에 대한 텍스트 설명 \(참고용\) |
| program | 실행할 훈련 스크립트 \(필수\) |
| metric | 최적화할 메트릭 지정 \(일부 검색 전략 및 중지 기준\(stopping criteria\)에 의해서 사용됨\) |
| method | [검색 전략](https://docs.wandb.com/sweeps/configuration#search-strategy) 지정 \(필수\) |
| early\_terminate | [중지 기준\(stopping criteria\)](https://docs.wandb.com/sweeps/configuration#stopping-criteria) 지정 \(선택 사항, 기본값으로 조기 중지 없음으로 설정됨\) |
| parameters | 검색할 [매개변수](https://docs.wandb.com/sweeps/configuration#parameters) 경계 지정 \(필수\) |
| project | 이 스윕에 대한 프로젝트 지정 |
| entity | 이 스윕에 대한 개체\(entity\) 지정 |
| command | 훈련 스크립트 실행 방법에 대한 [명령줄](https://docs.wandb.ai/v/ko/sweeps/configuration) 지정 |

###  **메트릭**

최적화할 메트릭을 지정합니다. 이 메트릭은 훈련 스크립트에 의해 분명하게 W&B로 로그되어야 합니다. 예를 들어, 모델의 검증 손실을 최소화하시려면 다음과 같습니다.

```python
# [model training code that returns validation loss as valid_loss]
wandb.log({"val_loss" : valid_loss})
```

| 메트릭 서브키\(sub-key\) | 의미 |
| :--- | :--- |
| name | **최적화할 메트릭의 이름** |
| goal | `minimize`\(최소화\) 또는 `maximize`\(최대화\) \(기본값: minimize\) |
| target | 최적화하는 메트릭에 대해 달성하고자 하는 값. 스윕에서 실행이 목표 값을 달성하면, 스윕의 상태는 “Finished”로 설정됩니다. 즉, 활성화된 실행과 함께 모든 에이전트는 해당 작업을 종료하지만, 스윕에서 새 실행이 시작되지는 않습니다. |

{% hint style="danger" %}
지정된 메트릭은 “최상위 수준\(top level\)” 메트릭 이어야 합니다.

This will **NOT** work:  
Sweep configuration:  
metric:  
name: my\_metric.nested  
Code:  
`nested_metrics = {"nested": 4}    
wandb.log({"my_metric", nested_metrics}`

To work around this limitation the script should log the nested metric at the top level like this:  
Sweep configuration:  
metric:  
name: my\_metric\_nested  
Code:  
`nested_metrics = {"nested": 4}    
wandb.log{{"my_metric", nested_metric}    
wandb.log({"my_metric_nested", nested_metric["nested"]})`
{% endhint %}

 **예시**

{% tabs %}
{% tab title="Maximize" %}
```text
metric:
  name: val_loss
  goal: maximize
```
{% endtab %}

{% tab title="Minimize" %}
```text
metric:
  name: val_loss
```
{% endtab %}

{% tab title="Target" %}
```text
metric:
  name: val_loss
  goal: maximize
  target: 0.1
```
{% endtab %}
{% endtabs %}

**검색 전략\(Search Strategy\)**

 스윕 구성\(sweep configuration\)에서 `method` 키\(key\)를 사용하여 검색 전략을 지정합니다.

| `method` | 의미 |
| :--- | :--- |
| grid | Grid\(그리드\) 검색은 모든 가능성 있는 매개변수 값의 조합에 대해서 반복합니다. |
| random | Random\(임의\) 검색은 임의의 값의 세트를 선택합니다. |
| bayes | Bayesian Optimization\(베이지언 최적화\)는 gaussian process\(가우시안 프로세스\)를 사용하여 함수를 모델링하고 개선 확률을 최적화하기 위해 매개변수를 선택합니다. 이 전략 수행하시려면 메트릭 키를 지정하셔야 합니다. |

 **예시** 

{% tabs %}
{% tab title="Random\(임의\) 검색" %}
```text
method: random
```
{% endtab %}

{% tab title="Grid\(그리드\) 검색" %}
```text
method: grid
```
{% endtab %}

{% tab title="Grid\(그리드\) 검색" %}
```text
method: bayes
metric:
  name: val_loss
  goal: minimize
```
{% endtab %}
{% endtabs %}

###  **중지 기준\(Stopping Criteria\)**

 조기 종료\(early termination\)는 퍼모먼스가 떨어지는 실행을 종료하여 초매개변수 검색의 속도를 높이는 선택적 기능입니다. 조기 종료가 실행되면, 에이전트는 현재 실행을 중지하고 수행할 다음 초매개변수 세트를 가져옵니다

| `early_terminate` sub-key | Meaning |
| :--- | :--- |
| type | 중지 알고리즘\(algorithm\) 지정하기 |

저희는 다음 중지 알고리즘을 지원합니다:

| `type` | Meaning |
| :--- | :--- |
| hyperband | [하이퍼밴드 방법\(hyperband method\)](https://arxiv.org/abs/1603.06560)을 사용합니다 |

Hyperband\(하이퍼밴드\) 중지는 프로그램 가동 동안 프로그램을 중지할지 또는 하나 이상의 브래킷\(bracket\)에서 계속하도록 허용할지 여부를 평가합니다. 브래킷\(Brackets\)은 특정 metric\(반복\(iteration\)은 메트릭이 로그된 횟수를 의미합니다. 즉, 메트릭이 에포크\(epoch\)마다 기록될 때, 에포크 반복\(epoch iterations\)가 있습니다\)에 대해 정적 반복\(static iterations\)으로 구성됩니다.

| `early_terminate` sub-key | Meaning |
| :--- | :--- |
| min\_iter | 첫 번째 브래킷에 대한 반복을 지정 |
| max\_iter | 프로그램에 대한 최대 반복 횟수 지정 |
| s | 브래킷의 총 개수 지정 \(max\_iter에 필요\) |
| eta | 브래킷 멀티플라이어\(braket multiplier\) 스케줄 지정 \(기본값: 3\) |

 **예시**

{% tabs %}
{% tab title="Hyperband \(min\_iter\)" %}
```text
early_terminate:
  type: hyperband
  min_iter: 3
```

Brackets: 3, 9 \(3\*eta\), 27 \(9 \* eta\), 81 \(27 \* eta\)
{% endtab %}

{% tab title="Hyperband \(max\_iter\)" %}
```text
early_terminate:
  type: hyperband
  max_iter: 27
  s: 2
```

Brackets: 9 \(27/eta\), 3 \(9/eta\)
{% endtab %}
{% endtabs %}

### **매개변수**

탐색할 초매개변수를 설명합니다. 각 초매개변수에 대하여, 상수\(constants\) 목록의 리스트 \(모든 방법의 경우\) 로 이름과 가능한 값을 지정하거나 분포를 지정합니다 \(`random` 또는 `Bayes` 경우\).

| 값 | 의미 |
| :--- | :--- |
| values: \[\(type1\), \(type2\), ...\] | 이 초매개변수에 대한 모든 유효값 지정. `grid`와 호환됩니다. |
| value: \(type\) | 이 초매개변수에 대한 단일 유효값 지정. `grid`와 호환됩니다. |
| distribution: \(distribution\) | 아래의 분포표에서 분포를 선택. 지정되지 않은 경우, 값이 설정된 경우 기본값으로 categorical 지정되며, max 및 min이 integers\(정수\)로 설정된 경우 int\_uniform으로, max 및 min이 floats로 설정된 경우 uniform으로, 또는 값이 설정된 경우 constant로 설정됩니다. |
| min: \(float\) max: \(float\) | `uniform` 하게 분포된 초매개변수에 대한 최대 및 최소 유효값 |
| min: \(int\) max: \(int\) | `int_uniform` 하게 분포된 초매개변수에 대한 최대 및 최소값 |
| mu: \(float\) | `normal` 또는 `lognormal` 하게 분포된 평균 매개변수 |
| sigma: \(float\) | `normal` 또는 `lognormal` 하게 분포된 초매개변수에 대한 표준 쳔차 매개변수 |
| q: \(float\) | 양자화된 초매개변수에 대한 양자화 단계 사이즈 |

**Example**

{% tabs %}
{% tab title="grid - single value" %}
```text
parameter_name:
  value: 1.618
```
{% endtab %}

{% tab title="grid - multiple values" %}
```text
parameter_name:
  values:
  - 8
  - 6
  - 7
  - 5
  - 3
  - 0
  - 9
```
{% endtab %}

{% tab title="random or bayes - normal distribution" %}
```text
parameter_name:
  distribution: normal
  mu: 100
  sigma: 10
```
{% endtab %}
{% endtabs %}

###  **분포**

| 이름 | 의미 |
| :--- | :--- |
| constant | 상수 분포. 반드시 `value`를 지정해야 합니다. |
| categorical | 카테고리 분포. 반드시 `values`를 지정해야 합니다. |
| int\_uniform | 정수에 대한 이산균등분포. 반드시 `max` 및 `min` 을 정수\(integers\)로 지정해야 합니다. |
| uniform | 연속균등분포. 반드시 `max` 및 `min`을 floats로 지정해야 합니다. |
| q\_uniform | 양자화된 균등분포`. X`가 균등한 `round(X / q) * q`을 반환합니다. q의 기본값은 1입니다. |
| log\_uniform |  로그-균등 분포. 자연로그\(natural logarithm\)이 `min`과 `max` 사이에 균일하게 분포되도록 `exp(min)`와 `exp(max)` 사이의 값을 반환합니다. |
| q\_log\_uniform | 양자화된 로그 균일\(log uniform\). X가 log\_uniform인 round\(X / q\) \* q을 반환합니다. q의 기본값은 1입니다. |
| normal | 정규 분포. 반환 값은 평균\(mean\) mu \(기본값 0\) 및 표준 편차 sigma \(기본값 1\)과 함께 정규적으로 분포됩니다. |
| q\_normal |  양자화된 정규 분포. `X`가 normal인 `round(X / q) * q`를 반환합니다. Q의 기본값은 1입니다. |
| log\_normal | 로그 정규 분포. 자연 로그 log`(X)`가 평균 `mu` \(기본값 0\) 및 표준 편차 `sigma` \(기본값 1\)과 함께 정규적으로 분포되도록 값 X를 반환합니다. |
| q\_log\_normal |  양자화된 로그 정규 분포. X가 log\_normal인 `round(X / q) * q`를 반환합니다`. q`의 기본값은 `1` 입니다. |

 **예시**

{% tabs %}
{% tab title="constant\(상수\)" %}
```text
parameter_name:
  distribution: constant
  value: 2.71828
```
{% endtab %}

{% tab title="categorical\(범주형\)" %}
```text
parameter_name:
  distribution: categorical
  values:
  - elu
  - celu
  - gelu
  - selu
  - relu
  - prelu
  - lrelu
  - rrelu
  - relu6
```
{% endtab %}

{% tab title="uniform" %}
```text
parameter_name:
  distribution: uniform
  min: 0
  max: 1
```
{% endtab %}

{% tab title="q\_uniform" %}
```text
parameter_name:
  distribution: q_uniform
  min: 0
  max: 256
  q: 1
```
{% endtab %}
{% endtabs %}

###  **명령줄\(Command Line\)** <a id="command"></a>

스윕 에이전트\(sweep agent\)는 기본값으로 다음 포맷으로 명령줄을 구축합니다.

```text
/usr/bin/env python train.py --param1=value1 --param2=value2
```

{% hint style="info" %}
Windows 머신에서는 the /usr/bin/env가 생략됩니다. UNIX 시스템에서는 환경에 따라 올바른 python interpreter가 선택되었는지 확인하시기 바랍니다.
{% endhint %}

구성 파일\(configuration file\)에서 명령 키를 지정하여 이 명령줄을 수정할 수 있습니다

기본값으로 명령은 다음과 같이 정의됩니다:

```text
command:
  - ${env}
  - ${interpreter}
  - ${program}
  - ${args}
```

| 명령 매크로 | 확장 |
| :--- | :--- |
| ${env} | UNIX 시스템에서 /usr/bin/env, Windows에서 생략됨 |
| ${interpreter\| | “python”으로 확장. |
| ${program} | 스윕 구성 `program` 키에 의해 지정된 훈련 스크립트 |
| ${args} | --param1=value1 --param2=value2 형식으로 확장된 전달인자 |
| ${args\_no\_hyphens} | param1=value1 param2=value2 형식으로 확장된 전달인자 |
| ${json} | JSON으로 인코딩된 전달인자 |
| ${json\_file} | JSON으로 인코딩된 args를 포함한 파일의 경로 |

 예시:

{% tabs %}
{% tab title="python interpreter 설정하기" %}
python interpreter를 하드코드 하기 위해 다음과 같이 분명하게 interpreter를 지정할 수 있습니다.

```text
command:
  - ${env}
  - python3
  - ${program}
  - ${args}
```
{% endtab %}

{% tab title="추가 매개변수 하기" %}
스윕 구성 매개변수에 의해 지정되지 않은 추가 명령줄 전달인자를 추가합니다:

```text
command:
  - ${env}
  - ${interpreter}
  - ${program}
  - "-config"
  - your-training-config
  - ${args}
```
{% endtab %}

{% tab title="전달인자 생략하기" %}
프로그램이 전달인자 분석을 사용하지 않는 경우, 전달인자를 모두 함께 전달하지 않고, 스윕 매개변수를 자동으로 선택하는 `wandb.init()`를 활용하실 수 있습니다:

```text
command:
  - ${env}
  - ${interpreter}
  - ${program}
```
{% endtab %}

{% tab title="Hydra와 함께 사용" %}
명령을 번경하여 Hydra가 예상하는 방식으로 전달인자를 전달할 수 있습니다.

```text
command:
  - ${env}
  - ${interpreter}
  - ${program}
  - ${args_no_hyphens}
```
{% endtab %}
{% endtabs %}

##  **공통 질문**

**네스트 구성\(Nested Config\)**

현재, 스윕은 네스트 값\(nested values\)를 지원하지 않지만, 가까운 미래에 지원할 예정입니다.

