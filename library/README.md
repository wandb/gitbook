---
description: '실험 추적, 데이터세트 버저닝, 모델 관리용 Weights & Biases'
---

# Library

wandb Python 라이브러리를 사용하여 단 몇 줄의 코드로 머신러닝 실험을 추적할 수 있습니다. [PyTorch](about:blank) 또는 [Keras](about:blank)와 같이 인기 있는 프레임워크를 사용하고 계시는 경우, 저희는 가벼운 [통합](about:blank)기능을 제공하고 있습니다.

##  **스크립트에 W&B 통합하기**

 다음은 W&B로 실험을 추적할 수 있는 간단한 빌딩 블록입니다. 저희는 또한 [PyTorch](https://docs.wandb.ai/v/ko/integrations/pytorch), [Keras](https://docs.wandb.ai/v/ko/integrations/keras), [Scikit](https://docs.wandb.ai/v/ko/integrations/scikit) 등에 대하여 다양한 특별 통합\(integrations\)를 제공하고 있습니다. 자세한 내용은[ 통합](https://docs.wandb.ai/v/ko/integrations)기능을 참조해 주십시오.

1. \*\*\*\*[**wandb.init\(\)**:](https://docs.wandb.ai/v/ko/library/init) 스크립트 상단에 새 실행을 초기화합니다. 이를 통해서 실행 객체가 반환되고 모든 로그 및 파일이 저장된 후 비동기식으로 W&B로 스트리밍 되는 로컬 디렉토리를 생성합니다. 저희의 호스팅 클라우드 서버 대신 개인 서버를 사용하려는 경우, 저희는 [자체 호스팅\(Self-Hosting](https://docs.wandb.ai/self-hosted)\) 서비스를 제공하고 있습니다.
2. [ **wandb.config**](https://docs.wandb.ai/v/ko/library/config): 학습률 또는 모델 유형과 같은 초매개변수 사전을 저장합니다. 구성\(config\)에서 캡처한 모델 설정은 나중에 결과 구성 및 쿼리 시에 유용합니다.
3. \*\*\*\*[**wandb.log\(\)**](https://docs.wandb.ai/v/ko/library/log): 시간 경과에 따라 정확성 및 손실과 같은 훈련 반복 루프\(training loop\)에 메트릭을 로그 합니다. 기본값으로 wandb.log\(\)를 호출하면, 히스토리 객체에 새로운 단계를 추가하고 요약 객체를 업데이트합니다.
   * **히스토리**: 시간 경과에 따라 메트릭을 추적하는 사전 같은 객체의 배열입니다. 이러한 시계열\(time series\) 값은 기본값 라인 플롯으로 UI에 표시됩니다.
   * **요약**: 기본값으로 wandb.log\(\)로 로그된 메트릭의 최종 값입니다. 메트릭에 대한 요약을 수동으로 설정하여 최종 값 대신 가장 높은 정확도 또는 가장 낮은 손실을 캡처할 수 있습니다. 이러한 값들은 실행을 비교하는 테이블, 플롯에 사용됩니다. 즉, 프로젝트에서 모든 실행 과정을 높은 정확도로 시각화할 수 있습니다.
4.  [**아티팩트\(Artifacts\)**](https://docs.wandb.ai/v/ko/artifacts): 모델 가중치 또는 예측 테이블과 같은 실행의 출력을 저장합니다. 이를 통해서 모델 훈련뿐만 아니라 최종 모델에 영향을 미치는 모든 파이프라인을 추적하실 수 있습니다.

## **모범 사례**

`wandb` 라이브러리는 놀라울 정도로 유연합니다. 다음은 몇 가지 지침입니다.

1.  **구성**: 초매개변수, 아키텍처, 데이터 세트 및 모델 재현을 위해 사용하고자 하는 모든 항목을 추적합니다. 열에 표시되며, 구성 열을 사용하여 앱의 실행을 동적으로 분류, 정리, 필터링할 수 있습니다.
2.  **프로젝트**: 프로젝트는 함께 비교할 수 있는 실험의 세트입니다. 각 프로젝트에는 전용 대시보드 페이지가 있으며, 다른 모델 버전을 비교하기 위해 서로 다른 실행 그룹을 쉽게 켜거나 끌 수 있습니다.
3.  **노트**: 본인에게 보내는 간단한 커밋 메시지로, 노트는 스크립트에서 설정할 수 있으며 테이블에서 편집할 수 있습니다.
4.  **태그**: 기준\(baseline\) 실행 및 즐겨찾기 실행을 식별합니다. 태그를 사용하여 실행을 필터링하실 수 있으며, 테이블에서 편집할 수 있습니다.

```python
import wandb

config = dict (
  learning_rate = 0.01,
  momentum = 0.2,
  architecture = "CNN",
  dataset_id = "peds-0192",
  infra = "AWS",
)

wandb.init(
  project="detect-pedestrians",
  notes="tweak baseline",
  tags=["baseline", "paper1"],
  config=config,
)
```

##   **어떤 데이터가 로그되나요?**

 스크립트에 로그된 모든 데이터는 로컬로 wandb 디렉토리 내의 머신에 로컬로 저장되며, 그 다음 W&B 클라우드 또는 개인 서버에 동기화됩니다.

### **자동 로그**

* **시스템 메트릭**: CPU 및 GPU 활용, 네트워크 등. [nvidia-smi](https://developer.nvidia.com/nvidia-system-management-interface)에서 제공되며, 실행 페이지의 시스템 탭에 표시됩니다.
* **명령 행**: stdout 및 stderr이 선택되어 실행 페이지의 로그 탭에 표시됩니다.

 [설정 페이지](https://wandb.ai/settings)에서 코드 저장을 켜서 다음을 가져옵니다:

* **git 커밋:** 가장 최근의 키트 커밋을 선택한 후 실행 페이지의 개요 탭에서 확인하며 또한 커밋되지 않은 변경사항이 있는 경우 diff.patch 파일을 확인합니다.
* **파일**: requirements.txt 파일 및 실행에 대하여 **wandb** 디렉토리에 저장한 모든 파일은 업로드 되며 실행 페이지의 파일 탭에 표시됩니다.

###  **특정 호출 로그**

데이터 및 모델 메트릭의 경우, 여러분께서 정확하게 로그할 대상을 결정하실 수 있습니다.

* **데이터 세트:** W&B로 스트리밍 하려면 이미지 및 또는 기타 데이터 세트 샘플을 구체적으로 로그하셔야 합니다.
* **PyTorch 경사: wandb.watch\(model\)을 추가하여 UI에서 경사를 히스토그램으로 확인합니다**
* **구성**: 초매개변수, 데이터세트 링크 같은 방식으로 제출된 구성 매개변수로 사용하고 있는 아키텍처의 이름을 로그합니다. 값을 전달하는 방법은 다음과 같습니다: `wandb.init(config=your_config_dictionary).`
* **메트릭**: wandb.log\(\)를 사용하여 모델의 메트릭을 확인합니다. 훈련 반복 루프에 정확도 및 손실과 같은 메트릭을 로그하면, UI에서 실시간으로 업데이트된 그래프를 볼 수 있습니다.

