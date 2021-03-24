---
description: 각 모델의 훈련 실행은 더 큰 프로젝트 내에서 구성된 전용 페이지가 할당됩니다.
---

# Run Page

 실행 페이지를 사용해서 모델의 단일 버전에 대한 상세 정보를 탐색할 수 있습니다.

##  **개요 탭\(Overview Tab\)**

* 실행 이름, 설명 및 태그
* 호스트 이름, 운영 체제, 실행을 Python 버전 및 실행을 시작한 명령
* [wandb.config](https://docs.wandb.com/library/config)​를 사용해 저장된 구성 매개변수\(config parameters\)
* [wandb.log\(\)](https://docs.wandb.com/library/log)을 사용해 저장된 요약 매개변수\(summary parameters\) 리스트. 기본값으로 마지막으로 로그된 값으로 설정

[라이브 예시 보기 →](https://app.wandb.ai/carey/pytorch-cnn-fashion/runs/munu5vvg/overview?workspace=user-carey)​  


![](../../.gitbook/assets/run-page-overview-tab.png)

 페이지 자체를 공개로 설정하시더라도 Python 세부 정보는 비공개입니다. 여기 좌측엔 incognito\(시크릿 창 모드\)에서의 실행 페이지 예시이며, 우측은 제 계정입니다.

![](../../.gitbook/assets/screen-shot-2020-04-07-at-7.46.39-am.png)

##  **차트 탭\(Chart Tab\)**

* 검색, 그룹화 및 시각화 정렬
* 그래프에서 연필 아이콘 ✏️을 클릭해서 편집합니다
  * x 축, 메트릭 및 범위 변경
  * 차트의 범례\(legends\), 제목 및 색 편집
* 이러한 차트를 얻으려면, wandb.log\(\)를 통해 데이터를 로그하시기 바랍니다.

 [라이브 예시 보기 →](https://app.wandb.ai/wandb/examples-keras-cnn-fashion/runs/wec25l0q?workspace=user-carey)​

![](../../.gitbook/assets/image%20%2837%29.png)

## **시스템 탭\(System Tab\)**

* CPU 사용률, 시스템 메모리, disk I/O, 네트워크 트래픽, GPU 사용률, GPU 온도, 메모리 엑세스에 소요된 GPU 시간, 할당 GPU 메모리 및 GPU 파워 사용량을 시각화합니다.
* Lambda Labs에서 저희 시스템 메트릭 사용에 관한 포스팅을 작성했습니다. ​ 

  [라이브 예시 보기 →](https://app.wandb.ai/wandb/feb8-emotion/runs/toxllrmm/system)​ 

![](../../.gitbook/assets/image%20%2888%29%20%282%29%20%283%29.png)

##   **모델 탭\(Model Tab\)**

*  모델의 레이어, 매개변수의 수 및 각 레이어의 출력 모양\(output shape\)을 확인하실 수 있습니다.

[라이브 예시 보기 →](https://app.wandb.ai/stacey/deep-drive/runs/pr0os44x/model)​ 

![](../../.gitbook/assets/image%20%2829%29%20%281%29%20%282%29%20%282%29.png)

##  **로그 탭\(Logs Tab\)**

* 명령줄, 모델을 훈련하는 머신의 stdout 및 stderr에 개제된 출력
* 저희는 마지막 1000줄을 보여드립니다. 실행이 종료된 후, 전체 로그 파일을 다운로드하고 싶으신 경우, 우측상단의 download\(다운로드\)버튼을 클릭하시기 바랍니다.

 [라이브 예시 보기 →](https://app.wandb.ai/stacey/deep-drive/runs/pr0os44x/logs)​

![](../../.gitbook/assets/image%20%2869%29%20%284%29%20%286%29%20%287%29.png)

##  **파일 탭\(Files Tab\)**

* [wandb.save\(\)](https://docs.wandb.com/library/save)를 사용하는 실행과 동기화 할 파일을 저장하세요 — AI용 Dropbpx입니다.
* 모델 체크포인트, 검증 세트 예시 등을 보관합니다.
* diff.patch를 사용해 코드의 정확한 버전을 [복구](https://docs.wandb.com/library/restore)합니다.

**🌟**새로운 권장사항: 입력 및 출력 추적을 위해 [아티팩트\(Artifacts\)](https://docs.wandb.ai/artifacts)를 사용해 보시기 바랍니다.

  [라이브 예시 보기 →](https://app.wandb.ai/stacey/deep-drive/runs/pr0os44x/files/media/images)​ 

![](../../.gitbook/assets/image%20%283%29.png)

