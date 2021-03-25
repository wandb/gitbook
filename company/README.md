# Company

###  **WandB란?**

Wandb는 머신 러닝을 위한 실험 추적 툴입니다. 저희는 머신 러닝을 사용하는 모든 분들이 실험을 추적하고 동료 및 미래의 자신과의 결과 공유를 쉽게 간편하게 도와드립니다

다음은 간략한 1분 소개 영상입니다. [예시 프로젝트 보기 →](https://app.wandb.ai/stacey/estuary)​

{% embed url="https://www.youtube.com/watch?v=icy3XkZ5jBk" %}

###  **어떻게 작동하나요?**

Wandb로 훈련코드를 계측하면, 저희 백그라운드 프로세스는 모델을 훈련할 때 발생하는 유용한 데이터를 수집합니다. 예를 들면, 저희는 모델 퍼포먼스 메트릭, 초매개변수, 경사\(gradients\), 시스템 메트릭, 출력 파일 및 가장 최근의 git 커밋\(commit\) 파일을 추적할 수 있습니다.

[Weights & Biases](https://docs.wandb.ai/)

###  **설정하는 것은 어렵나요?**

대부분의 사용자들이 emac 및 Google Sheets와 같은 툴을 사용하여 훈련을 추적한다고 알고 있습니다. 따라서 저희는 wandb를 가능한 한 가볍게 디자인했습니다. 통합\(Integration\)은 5-10분 정도 소요되며, wandb는 여러분의 훈련 스크립트를 느리게 하거나, 손상시키지 않습니다.

##  **wandb의 장점**

저희 사용자들은 다음의 세 가지를 wandb를 사용해서 얻을 수 있는 장점이라고 말합니다:

### 1. **훈련 시각화**

일부 사용자들은 wandb를 “persistent\(영구\) TensorBoard”로 받아들입니다. 기본값으로, 저희는 정확성 및 손실과 같은 모델 퍼포먼스 메트릭을 수집합니다. 또한 matplotlib 객체\(objects\), 모델 파일, GPU 사용률과 같은 시스템 메트릭 및 마지막 커밋 이후 최신 git 커밋 SHA + 변경사항 패치 파일 등을 수집하고 표시합니다.

또한, 여러분의 훈련 데이터와 함께 저장할 개별 실행에 관한 메모도 작성하실 수 있습니다. 다음은 저희가 Bloomberg를 가르친 수업에서의 관련 [예시 프로젝트](https://app.wandb.ai/bloomberg-class/imdb-classifier/runs/2tc2fm99/overview)입니다.

### 2.  **다양한 훈련 실행 구성 및 비교**

머신 러닝 모델을 훈련하는 대부분의 사용자들은 수많은 버전의 모델을 시도하고 있으며, 저희는 사용자들이 체계적인 상태를 유지할 수 있도록 돕는 것을 목표로 하고 있습니다.

여러분들께서는 프로젝트를 생성하여 모든 실행을 단일 장소에 보관하실 수 있습니다. 또한 수많은 실행에서 퍼포먼스 메트릭을 시각화 할 수 있으며, 원하시는 대로 필터링, 그룹화 및 태그할 수 있습니다.

 Stacey의 [estuary 프로젝트](https://app.wandb.ai/stacey/estuary)는 좋은 예시 중 하나입니다. 여러분은 사이드 바에서 실행 켜기/끄기를 하여 그래프에 표시하거나, 하나의 실행을 클릭하여 좀 더 자세히 살펴보실 수 있습니다. 모든 실행은 사용자들을 위해 통합된 작업 공간에 저장 및 정리됩니다.

![](../.gitbook/assets/image%20%2885%29%20%281%29%20%282%29%20%282%29.png)

### 3.  **결과 공유**

많은 실행을 일단 수행하고 나면, 어떠한 결과를 표시하도록 정리하는 것이 좋습니다. 이번에 Latent Space에서 근무하는 저희 친구들이 W&B 리포트를 사용하여 팀 생산성을 향상시키는 방법에 대해 서술한 [ML Best Practices: Test Driven Development](https://www.wandb.com/articles/ml-best-practices-test-driven-development)라는 괜찮은 보고서를 작성했습니다.

 Boris Dayma라는 사용자는 [Semantic Segmentation\(의미 분할\)](https://app.wandb.ai/borisd13/semantic-segmentation/reports?view=borisd13%2FSemantic%20Segmentation%20Report)에 관한 공개 예시 리포트를 작성했습니다. Boris는 보고서에서 그가 시도한 다양한 접근법과 그 방법이 얼마나 효과가 있는지에 대해 자세히 설명하고 있습니다.

저희는 wandb가 ML 팀이 보다 생산적으로 공동작업을 할 수 있도록 도움이 되길 바랍니다.

 여러 팀이 어떻게 wandb 활용하는지에 대해 자세히 알고 싶으시다면, [OpenAI](https://www.wandb.com/articles/why-experiment-tracking-is-crucial-to-openai)와 [Toyota Research](https://www.youtube.com/watch?v=CaQCw-DKiO8)에서 진행한 기술 사용자와의 녹화 인터뷰를 살펴보시기 바랍니다.  


## **팀**

공동작업자와 머신 러닝 프로젝트를 진행 중이신 경우, 저희는 여러분들이 결과를 보다 쉽게 공유할 수 있도록 해드립니다.

* [기업 팀](https://www.wandb.com/pricing): 저희는 소규모 스타트업 및 OpenAI 및 Toyota Research Institute와 같은 대기업 팀 모두 지원합니다. 저희는 여러분 팀의 요구에 맞는 유연한 가격 옵션을 가지고 있으며, 호스팅 클라우드, 프라이빗 클라우드 및 온프렘\(on-prem\) 설치를 지원합니다.
* [아카데믹 팀](https://www.wandb.com/academic): 저희는 학문적으로 명료하고 협력적인 연구를 지원하기 위해 헌신하고 있습니다. 여러분들께서 학자인 경우, 여러분들께 개인 프로젝트에서 연구를 공유할 수 있도록 무료 팀에 대한 액세스 권한을 부여해드립니다.

팀 외부의 사람들과 프로젝트를 공유하고 싶으시다면, navigation bar\(탐색 바\)에서 project privacy settings\(프로젝트 개인 정보 설정\)을 클릭하고, 프로젝트를 “Public\(공개\)”로 설정합니다. 링크를 공유한 모든 사용자는 여러분의 공개 프로젝트 결과를 확인할 수 있습니다.

