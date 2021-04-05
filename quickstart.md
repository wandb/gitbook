---
description: 간편하게 스크립트를 작성하여 여러분의 프로젝트의 실험 추적 및 시각화 기능을 확인하실 수 있습니다
---

# Quickstart

**세 가지 빠른 단계로 머신 러닝 실험 로깅을 시작하세요.**

## **1. 라이브러리 설치**

**Python 3환경에 라이브러리를 설치하세요.**

```bash
pip install wandb
```

{% hint style="info" %}
Google의 CloudML과 같은 쉘 명령을 실행하기 어려운 자동화된 환경에서 모델을 학습시키는 경우, [자동환경에서 실행하기](https://docs.wandb.com/advanced/automated) 문서를 참조하시기 바랍니다.
{% endhint %}

## 2. 계정 생성하기

 여러분의 쉘에 무료 계정을 등록하시거나 [가입 페이지](https://app.wandb.ai/login?signup=true)로 이동하세요

```bash
wandb login
```

## 3.  **학습 스크립트 수정하기**

 초매개변수 및 메트릭 로그를 진행하시려면 스크립트에 추가 입력하세요.

{% hint style="info" %}
Weights and Biases는 프레임워크에 구애되지 않습니다. 하지만 일반적인 ML 프레임워크를 사용하고 있으시다면, 프레임워크에 특정한 예시를 시작하는 것이 훨씬쉬울 수도 있습니다. 저희는 [Keras](https://docs.wandb.com/frameworks/keras), [TensorFlow](https://docs.wandb.com/frameworks/tensorflow), [PyTorch](https://docs.wandb.com/frameworks/pytorch), [Fast.ai](https://docs.wandb.com/frameworks/fastai), [Scikit-learn](https://docs.wandb.com/frameworks/scikit), [XGBoost](https://docs.wandb.com/frameworks/xgboost), [Catalyst](https://docs.wandb.com/frameworks/catalyst), 및[Jax](https://docs.wandb.com/frameworks/jax-example)에 대한 통합을 단순화하기 위해서 프레임워크 특정 후크를 개발했습니다.
{% endhint %}

###  **Wandb 초기설정하기**

  불러오기**를 진행한 후**, 스크립트 시작 부분에서 `wandb`를 초기화하십시오

```python
# Inside my model training code
import wandb
wandb.init(project="my-project")
```

  만약 존재하지 않는 경우,여러분을 위해 자동으로 프로젝트를 생성합니다. 위의 훈련 스크립트의 실행은 “my-project”라는 프로젝트와 동기화 됩니다. 자세한 초기화 옵션은 [wandb.init](https://docs.wandb.com/library/init) 문서를 참조하십시오.

###  **초매개 변수 선언**

 ​[wandb.config](https://docs.wandb.com/library/config) 객체로 **초매개 변수**를 쉽게 저장하실 수 있습니다.

```python
wandb.config.dropout = 0.2
wandb.config.hidden_layer_size = 128
```

###  로그 메트릭

 모델을 훈련시키는 것처럼 손실 또는 정확성과 같은 메트릭을 기록하세요 \(대부분의 경우 프레임워크에 특정한 기본값을 제공합니다\). [wandb.log](https://docs.wandb.com/library/log)를 사용해서 히스토그램, 그래프, 이미지와 같은 좀 더 복잡한 출력 및 결과를 로그하세요.

```python
def my_train_loop():
    for epoch in range(10):
        loss = 0 # change as appropriate :)
        wandb.log({'epoch': epoch, 'loss': loss})
```

###  **파일 저장**

`wandb.run.dir` 디렉토리에 저장된 모든 항목은 W&B에 업로드 되며, 이 작업이 완료되면 결과가 저장됩니다. 이는 모델 내 문자상의 가중치 및 편향 저장에 유용합니다.

```python
# by default, this will save to a new subfolder for files associated
# with your run, created in wandb.run.dir (which is ./wandb by default)
wandb.save("mymodel.h5")

# you can pass the full path to the Keras model API
model.save(os.path.join(wandb.run.dir, "mymodel.h5"))
```

 좋습니다! 이제 스크립트를 정상적으로 실행하시면 백그라운드 프로세스로 로그가 동기화됩니다. git repo에서 실행중인 경우, 터미널 출력, 메트릭 및 파일은 여러분의 git 상태 기록과 함께 클라우드에 동기화 됩니다.

{% hint style="info" %}
테스트 중에 wandb 동기화 사용을 원하지 않으시다면, [환경 변수](https://docs.wandb.com/library/environment-variables) WANDB\_MODE=dryrun를 설정하세요.
{% endhint %}

## **다음 단계**

**계측\(implementation\)이 진행됩니다. 이 훌륭한 기능의 간략한 개요입니다.**

1. **프로젝트 페이지:** 프로젝트 대시보드에서 다양한 실험 결과를 비교합니다. 프로젝트에서 모델을 실행하실 때마다 그래프와 테이블에 새로운 라인이 나타납니다. 왼쪽 사이드바의 테이블을 클릭하시면 테이블을 확장하여 모든 초매개 변수와 메트릭을 확인하실 수 있습니다. 여러 프로젝트를 생성하여 실행을 구성하고, 테이블을 사용하여 태그와 노트를 실행에 추가하세요.
2. **시각화 사용자 지정하기**: 평행좌표 차트, 산점도 및 기타 고급 시각화를 추가하여 결과를 탐색하세요.
3.  [ **리포트**](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MO02cq56Hqy11WYFJ6I/v/han-guo-yu/reports): 마크다운 패널을 추가하여 실시간 그래프 및 테이블과 함께 연구 결과를 분석합니다. 리포트 기능으로 공동작업자, 교수, 상사와 프로젝트의 스냅샷을 쉽게 공유하실 수 있습니다.
4.  [**프레임워크**](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MO02cq56Hqy11WYFJ6I/v/han-guo-yu/integrations): PyTorch, Keras, XGBoost와 같은 대중적 프레임워크에 대한 특별 통합 기능을 갖추고 있습니다.
5. **쇼케이스**: 연구결과를 공유하고 싶으신가요? 저희는 항상커뮤니티의 놀라운 작업을 소개하기 위해 블로그에 게시하고 있습니다. [contact@wandb.com](mailto:contact@wandb.com)으로 연락해주시기 바랍니다. 

  

###   [**질문이 있으시면 연락해 주십시오→**](https://docs.wandb.com/company/getting-help)**​**​

###  [​](https://bit.ly/wandb-learning-dexterity) [**오픈AI 케이스 연구결과 보기 →​**](https://bit.ly/wandb-learning-dexterity)**​**

![](.gitbook/assets/image%20%2891%29.png)

