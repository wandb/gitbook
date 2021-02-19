---
description: Hyperparameter search and model optimization
---

# Sweeps

Weights & Biases 스윕\(Sweeps\)을 사용해서 초매개변수 최적화를 자동화하고 가능한 모델의 공간을 탐색을 하실 수 있습니다.Weights & Biases 스윕\(Sweeps\)을 사용해서 초매개변수 최적화를 자동화하고 가능한 모델의 공간을 탐색을 하실 수 있습니다.

##  **W&B 스윕 사용의 장점**

1.  **빠른 설정\(Quick setup\)**: 단 몇 줄의 코드만으로 W&B 스윕을 실행하실 수 있습니다.
2. **Transparent\(명료성\)**: 저희는 저희가 사용하는 모든 알고리듬\(algorithms\)을 인용하고 있으며, [저희 코드는 오픈 소스입니다](https://github.com/wandb/client/tree/master/wandb/sweeps).
3. **Powerful\(강력함\)**: 저희의 스윕은 완전히 사용자 정의 및 환경 설정 하실 수 있습니다. 수 십개의 머신에서 스윕을 실행하실 수 있으며, 노트북에서 스윕을 시작하는 것 만큼이나 간단합니다.

##  **공통 질문**

1. **Explore\(탐색\)**: 효율적으로 초매개변수 조합의 공간을 샘플링하여 유망한 영역\(promising region\)을 발견하고, 모델에 대한 직관을 구축합니다.
2. **Optimize\(최적화\)**: 최적 퍼포먼스의 초매개변수 세트를 찾기 위해 스윕을 사용하실 수 있습니다.
3. **K-fold cross validation\(K-겹 교차검증\)**: 다음은 W&b를 사용한 K-겹 교차검증의 [간략한 코드 예시](https://github.com/wandb/examples/tree/master/examples/wandb-sweeps/sweeps-cross-validation) 입니다.

##  **처리 방법\(Apporach\)**

1.  **wandb 추가하기**: Python 스크립트에서 몇 줄의 코드를 추가하여 초매개변수를 로그하고 스크립트에서 메트릭을 출력합니다. [지금 시작하세요 ](https://docs.wandb.com/sweeps/quickstart)
2. **config\(구성\) 작성하기**: 스윕할 변수와 범위를 정의합니다. 검색 전략\(search strategy\)를 선택합니다. 저희는 grid\(그리드\), random\(임의\), 및 베이지안\(Bayesian\) 검색 뿐 아니라 조기 중지\(early stopping\)을 지원합니다. [여기](https://github.com/wandb/examples/tree/master/examples/keras/keras-cnn-fashion)에서 몇 가지 예시 구성을 확인하세요.
3. **스윕 초기화하기\(Initialize sweep\)**: 스윕 서버를 시작합니다. 중앙 컨트롤러를 호스트하고 스윕을 수행하는 에이전트\(agents\)간 조정을 수행합니다.
4.  **에이전트 시작하기\(Launch agent\(s\)\)**: 스윕에서 모델을 훈련하는 데 사용할 각 머신에 이 명령을 실행합니다. 에이전트는 다음에 시도할 초매개변수를 중앙 스윕서버에 요청하고 그 다음, 실행을 수행합니다.
5. **결과 시작화하기\(Visualize results\)**: 라이브 대시보드를 열어 하나의 중앙 영역에서 모든 결과를 확인합니다.

![](../.gitbook/assets/central-sweep-server-3%20%282%29%20%282%29%20%282%29.png)

{% page-ref page="quickstart.md" %}

{% page-ref page="existing-project.md" %}

{% page-ref page="configuration.md" %}

{% page-ref page="local-controller.md" %}

{% page-ref page="python-api.md" %}

{% page-ref page="sweeps-examples.md" %}

