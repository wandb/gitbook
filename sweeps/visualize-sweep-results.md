# Visualize Sweep Results

##  **평행 좌표 플롯**

![](https://paper-attachments.dropbox.com/s_194708415DEC35F74A7691FF6810D3B14703D1EFE1672ED29000BA98171242A5_1578695138341_image.png)

평행 좌표 플롯은 초매개변수 값을 모델 메트릭에 작성합니다. 최고 모델 퍼포먼스로 이어지는 초매개변수 조합에 초점을 맞출 때 유용합니다.

##  **초매개변수 중요도 플롯**

![](https://paper-attachments.dropbox.com/s_194708415DEC35F74A7691FF6810D3B14703D1EFE1672ED29000BA98171242A5_1578695757573_image.png)

 초매개변수 중요도 플롯\(hyperparameter importance ploce\)은 어떤 초매개변수가 최고의 예측변수였는지, 그리고 메트릭에 대한 이상적 값과 높은 상관관계에 있는지를 나타냅니다.

**상관관계\(Correlation\)**는 초매개변수와 선택된 매트릭 \(이 경우 val\_loss\)간의 선형 상관관계입니다. 따라서 높은 상관관계는 초매개변수가 높은 값을 가지고 있을 때, 메트릭 또한 높은 값을 가지고 있으며, 그 반대의 경우도 마찬가지임을 의미합니다. 상관관계는 살펴보기에 좋은 메트릭이나 입력\(inputs\)간의 이차 상호 작용\(second order interactions\)를 담아내지는 못하며,입력\(inputs\)를 완전히 다른 범위와 비교하는 것은 복잡해 질 수 있습니다.

초매개변수를 입력\(inputs\)으로, 메트릭을 대상 출력\(target output\)으로 사용해 랜덤 포레스트를 훈련하는 **importance\(중요도\)** 메트릭을 계산하며, 랜덤 포레스트에 대한 특성 중요도 값\(feature importance values\)을 리포트합니다.

