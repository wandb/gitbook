---
description: 모델 버저닝에 대한 아티팩트 사용 가이드
---

# Model Versioning

W&B 아티팩트는 프로젝트 라이프사이클 전반에 걸쳐 머신 러닝 데이터세트 저장 및 구성을 도와드립니다.

### **일반 사용 사례** <a id="common-use-cases"></a>

1. 여러 머신에 걸쳐 모델을 전송하여 [안정적으로 버저닝 및 저장합니다](https://docs.wandb.ai/artifacts/model-versioning#version-and-store-reliably).
2. ​다양한 모델 아이디어를 구분하여, [브랜치\(branch\)에서 아이디어를 탐색합니다](https://docs.wandb.ai/artifacts/model-versioning#explore-ideas-in-branches).
3. ​ 여러 변형\(variants\)에 걸쳐 [모델을 정확하게 비교합니다](https://docs.wandb.ai/artifacts/model-versioning#compare-models-precisely).
4. ​ 심지어 종\(species\)이 증식하더라도 [모델 생태계\(model ecosystem\)를 관리합니다](https://docs.wandb.ai/artifacts/model-versioning#manage-a-model-ecosystem).
5. [데이터 워크플로를 시각화 및 공유하여](https://docs.wandb.ai/artifacts/model-versioning#visualize-and-easily-share-your-workflow) 모든 작업을 한곳에 둡니다.

### **유연한 추적 및 호스팅** <a id="flexible-tracking-and-hosting"></a>

 이러한 일반적인 경우 이외에도, 핵심 아티팩트 기능을 사용하여 데이터를 업로드, 버전, 별칭 붙이기\(alias\), 비교 및 다운로드할 수 있으며, S3, GCP, https를 통해 로컬 및 원격 파일 시스템에서 사용자 지정 데이터세트 워크플로를 지원합니다.

 이러한 기능에 대한 세부 정보는 [아티팩트 핵심 개념](https://docs.wandb.ai/artifacts/artifacts-core-concepts)을 참조하시기 바랍니다.

## **핵심 아티팩트 기능** <a id="core-artifacts-features"></a>

W&B 아티팩트는 다음의 기본 기능을 통해 모델 관리를 지원합니다:

1.  **업로드:** `run.log_artifact()`를 사용해 임의의 모델을 \(디렉토리 또는 임의의 포맷 파일로써\) 저장합니다. 또한 원시 콘텐츠\(raw contents\) 대신 링크 또는 URI를 사용하여 [참조로](https://docs.wandb.ai/artifacts/api#adding-references) 원격 파일 시스템\(예: S3 또는 GCP의 클라우드 저장소\)에서 데이터세트를 추적할 수도 있습니다.
2. **버전:** 아티팩트에  유형\(`"resnet50",` `"bert"`, `"stacked_lstm"`\) 및 이름`("my_resnet50_variant_with_attention")`을 지정하여 아티팩트를 정의합니다. 동일한 이름을 다시 로그하는 경우, W&B는 최신 콘텐츠를 포함한 새 버전의 아티팩트를 자동 생성합니다. 훈련 중에 아티팩트 버전을 사용하여 모델의 체크포인트를 설정할 수 있습니다. 새 모델 파일을 각 체크포인트에 동일한 이름으로 로그하기만 하시면 됩니다.
3.  **별칭\(alias\):** `"baseline"`, `"best"`, `"production"`과 같은 별칭을 설정하여 실험 및 개발된 모델의 계통\(lineage\)에서 중요한 버전을 강조 표시합니다.
4. **비교:** 임의의 두 버전을 선택하여 콘텐츠를 나란히 탐색합니다. 또한, 저희는 모델 입력 및 출력 시각화를 위한 툴도 개발 중입니다. [자세한 내용은 이곳을 참조하시기 바랍니다 →](https://docs.wandb.ai/datasets-and-predictions)**​**​
5. **다운로드:** 모델의 로컬 복사본을 \(예: 추론에 대한\) 가져오거나 아티팩트 콘텐츠를 참조로 확인합니다.

## **안정적인 버저닝 및 저장** <a id="version-and-store-reliably"></a>

자동 저장 및 버저닝을 통하여, 여러분께서 실행하는 각 실험은 가장 최근에 훈련된 모델 아티팩트를 W&B에 저장합니다. 이러한 모든 모델 버전을 스크롤하여, 주석 달기 및 필요한 경우 개발 히스토리\(development history\)를 유지하면서 이름 재설정을 할 수 있습니다. 어떤 실험 코드 및 구성이 어떤 가중치\(weights\)와 아키텍처를 생성했는지 정확하게 파악합니다. 여러분과 팀은 프로젝트, 하드웨어, dev 환경에 걸쳐 모델 체크 포인트를 다운로드 및 복구할 수 있습니다.

로컬 머신에서 모델을 훈련하고 아티팩트로 로그합니다. 각 훈련 실행은 모델 “inceptionV3”의 새 버전을 생성합니다.

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-MUxuBZhlYFKA7_UAH1j%2F-MUxuXUVCex7KaLX0r7f%2Fimage.png?alt=media&token=52a056ee-8cf5-4f37-b509-a89bfb7ca6b8)

Google Colab에서 추론을 위해 이름별로 동일한 모델을 로드하며, “latest” 버전을 사용하여 가장 최신 모델을 가져옵니다. 또한 인덱스별 또는 기타 맞춤형 별칭별로 다른 버전을 참조할 수 있습니다.

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-MUxuBZhlYFKA7_UAH1j%2F-MUxub24rge8jhpFPjT0%2Fimage.png?alt=media&token=d0bb61f9-10ed-483f-a996-b7c6debde476)

## **브랜치\(branches\)에서 아이디어 탐색** <a id="explore-ideas-in-branches"></a>

새 가설 테스트 또는 실험 세트 시작, 즉 핵심 아키텍처 변경 또는 주요 초매개변수 변경을 위해 새 이름 및 선택적으로 모델에 대한 아티팩트 유형을 생성합니다. 유형은 좀 더 광범위한 차이점에\(`"cnn_model"` vs `"rnn_model"`, `"ppo_agent`" vs `"dqn_agent"`\) 해당할 수 있으며, 이름은 더 많은 세부 정보 \("cnn\_5conv\_2fc", `"ppo_lr_3e4_lmda_0.95_y_0.97"` 등\)을 캡처할 수 있습니다. 동일한 이름으로 모델을 아티팩트의 버전으로 지정하여 작업을 간편하게 구성하고 추적할 수 있습니다. 코드 또는 브라우저에서 여러분은 개별 체크포인트를 설명 노트 또는 태그와 연결하고 \(추론, 미세조정 등을 위한\) 특정 모델 체크포인트를 사용하는 실험 실행\(experiment runs\)에 액세스할 수 있습니다. 아티팩트를 생성할 때 관련 메타데이터를 키-값 사전\(dictionary\)으로 \(예: 초매개변수 값, 실험 설정, 더 자세한 설명 텍스트\) 업로드 하실 수도 있습니다.

어떠한 모델 버전에서도 여러분께서는 노트를 작성하고 설명 태그 및 임의의 메타데이터를 추가하고 이 모델의 버전에서 로드된 모든 실험을 확인하실 수 있습니다.

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-MUxuBZhlYFKA7_UAH1j%2F-MUxufCN8Z4IROG_ZP7w%2Fimage.png?alt=media&token=de04ac36-7180-4072-bc90-bef1d85bbe58)

A partial view of an artifact tree showing two versions of an Inception-based CNN, iv3. A model checkpoint is saved before starting training \(with pre-existing ImageNet weights\) and after finishing training \(suffix \_trained\). The rightmost nodes show various inference runs which loaded the iv3\_trained:v2 model checkpoint and the test data in inat\_test\_data\_10:v0 \(bottom right\).![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-MUxuBZhlYFKA7_UAH1j%2F-MUxuinjAx_bI2yL2sDK%2Fimage.png?alt=media&token=bf70f9cb-d6c5-46ab-be13-4ae750cdbd71)

다음은 인셉션\(inception\) 기반 CNN인, iv3의 두 버전을 보여주는 아티팩트 트리의 일부 보기입니다. 모델 체크포인트는 훈련 시작 전 \(기존 ImageNet 가중치\(weights\)를 통해\) 및 훈련 종료 후에 \(suffix \_trained\) 저장됩니다. 가장 우측의 노드는 iv3\_trained:v2 모델 체크포인트 및  `inat_test_data_10:v0`의 \(우측 하단\) 테스트 데이터를 로드한 다양한 추론 실행을 보여줍니다. **다음은** `beyond roads iou 0.48`과 \(좌측 상단 노드\) `fastai baseline`\(베이스라인\)으로 \(좌측 하단 사각형 노드\) 이름이 지정된 두 훈련 실행을 \(prefixed train\) 중점적으로 다루는 복잡한 아티팩트 트리의 일부 보기입니다.

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-MUxuBZhlYFKA7_UAH1j%2F-MUxuoOKhL1CdIBo3aBq%2Fimage.png?alt=media&token=629b9754-6777-4a40-a552-48b35d6652ae)

##  **정확한 모델 비교** <a id="compare-models-precisely"></a>

로그된 또는 파생 메트릭\(예: 손실, 정확도, 평균 IoU\(intersection over union\)\)이나 동일 세트의 데이터를\(예: 테스트 또는 검증\) 기준으로 모델을 비교합니다. 다른 모델 변형을 시각화하고, 그 계통\(lineage\)을 추적하고 아티팩트 계산 그래프\(하단의 첫 번째 이미지\)를 통해 동일한 데이터 세트 버전을 사용하고 있는지 확인할 수 있습니다. 아티팩트 버전을 선택하여 여러분 및 동료가 남긴 노트를 확인할 수 있으며, 세부 정보를 자세히 살펴보거나 연결을 추적하여 실행 및 다른 아티팩트\(두 번째 이미지\)를 계산하거나 [데이터세트 및 예측을 \(베타\)](https://docs.wandb.ai/datasets-and-predictions) 통하여 모델 예측의 시각적 side-by-side diff 모드를 입력할 수 있습니다. 또한, W&B 작업 공간을 대시보드로 사용하여 프로젝트의 실행을 구성 및 쿼리하고 다운로드, 미세 조정 및 추가 분석\(마지막 이미지\)에 대한 특정 실행과 연관된 모델 아티팩트를 찾을 수 있습니다. 

이 아티팩트 트리는 12개의 모델 변형\(좌측 하단\)을 나타내며, test\_dataset: 14 entry\_predictions 및 2 predictions에서 두 세트의 예측\(predictions\)을 생성합니다. 이들은 19개의 결과 아티팩트\(계산 메트릭 및 이미지에 대한 ground truth 주석\)를 생산하도록 평가됩니다.

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-MUxuBZhlYFKA7_UAH1j%2F-MUxuugRG6h9JE18vk6q%2Fimage.png?alt=media&token=e4321ce7-dd5e-40c7-9d66-a9f1dac4839d)

여러 이름에서 버전을 선택하여 \(여기서는 여러 팀에서의 경쟁 벤치마크에 대한 모델 엔트리\) 세부 사항 및 연결된 실험 실행을 탐색합니다. 두 버전을 선택하는 경우 콘텐츠를 나란히 비교할 수 있습니다. \(시각적 비교의 경우 [데이터세트 및 예측 베타](https://docs.wandb.ai/datasets-and-predictions)를 참조하시기 바랍니다\)

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-MUxuBZhlYFKA7_UAH1j%2F-MUxuxe9yhhvO6QQR6YI%2Fimage.png?alt=media&token=4bc8080f-c1e9-4406-9176-a8f95e0f1f3a)

작업공간에서 보이는 각 실험 실행은 관련 아티팩트와 연결되어 있습니다. 특정 실행\(여기서는 팀명 “Daenerys”의 최상위 mean\_class\_iou\)을 찾아 해당 모델을 다운로드하십시오.

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-MUxuBZhlYFKA7_UAH1j%2F-MUxv0S1bGOF0TNMOhM9%2Fimage.png?alt=media&token=60b8d762-be95-4019-b994-58704a19c0b4)

## **모델 생태계 관리** <a id="manage-a-model-ecosystem"></a>

아티팩트 그래프는 데이터세트, 훈련 및 평가 코드 저장소\(repository\), 프로젝트 및 팀원들 간에 모델의 발전\(evolution\)을 기록하고 추적할 수 있게 만듭니다. 모델의 확산\(proliferation\)을 체계화하기 위해 다음을 수행할 수 있습니다.

*  **별칭\(aliases\)을 사용하여** W&B UI 또는 [코드](https://docs.wandb.ai/artifacts/api#updating-artifacts)에서 특정 모델을 `“baseline"`, `"production"`, `"ablation"`, 기타 맞춤형 태그로 지정합니다. 또한 더 긴 메모 또는 사전-스타일의 메타데이터를 추가할 수 있습니다.
* [아티팩트 API](https://docs.wandb.ai/artifacts/api#updating-artifacts)를 활용하여 아티팩트 그래프 및 스크립트 파이프라인을 트래버스\(traverse\) 합니다. 예: 훈련이 종료되면 새 모델을 자동으로 평가함
* 동적 업데이팅 [리포트](https://docs.wandb.ai/reports) 및 대시보드를 생성하여 타깃 메트릭에 대한 최고 퍼포먼스의 모델 및 다운스트림 사용을 위한 관련 모델 아티팩트 딥 링크를 표시합니다.
* 고정 아티팩트 유형을 통하여 모델의 계통\(lineage\)을 유지하며 최상의 퍼포먼스를 능가하는 모델만 저장합니다. 따라서 `“latest”` 별칭은 항상 해당 유형의 최고 모델 버전만을 가리킵니다.
* 실험을 실행할 때 고정 모델 아티팩트\(fixed model artifacts\)를 이름 및 별칭 \(또는 버전\) 별로 참조합니다. 개인 또는 팀에 걸쳐 모든 프로젝트는 동일한 모델 복사본을 사용합니다.

 프로젝트 대시보드에서 어떤 실행이 prod\_ready인지 확인하고 실행 이름을 클릭하여 다운로드할 해당 모델 아티팩트를 찾습니다.

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-MUxuBZhlYFKA7_UAH1j%2F-MUxv6Utp8Os9lwLCLss%2Fimage.png?alt=media&token=c27559e4-5d7a-4e42-b041-990120c17b04)

## **워크플로 시각화 및 공유** <a id="visualize-and-share-your-workflow"></a>

아티팩트를 통해 모델 개발 단계를 확인하고 공식화할 수 있으며, 전체 팀을 위하여 모든 모델 변형을 신뢰할 수 있는 방식으로 액세스하고 체계화하여, 각 각에 대한 하나의 신뢰할 수 있는 출처를 제공합니다.

* **팀이 생성하는 의미 있는 모델 유형:** 아티팩트 유형을 사용하여 다르게 명명된 아티팩트를 함께 계산 그래프에서 그룹화합니다. \(예: `"resnet_model"` vs `"inceptionV3_model"`, `"a2c_agent"` vs `"a3c_agent"`\) 그러면 유형 내의 다른 모델 아티팩트는 서로 다른 이름을 갖습니다. 주어진 명명된 아티팩트의 경우, 아티팩트의 버전이 연속 모델 체크포인트와 일치하는 것이 좋습니다. \[1\]
* **설\(hypothesis\) 또는 팀이 사용하는 탐색 브랜치\(branch\):** 어떤 파라미터 또는 코드 변경 사항이 어떤 모델 체크포인트를 발생시켰는지 쉽게 추적합니다. 아티팩트 그래프 graph \(입력 아티팩트 → 스크립트 또는 작업 → 출력 아티팩트\)를 탐색하며 \(데이터, 훈련 코드, 결과 모델 간의 모든 연결과 상호 작용합니다. 계산 그래프의 “explode”를 클릭하여 각 아티팩트의 모든 버전과 작업별 각 스크립트의 모든 실행을 확인하실 수 있습니다. 새 탭의 개별 노드를 클릭하여 추가 세부 정보를 확인하실 수 있습니다. \(파일 내용 또는 코드, 주석/메타데이터, 구성, 타임스탬프, 부모/자식 노드 등\)
* **의미 있는 인스턴스 포인터 또는 필요로 하는 별칭:** `"prod_ready"`, `"SOTA"`, `"baseline"`과 같은 별칭을 사용하여 팀 전체에 걸쳐 모델을 표준화\(standardize\)합니다. 이러한 별칭은 동일한 모델 체크포인트 파일을 확실하게 반환하며, 파일 시스템, 환경, 하드웨어, 사용자 계정 등에 걸쳐 더 확장할 수 있고 재현 가능한 워크플로를 용이하게 합니다.

 아티팩트를 사용하여, 확실하게 반복하실 수 있으며, 모든 실험에서 생성된 모델이 저장, 버저닝, 정리되어 간편하게 검색하실 수 있습니다. 사용하지 않는 아티팩트 정리는 브라우저 또는 [API](https://docs.wandb.ai/artifacts/api#cleaning-up-unused-versions)를 통해 간편하게 하실 수 있습니다.

다음은 워크플로 시각화를 위한 이러한 기능의 조합을 상세히 설명한 것입니다.

## **좀 더 상세한 예시: 계산 그래프 탐색** <a id="longer-example-compute-graph-exploration"></a>

​ [여기서 따라 해보세요 →](https://wandb.ai/stacey/evalserver_answers_2/workspace?workspace=user-stacey)​

 실험 간 최고의 결과를 탐색해보겠습니다. 여기서 후보 모델 평가는 비교 벤치마크에 입력됩니다. Swept-water-5은 \(녹색 박스 표시 안\) 클래스 IOU가 가장 높습니다. 실행 이름을 클릭하여 입력 및 출력 아티팩트를 확인하실 수 있습니다.

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-MUxuBZhlYFKA7_UAH1j%2F-MUxvCBPGmfq28tP-ccx%2Fimage.png?alt=media&token=67805f45-9abb-4dd5-bc24-6bcc15fdbd5d)

이 보기\(view\)는 실험 실행 "swept-water-5"의 입력 및 출력 아티팩트를 나타냅니다. 이 실행은 레이블된 테스트 데이터세트와 해당 데이터에 대한 모델 엔트리의 예측으로 판독되며, ground truth 레이블에 기초한 예측의 정확성을 평가하고 아티팩트로 결과를 저장했습니다. "entry\_predictions"를 클릭하여 어떻게 생성되었는지 확인할 수 있습니다.

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-MUxuBZhlYFKA7_UAH1j%2F-MUxvFILUmvQDSykVFhh%2Fimage.png?alt=media&token=f4d66610-f7c5-4cff-a91b-45dd1c147d97)

 이러한 모델 예측은 사이드바에 표시된 다양한 팀의 긴 제출 목록에서 벤치마크에 이르는 하나의 엔트리입니다. 이것은 "skilled-tree-6" 실행에서 생성되었습니다.

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-MUxuBZhlYFKA7_UAH1j%2F-MUxvIOk-QwVZc-PtLfg%2Fimage.png?alt=media&token=70f6181d-09dd-4c23-a983-63bf9a4d33f4)

 이러한 예측을 생성하는 데 사용된 모델을 확인합니다.

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-MUxuBZhlYFKA7_UAH1j%2F-MUxvKtmEQLUMAVAMDMB%2Fimage.png?alt=media&token=5052cb8d-1d48-4f41-a597-506cd137e9b4)

이 모델의 훈련 세부 사항과 평가를 위해 이를 사용하는 모든 실행을 확인합니다.

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-MUxuBZhlYFKA7_UAH1j%2F-MUxvNODyb4vPf6RceYe%2Fimage.png?alt=media&token=6ac093c8-c7b4-45b2-bc89-a88727b38987)

 훈련 이미지의 임의의 부분집합 및 검증 이미지의 고정 부분집합에서 각 에포크 이후에 저장된 모든 예측을 확인합니다.

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-MUxuBZhlYFKA7_UAH1j%2F-MUxvQhDE6_SkycejH1t%2Fimage.png?alt=media&token=0f510013-f056-44e5-b434-bc8153c7f141)

resnet18 아키텍처의 v3 attempt\(시도\) 모델 자체는 이 목록 끝에 출력 아티팩트로 나타납니다.

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-MUxuBZhlYFKA7_UAH1j%2F-MUxvT-bYfu11yK11xMw%2Fimage.png?alt=media&token=9557864f-637c-4c14-bc9c-a190ffe4a5b7)

**엔드노트**

 \[1\] 짧은 실험의 경우, 버전은 또한 뚜렷이 다른 모델 변형일 수도 있습니다\(예: 동일 모델 아키텍처에 대한 다양한 학습률\). 학습률의 측면에서 v4와 v6 사이에 정확히 무엇이 변경되었는지 사용자가 잊을 수 있기 때문에 몇 개의 수치 인덱스화된 버전 뒤에는 이것이 통제하기 힘들어질 수 있습니다. 여기서 별칭\(aliases\)을 유용하게 사용할 수 있으며, 한 이름의 상당히 다른 여러 모델 변형을 저글링 할 때 의미 있는 새로운 아티팩트 이름을 사용하시기 바랍니다.  
[  
](https://docs.wandb.ai/artifacts/dataset-versioning)

