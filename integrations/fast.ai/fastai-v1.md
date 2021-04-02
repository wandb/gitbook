---
description: '참고: 이 문서는 fastai v1를 위한 문서입니다.'
---

# fastai v1

fastai의 현재 버전을 사용중인 경우, [fastai 페이지](https://docs.wandb.com/library/integrations/fastai)를 참조하십시오. 

fastai v1를 사용하는 스크립트의 경우, 모델 토폴로지, 손실, 메트릭, 가중치\(weights\), 경사\(gradients\), 샘플 예측 및 최고수준으로 훈련된 모델을 자동으로 로그할 수 있는 콜백\(callback\)이 있습니다.

```text
import wandbfrom wandb.fastai import WandbCallback​wandb.init()​learn = cnn_learner(data,                    model,                    callback_fns=WandbCallback)learn.fit(epochs)
```

요청된 로그된 데이터는 콜백\(callback\) 생성자\(constructor\)를 통해 구성할 수 있습니다.

```text
from functools import partial​learn = cnn_learner(data, model, callback_fns=partial(WandbCallback, input_type='images'))
```

또한, 훈련을 시작할 때만 WandbCallback을 사용하시는 것도 가능합니다. 이 경우, 인스턴트화 되어야 합니다.

```text
learn.fit(epochs, callbacks=WandbCallback(learn))
```

사용자 정의 매개변수는 이 단계에서도 지정할 수 있습니다.

```text
learn.fit(epochs, callbacks=WandbCallback(learn, input_type='images'))
```

## **예시 코드** <a id="example-code"></a>

통합 작업 방식을 확인하기 위한 몇 가지 예시를 만들어봤습니다.

**Fastai v1**

* [심슨 캐릭터 분류\(Classify Simpsons characters](https://github.com/borisdayma/simpsons-fastai)\)​[: ](https://app.wandb.ai/jxmorris12/huggingface-demo/reports/A-Step-by-Step-Guide-to-Tracking-Hugging-Face-Model-Performance--VmlldzoxMDE2MTU)Fastai 모델을 추적 및 비교하는 간단한 데모
*  ​[Fastai를 사용한 의미 분할\(Semantic Segmentation with Fastai\)](https://github.com/borisdayma/semantic-segmentation): 자율주행 차의 신경망\(neural networks\) 최적화

##  **옵션** <a id="options"></a>

`WandbCallback()` 클래스는 다양한 옵션을 지원합니다:

| 키워드 전달인자 | 기본값 | 설명 |
| :--- | :--- | :--- |
| learn | N/A | 훅\(hook\)할 fast.ai 학습자\(learner\) |
| save\_model | True | 각 단계에서 개선된 경우 모델을 저장합니다. 또한 훈련이 종료될 때 최고의 모델을 로드합니다. |
| mode | auto | min', 'max', 'auto': 단계 간, `monitor`에서 지정된 훈련 메트릭을 비교하는 방법 |
| monitor | None | 최고의 모델 저장을 위해 퍼포먼스를 측정하는 데 사용되는 훈련 메트릭.None은 검증 손실\(validation loss\)로기본 설정 됩니다. |
| log | gradients | "gradients", "parameters", "all", 또는 None. 손실 & 메트릭은 항상 로그됩니다. |
| input\_type | None | "images" 또는 None. 샘플 예측을 나타내는 데 사용됩니다. |
| validation\_data | None | input\_type이 설정된 경우 샘플 예측에 사용되는 데이터 |
| predictions | 36 | input\_type이 설정되고validation\_data이 None인 경우 수행할 예측 횟수 |
| seed | 12345 | input\_type이 설정되고validation\_data이 None인 경우 샘플예측에 대한 임의의 생성기\(generator\)를 초기화합니다. |

[  
](https://docs.wandb.ai/integrations/fastai)

