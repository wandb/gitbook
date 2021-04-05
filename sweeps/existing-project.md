---
description: '이미 wandb.init, wandb.config, wandb.log을 사용 중이신 경우, 여기서 시작하세요!'
---

# Sweep from an existing project

기존 W&B 프로젝트가 있는 경우, 초매개변수 스윕으로 모델 최적화를 쉽고 간단하게 시작할 수 있습니다. 작업 예시와 함께 각 단계를 설명해 드리겠습니다. 우선 저의 [W&B 대시보드](https://app.wandb.ai/carey/pytorch-cnn-fashion)를 열어보십시오. 저는 [이 예시 repo](https://github.com/wandb/examples/tree/master/examples/pytorch/pytorch-cnn-fashion)의 코드를 사용하고 있으며, 이는 [패션 MNIST 데이터세트](https://github.com/zalandoresearch/fashion-mnist)에서 이미지를 분류하기 위해 PyTorch 합성곱 신경망\(convolutional neural network\)을 훈련합니다.  


## **1. 프로젝트 생성하기**

 첫 번째 기준선 실행\(baseline run\)을 수동으로 실행 W&B 로깅이 제대로 작동하는지 확인하세요. 다음의 간단한 예제 모델을 다운로드하고, 몇 분 동안 훈련한 다음, 웹 대시보드에 나타나는 예시를 확인하십시오

* 다음의 repo를 복제하세요. `git clone https://github.com/wandb/examples.git`
* 다음의 예시를 여세요. `cd examples/pytorch/pytorch-cnn-fashion`
* 실행을 수동 실행하세요. `python train.py`

 [예시 프로젝트 페이지 보기 →](https://app.wandb.ai/carey/pytorch-cnn-fashion)

## 2.  **스윕 생성하기**

프로젝트 페이지에서, 사이드 바에서 Sweep\(스윕\) 탭을 열고 “스윕 생성”\(Create Sweep\)을 클릭합니다.

![](../.gitbook/assets/sweep1.png)

자동 생성된 구성\(config\)은 여러분께서 이미 수행한 실행을 기반으로 스윕할 값을 추측합니다. 구성\(config\)을 편집하여 사용자께서 수행할 초매개변수의 범위를 지정합니다. 스윕을 실행하면, 저희의 호스팅된 W&B 스윕 서버에서 새 프로세스가 시작됩니다. 이 중앙집중식 서비스는, 훈련 작업을 실행하는 여러분의 머신인, 에이전트를 조정합니다.

![](../.gitbook/assets/sweep2.png)

## 3. **에이전트 실행하기**

 다음 에이전트를 로컬범위로 시작합니다. 작업을 분배하고 스윕을 보다 빨리 끝내고 싶으시다면, 여러 머신에서 병렬로 수십 개의 에이전트를 실행하실 수 있습니다. 에이전트는 다음에 수행할 매개변수 세트를 출력합니다.

![](../.gitbook/assets/sweep3.png)

이제 다 됐습니다. 이제 스윕을 실행하고 있습니다. 다음은 저의 예시 스윕이 시작될 때 대시보드가 어떤 모습인지를 나타낸 것입니다. [예시 프로젝트 페이지 보기 →](https://app.wandb.ai/carey/pytorch-cnn-fashion)**​**​

![](https://paper-attachments.dropbox.com/s_5D8914551A6C0AABCD5718091305DD3B64FFBA192205DD7B3C90EC93F4002090_1579066494222_image.png)

## **새 스윕을 기존 실행으로 시드\(seed\)하기**

이전에 로그한 기존 실행을 사용하여 새 스윕을 실행합니다.

1. 프로젝트 테이블을 엽니다.
2. 테이블 좌측에 있는 체크박스에 사용할 실행을 선택합니다.
3. 드롭다운을 클릭하여 새 스윕을 생성합니다.

이제 여러분의 스윕이 저희 서버에 설치됩니다. 이제 여러분이 하셔야 하는 것은 하나 또는 그 이상의 에이전트를 실행하여 실행을 실행하는 것입니다.

![](../.gitbook/assets/create-sweep-from-table%20%281%29%20%281%29.png)

