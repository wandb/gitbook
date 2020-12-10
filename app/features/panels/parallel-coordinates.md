---
description: 머신 러닝 실험에서 고차원 데이터를 시각화 하실 수 있습니다
---

# Parallel Coordinates

 다음은 평행좌표플롯\(parallel coordinates plot\)의 예시입니다. 각 축은 다른 것을 나타냅니다. 아래의 경우, 4개의 수직 축을 선택했습니다. 이 경우에, 저는 다른 초매개변수간의 관계 및 모델의 최종 정확성을 시각화하고 있습니다

* **Axes\(축\)**: [wandb.config](https://docs.wandb.com/library/config)의 다른 초매개변수 및 [wandb.log\(\)](https://docs.wandb.com/library/log)​의 메트릭
* **Lines\(선\)**: 각 선은 단일 실행을 나타냅니다. 마우스를 선 위에 놓으면 실행에 관한 세부정보를 포함한 툴팁\(tooltip\)을 확인하실 수 있습니다. 현재 필터와 일치하는 모든 선이 표시되지만, eye\(눈 모양\)을 끄면, 선은 회색으로 표시됩니다.

 **패널 설정\(Panel Settings\)**

패널 설정에서 다음의 기능을 구성하세요. 패널 우측 상단의 edit\(편집\) 버튼을 클릭합니다.

* **Tooltip\(툴팁\)**: 마우스를 위에 두시면, 각 실행에 관한 정보와 함께 범례가 표시됩니다
* **Titles\(제목\)**: 보다 읽기 쉽게 축 제목을 편집합니다
* **Gradient\(경사\)**: 원하는 색상 범위로 경사를 사용자 정의합니다.
* **Log scale\(로그 스케일\)**: 각 축을 로그 스케일에서 독립적으로 보도록 설정할 수 있습니다.
* **Flip axis\(플립 축\)**: 축 방향을 전환합니다. 정확도와 손실 모두 축으로 사용할 때 유용합니다.

   [라이브로 보기 →](https://app.wandb.ai/example-team/sweep-demo/reports/Zoom-in-on-Parallel-Coordinates-Charts--Vmlldzo5MTQ4Nw)​

![](../../../.gitbook/assets/2020-04-27-16.11.43.gif)

