---
description: 모델의 초매개변수와 출력 메트릭 간의 관계를 시각화합니다
---

# Parameter Importance

이 패널은 어떤 초매개변수가 최고의 예측변수\(predictors\)였는지, 그리고 메트릭의 이상값\(desirable value\)와 높은 상관관계에 있는지를 나타냅니다.

![](https://paper-attachments.dropbox.com/s_B78AACEDFC4B6CE0BF245AA5C54750B01173E5A39173E03BE6F3ACF776A01267_1578795733856_image.png)

**상관관계\(Correlation\)**은 초매개변수와 선택된 메트릭\(이 경우 val\_loss\) 간의 선형 상관관계입니다. 따라서 높은 상관관계는 초매개변수가 높은 값은 가지고 있을 때, 메트릭 또한 높은 값을 가지고 있으며 그 반대의 경우도 마찬가지임을 의미합니다. 상관관계는 살펴보기에 좋은 메트릭이나 입력\(inputs\) 간의 이차 상호 작용\(second order interactions\)을 담아내지는 못하며, 입력\(inputs\)를 완전히 다른 범위와 비교하는 것은 복잡해질 수 있습니다.

따라서 저희는 입력\(inputs\)으로써의 초매개변수와 함께 랜덤 포레스트\(random forest\)를 훈련하는 **importance\(중요도\)** 메트릭도 계산합니다.’

초매개변수를 입력\(inputs\)으로, 메트릭을 대상 출력\(target output\)으로 사용해 랜덤 포레스트를 훈련하는 **importance\(중요도\)** 메트릭을 계산하며, 랜덤 포레스트에 대한 특성 중요도 값\(feature importance values\)을 리포트합니다.

**​**[Fast.ai](http://fast.ai/)에서 초매개변수 공간\(hyperparameter spaces\)를 탐색하기 위한 랜덤 포레스트 특성 중요도\(random forest feature importances\)의 사용을 개척해온 [Jeremy Howard](https://twitter.com/jeremyphoward)와의 대화에서 이 기술에 대한 아이디어를 얻었습니다. 이 분석 뒤의 동기에 대해서 더 알아보시려면 Jeremy Howard의 경의로운 [강의](http://course18.fast.ai/lessonsml1/lesson4.html) \(및 [노트들](https://forums.fast.ai/t/wiki-lesson-thread-lesson-4/7540)\)을 확인해 보시기 바랍니다.  


이 초매개변수 높은 상관관계를 가진 초매개변수간의 복잡한 상호 작용을 풀어냅니다. 이렇게 하면, 모델 퍼포먼스 예측 측면에서 어떤 초매개변수가 가장 중요한지 보여줌으로써 여러분의 초매개변수 검색을 미세조정할 수 있도록 돕습니다.

##  **초매개변수 중요도 패널 생성**

Weights & Biases Project로 이동합니다. 없는 경우, [이 프로젝트](https://app.wandb.ai/sweep/simpsons)를 사용하실 수 있습니다.

프로젝트 페이지에서 Add Visualization\(시각화 추가\)을 클릭하세요.

![](https://paper-attachments.dropbox.com/s_B78AACEDFC4B6CE0BF245AA5C54750B01173E5A39173E03BE6F3ACF776A01267_1578795570241_image.png)

 **그 후, Parameter Importance\(매개변수 중요도\)를 선택합니다.**

여러분의 프로젝트로 [Weights & Biases 통합하기\(integrating Weights & Biases\)](https://docs.wandb.com/quickstart)외에 새로운 코드를 작성하실 필요는 없습니다.

![](https://paper-attachments.dropbox.com/s_B78AACEDFC4B6CE0BF245AA5C54750B01173E5A39173E03BE6F3ACF776A01267_1578795636072_image.png)

### **초매개변수 중요도 패널 해석하기**

![](https://paper-attachments.dropbox.com/s_B78AACEDFC4B6CE0BF245AA5C54750B01173E5A39173E03BE6F3ACF776A01267_1578798509642_image.png)

이 패널은 여러분의 훈련 스크립트에 [wandb.config](https://docs.wandb.com/library/python/config) 객체로 전달된 모든 매개변수를 보여줍니다. 다음으로, 특성 중요도\(feature importances\) 및 선택한 모델 메트릭\(이 경우, val\_loss\)에 대한 cofig parameters\(구성 매개변수\)의 상관관계를 표시합니다.

### **중요도\(Importance\)**

중요도 열\(Importance column\)은 각 초매개변수가 선택된 메트릭을 예측하는데 유용한 정도를 나타냅니다. 과도한 초매개변수를 조정하고 이 플롯을 이용하여 어떤 것이 더 탐색할 가치가 있는지에 관심을 쏟는 것으로 시작하는 시나리오를 상상할 수 있습니다. 그 다음 스윕은 가장 중요한 초매개변수로 제한되어, 따라서 더 나은 모델을 좀 더 빠르고 저렴하게 찾을 수 있습니다.

참고: 트리 기반 모델이 범주형 데이터\(categorical data\)와 정규화 되지 않은 데이터 모두에 더 강한 내성이 있으므로, 일반화 저희는 선형모델 보다는 트리 기반 모델을 사용해서 중요도를 계산합니다. 전술한 패널에서, 저희는 `epochs`, `learning_rate`, `batch_size` 및 `weight_decay`가 꽤 중요했다는 것을 확인할 수 있었습니다.

 다음 단계로써, 이러한 초매개변수의 더 미세한 값은 탐색하는 다른 스윕을 실행할 수도 있습니다. 재밌게도, `learning_rate`와 `batch_size`는 중요했지만, 출력\(output\)과의 상관관계는 별로 없었습니다.  


이는 저희에게 상관관계에 대해 알려줍니다.

###  **상관관계**

상관관계는 개별 초매개변수와 메트릭 값 간의 선형 관계를 포착합니다. 즉, SGD optimizer와 val\_loss라고 하는 초매개변수를 사용하는 것 사이에 중요한 관계가 있습니까? 같은 질문에 답을 합니다 \(이 경우, 답은 yes입니다\). 상관관계 값의 범위는 -1부터 1이며, 여기서 양\(positive\)의 값은 양의 선형 상관관계를 나타내고, 음\(negative\)의 값은 음의 선형 상관관계를 나타내며, 0의 값은 상관관계를 나타내지 않습니다. 일반적으로 어느 한 방향에서 0.7을 초과하는 값은 강한 상관관계를 나타냅니다.

이 그래프를 사용해서 우리의 메트릭과 더 높은 상관관계를 가지는 값을 탐색하거나 \(이 경우, rmsprop 또는 nadam 보다는 stochastic gradient descent\(확률적 경사 하강법\) 또는 adam을 선택할 수도 있습니다. 더 많은 에포크\(epochs\)를 위한 훈련을 할 수도 있습니다.

상관관계 해석에 대한 빠른 참고 노트입니다:

* 상관관계는 반드시 인과관계\(causation\)이 아닌, 연관성의 증거\(evidence of association\)을 나타냅니다.
* 인과관계는 이상치\(outliers\)에 민감하며, 특히, 시도된 초매개변수 샘플 사이즈가 작은 경우, 강한 관계에서 중간 정도의 관계로 전환 할 수 있습니다.
* 그리고, 마지막으로, 상관관계는 초매개변수와 메트릭 간의 선형 관계만을 포착합니다. 강한 다항 관계\(polynomial relationship\)이 있는 경우, 상관관계에 의해 포착되지 않습니다.

 중요도와 상관관계간의 차이는 중요도가 초매개변수 간의 상호작용은 설명하지만, 반면에 상관관계는 오직 메트릭 값에 대한 개별 초매개변수의 영향만을 측정한다는 사실에서 비롯됩니다. 두 번째로, 상관관계는 오직 선형 관계만을 포착하지만, 반면에 중요도는 더 복잡한 관계를 포착할 수 있습니다.

여러분들도 보시다시피, 중요도 및 상관관계 모두 초매개변수가 모델 퍼포먼스에 얼마나 영향을 미치는지를 이해하기 위한 강력한 툴입니다.

저희는 이 패널을 통해서 이러한 통찰력을 가지고 더 빨리 강력한 모델에관심을 쏟으시기를 바랍니다.  
****

