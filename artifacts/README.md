---
description: '파이프라인 전반에 걸친 버전 데이터, 모델 및 결과'
---

# Artifacts

데이터세트 버저닝, 모델 버저닝, 머신러닝 파이프라인 간의 종속성\(depencencies\) 및 결과 추적에 W&B 아티팩트를 사용하세요. 

아티팩트를 버저닝된 데이터 폴더라고 생각하세요. 전체 데이터세트를 직접 아티팩트에 저장할 수 있으며, 또는 아티팩트 참조\(artifact references\)를 사용하여 S3, GCP, 또는 여러분의 자체 시스템과 같은 다른 시스템의 데이터를 가리킬 수 있습니다.

​데이터세트 및 모델 버저닝에 대한 아티팩트 사용의 엔드-투-엔드 예시에 대해서 살펴보시려면 [W&B 아티팩트 가이드](https://wandb.ai/wandb/arttest/reports/Artifacts-Quickstart--VmlldzozNTAzMDM)를 참조하시기 바랍니다.

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-MRQAIh244AIbK4o5I0U%2F-MRQAKR6eXsRqXtDZACU%2Fkeras%20example.png?alt=media&token=88920f3e-680c-414f-ac12-c4dc7db7c931)

## **빠른 시작** <a id="quickstart"></a>

​[​![](https://colab.research.google.com/assets/colab-badge.svg)​](http://wandb.me/artifacts-quickstart)​

### 1. **아티팩트 로그하기** <a id="1-log-an-artifact"></a>

실행을 초기화하고 아티팩트를 로그합니다. 예: 모델 훈련에 사용하는 데이터세트 버전

```text
run = wandb.init(job_type="dataset-creation")artifact = wandb.Artifact('my-dataset', type='dataset')artifact.add_file('my-dataset.txt')run.log_artifact(artifact)
```

### 2. **아티팩트 사용하기** <a id="2-use-the-artifact"></a>

새 실행을 시작하고 저장된 아티팩트를 풀다운\(pull down\)합니다. 예: 데이터세트를 사용하여 모델 훈련

```text
run = wandb.init(job_type="model-training")artifact = run.use_artifact('my-dataset:latest')artifact_dir = artifact.download()
```

### 3. **새 버전\(version\) 로그하기** <a id="3-log-a-new-version"></a>

아티팩트가 변경된 경우, 동일한 아티팩트 생성 스크립트를 재실행합니다. 이 경우, my-dataset.txt 파일의 데이터가 변경되었다고 생각해보십시오. 이 동일한 스크립트는 새 버전을 깔끔하게 캡처합니다. 저희는 아티팩트를 체크섬\(checksum\)하고, 변경된 점을 확인하며, 새 버전을 추적합니다. 변경 사항이 없는 경우, 데이터를 업로드하거나 새 버전을 생성하지 않습니다.

```text
run = wandb.init(job_type="dataset-creation")artifact = wandb.Artifact('my-dataset', type='dataset')# Imagine more lines of text were added to this text file:artifact.add_file('my-dataset.txt')# Log that artifact, and we identify the changed filerun.log_artifact(artifact)# Now you have a new version of the artifact, tracked in W&B​
```

 실제 모델 훈련에 대한 더 자세한 예시를 찾고 계신가요? [W&B 아티팩트 가이드](https://wandb.ai/wandb/arttest/reports/Guide-to-W-B-Artifacts--VmlldzozNTAzMDM)를 참조하시기 바랍니다.

## **작동 방식** <a id="how-it-works"></a>

저희 아티팩트 API를 사용하시면 아티팩트를 W&B 실행의 출력으로 로그할 수 있으며, 또는 실행의 입력으로 사용하실 수 있습니다.

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-M94QAXA-oJmE6q07_iT%2F-M94QJCXLeePzH1p_fW1%2Fsimple%20artifact%20diagram%202.png?alt=media&token=94bc438a-bd3b-414d-a4e4-aa4f6f359f21)

실행은 다른 실행의 출력 아티팩트를 입력으로 사용할 수 있으므로, 아티팩트와 실행이 함께 방향 그래프\(directed graph\)를 구성합니다. 사전에 파이프라인을 정의하실 필요가 없으며, 아티팩트를 사용하고 로그만 하시면 저희가 모든 것을 연결하겠습니다.

다음은 DAG의 요약 보기\(summary view\)와 각 단계 및 모든 아티팩트 버전의 전체 실행의 축소 보기를 확인하실 수 있는 [예시 아티팩트](https://app.wandb.ai/shawn/detectron2-11/artifacts/model/run-1cxg5qfx-model/4a0e3a7c5bff65ff4f91/graph)입니다.

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-MGLA6RWM_CgTAYNmodM%2F-MGLC6PaQUcw4SpqfE0E%2F2020-09-03%2015.59.43.gif?alt=media&token=5b13721b-31d7-4bda-922a-2e7a993d1cc3)

## **아티팩트 자료** <a id="artifacts-resources"></a>

데이터 및 모델 버저닝에 대한 아티팩트 사용법을 자세히 살펴보시기 바랍니다.

1. ​ [아티팩트 핵심 개념](https://docs.wandb.ai/v/ko/artifacts/artifacts-core-concepts)​​
2. ​ [아티팩트 상세 설명](https://docs.wandb.ai/v/ko/artifacts/artifacts-walkthrough)​​
3. ​ [데이터세트 버저닝](https://docs.wandb.ai/v/ko/artifacts/dataset-versioning)​​
4. ​ [모델 버저닝](https://docs.wandb.ai/v/ko/artifacts/model-versioning)​​
5. ​[ 아티팩트 FAQ​​](https://docs.wandb.ai/v/ko/artifacts/api)
6. [​ 아티팩트 예시](https://docs.wandb.ai/v/ko/artifacts/examples)​​
7. ​[아티팩트](https://docs.wandb.ai/ref/artifact) 참조 문서

## **W&B 아티팩트 비디오 튜토리얼**  <a id="video-tutorial-for-w-and-b-artifacts"></a>

양방향 [튜토리얼](https://www.youtube.com/watch?v=Hd94gatGMic)을 따라 연습해보시면서 W&B 아트팩트를 통해 머신러닝 파이프라인 추적 방법을 학습하시기 바랍니다.[  
](https://docs.wandb.ai/app/features/panels/scatter-plot)

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-MOoWaLZbumzeOryYOzo%2F-MOoX6IY3tIoiQJhwojw%2Fwandb%20artifacts%20video.png?alt=media&token=9e6a2f97-160a-46f7-b3c9-645cc8129602)

