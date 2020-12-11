---
description: '파이프라인 전반에 걸친 버전 데이터, 모델 및 결과'
---

# Artifacts

## **개요**

W&B 아티팩트를 사용해서 머신 러닝 파이프라인 전반에 걸친 데이터세트, 모델, 평가 결과를 저장 및 추적하세요. 아티팩트를 버전 데이터 폴더\(versioned folder of data\)로 간주하십시오. 아티팩트에 전체 대이터 집합을 직접 저장하거나 아티팩트 참조 사용하여 다른 시스템의 데이터에 나타낼 수 있습니다

##  **구동 방식**

아티팩트 API를 사용하여, 아티팩트를 W&B 실행 출력으로 로깅하거나 실행으로의 입력으로 사용하실 수 있습니다.

![](../.gitbook/assets/simple-artifact-diagram-2.png)

실행은 입력으로써 다른 실행의 출력 아티팩트 사용할 수 있으므로, 아티팩트와 실행은 함께 방향그래프를 형성합니다. 미리 파이프라인을 정의할 필요는 없습니다. 그냥 아티팩트를 사용, 로그 하시면, 저희가 모든 것을 꿰어\(stitch\) 놓겠습니다.

다음은 DAG의 요약 보기와 각 단계 및 모든 아티팩트 버전의 모든 실행의 축소 보기를 확인할 수 있는 [예시 아티팩트](https://app.wandb.ai/shawn/detectron2-11/artifacts/model/run-1cxg5qfx-model/4a0e3a7c5bff65ff4f91/graph)입니다.

![](../.gitbook/assets/2020-09-03-15.59.43.gif)

아티팩트 사용법에 대해서 알아보시려면, [아티팩트 API Docs →](https://docs.wandb.com/artifacts/api)를 참조해주십시오.

