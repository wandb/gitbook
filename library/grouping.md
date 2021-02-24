---
description: 훈련 및 평가 실행을 더 큰 실험으로 그룹화합니다
---

# Grouping

고유한 그룹 이름을 **wandb.init\(\)**으로 전달하여 개별 실행을 실험으로 그룹화합니다.

###  **활용 사례**

1. **분산 훈련\(Distributed training\): 실험이 더 큰 전체의 일부분으로 간주되어야 할 별도의 훈련 및 평가 스크립트와 함께 실험을 여러 조각으로 분할 된 경우 그룹화를 사용하시기 바랍니다.**
2. **다중 프로세스\(Multiple processes\): 여러 개의 작은 프로세스를 함께 한 실험으로 그룹화합니다.**
3.  K-겹 교차 검증\(K-fold cross-validation\): 여러 다른 임의의 시드를 사용하여 실험을 함께 그룹화하면 더 큰 실험을 확인하실 수 있습니다. 여기 스윕 및 그룹화를 활용한 k-겹 교차 검증 [예시](https://github.com/wandb/examples/tree/master/examples/wandb-sweeps/sweeps-cross-validation)가 있습니다.

###  **어떤 모습인가요?** 

스크립트에 그룹화를 설정하신 경우, UI의 테이블에 기본값으로 실행을 그룹화합니다. 사용자는 테이블 상단의 **Group \(그룹화\)** 버튼을 클릭하여 이 기능을 설정 및 해제할 수 있습니다. 여기 프로젝트 페이지에서 그룹화의 예시를 보여드리겠습니다.

* **사이드바\(sidebar\)**: 에포크\(epoch\)의 숫자에 따라 실행이 그룹화됩니다.
* **그래프**: 각 선은 그룹의 평균을 나타내며, 음영은 분산을 나타냅니다. 이 동작은 그래프 설정에서 변경하실 수 있습니다.

![](../.gitbook/assets/demo-grouping.png)

그룹화를 사용하는 방법에는 다음의 몇 가지가 있습니다:

**스크립트에 그룹 설정하기**

선택적 그룹\(optional group\) 및 job\_type을 `wandb.init()`에 전달합니다. 예: `wandb.init(group="experiment_1", job_type="eval")`. 그룹은 프로젝트 내에서 고유해야 하며 그룹 내의 모든 실행에서 공유되어야 합니다. `wandb.util.generate_id()`를 사용하여 모든 프로세스에서 사용할 고유한 8자 문자 스트링을 생성하실 수 있습니다. 예: `os.environ["WANDB_RUN_GROUP"] = "experiment-" + wandb.util.generate_id()`   
  


 **그룹 환경 변수 설정하기**

`WANDB_RUN_GROUP`을 사용하여 환경변수로써 실행에 대한 그룹을 지정합니다. 더 자세한 내용은 [**환경변수\(Environment Variables\)**](https://docs.wandb.com/library/environment-variables)에 관한 문서를 확인하시기 바랍니다.

###  **UI에서 그룹화 토글**

모든 구성 열을 기준으로 동적 그룹화를 하실 수 있습니다. 예를 들어, wandb.config 를 사용하여 배치 사이즈\(batch size\) 또는 학습률\(learning rate\)를 로그하는 경우, 웹 앱에서 이러한 초매개변수로 동적 그룹화를 하실 수 있습니다.

![](../.gitbook/assets/demo-no-grouping.png)

###  **그래프 설정 그룹화**

그래프의 우측 상단에 있는 편집\(Edit\) 버튼을 클릭하고 고급\(Advanced\) 탭을 클릭하여 선 및 음영을 변경합니다. 각 그룹의 선에 대한 평균\(mean\), 최소\(minimum\), 최대\(maximum\)값을 선택하실 수 있습니다. 음영의 경우, 음영을 끄고, 최소 및 최대값, 표준 편차\(standard deviation\), 표준 오차\(standard error\)를 표시할 수 있습니다.

![](../.gitbook/assets/demo-grouping-options-for-line-plots.gif)



