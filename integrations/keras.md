---
description: How to integrate a Keras script to log metrics to W&B
---

# Keras

Keras 콜백\(callback\)을 사용하여 `model.fit` 에서 추적된 모든 메트릭과 손실값\(loss values\)을 자동으로 저장합니다.

{% code title="example.py" %}
```python
import wandb
from wandb.keras import WandbCallback
wandb.init(config={"hyper": "parameter"})

# Magic

model.fit(X_train, y_train,  validation_data=(X_test, y_test),
          callbacks=[WandbCallback()])
```
{% endcode %}

 ****[**colab notebook**](https://colab.research.google.com/github/wandb/examples/blob/master/colabs/keras/Simple_Keras_Integration.ipynb)**에서 통합\(Integration\)을 실행하거나,** [**비디오 자습서**](https://www.youtube.com/watch?v=Bsudo7jbMow&ab_channel=Weights%26Biases)**를 학습하거나, 전체 스크립트 예제를 보시려면** [**예시 프로젝트\(example projects\)**](https://docs.wandb.com/examples)**를 참조하십시오.**  


#### **옵션**

 Keras `WandbCallback()` 클래스는 다양한 옵션을 지원합니다:

| 키워드 전달인자 | 기본값 | 설명 |
| :--- | :--- | :--- |
| monitor | val\_loss |  최고의 모델을 저장하기 위한 퍼포먼스를 측정하는 데 사용되는 훈련 메트릭. 즉, val\_loss |
| mode | auto | 'max', 또는 'auto': 단계별 `monitor`에서 지정된 훈련 메트릭을 비교하는 방법 |
| save\_weights\_only | False | 전체 모델 대신 가중치\(weights\)만을 저장합니다 |
| save\_model | True | 각 단계에서 모델이 개선된 경우 저장합니다 |
| log\_weights | False | 각 에포크\(epoch\)에서 각 레이어 매개변수의 값을 로그합니다. |
| log\_gradients | False | 각 에포크\(epoch\)에서 각 레이어 파라미터의 경사\(gradients\)를 로그합니다. |
| training\_data | None | 경사\(gradients\)를 계산하는 데 필요한 튜플\(X,y\) |
| data\_type | None | 저장 중인 데이터의 유형으로, 현재 오직 “image\(이미지\)”만 지원합니다 |
| labels | None | data\_type이 지정된 경우에만 사용되며, 분류기\(classifier\)를 생성하는 경우, 숫자 출력\(numeric output\)을 변환할 라벨의 리스트입니다 \(이진 분류 지원\) |
| predictions | 36 | data\_type이 지정된 경우 수행할 예상의 수. 최대값은 100입니다. |
| generator | None | 데이터 증가\(augmentation\) 및 data\_type을 사용하는 경우 예측을 할 생성기\(generator\)를 지정하실 수 있습니다. |

## **공통질문**

### **wandb를 통해 Keras 다중처리\(multiprocessing\) 사용**

`use_multiprocessing=True` 을 설정하고 오류 `Error('You must call wandb.init() before wandb.config.batch_size')` 가 표시된다면, 다음을 수행하시기 바랍니다  
****

1. Sequence class init에서, 다음을 추가하세요: `wandb.init(group='...')`
2. 메인 프로그램에서, `if name == "main"`: 을 사용 중인지 확인하고, 나머지 스크립트 로직\(script logic\)을 그 안에 입력하십시오.

##  **예시**

사용자를 위한 통합\(integration\)의 작동의 몇 가지 예시를 만들어보았습니다.

*   [**Github의 예시\(Example on Github\)**](https://github.com/wandb/examples/blob/master/examples/keras/keras-cnn-fashion/train.py)**: Python 스크립트에서 Fashion MNIST 예시**
* **Google Colab에서 실행: 시작하기 위한 간단한 notebook 예제**
* [**Wandb 대시보드\(Dashboard\)**](https://app.wandb.ai/wandb/keras-fashion-mnist/runs/5z1d85qs)**: W&B에서 결과 보기**

