# Scikit

단 몇 줄의 코드만으로 wandb를 사용하셔서 scikit-learn 모델의 퍼포먼스를 시각화하고 비교하실 수 있습니다. [**예시 보기 →**](https://colab.research.google.com/drive/1j_4UQTT0Lib8ueAU5zXECxesCj_ofjw7)​

###  **플롯 만들기**

#### **1단계: wandb 불러오기 및 새로운 실행 초기화하기**

```python
import wandb
wandb.init(project="visualize-sklearn")

# load and preprocess dataset
# train a model
```

####  **2단계: 개별 플롯 시각화하기**

```python
# Visualize single plot
wandb.sklearn.plot_confusion_matrix(y_true, y_pred, labels)
```

**또는, 모든 플롯 한 번에 시각화하기:**

```python
# Visualize all classifier plots
wandb.sklearn.plot_classifier(clf, X_train, X_test, y_train, y_test, y_pred, y_probas, labels,
                                                         model_name='SVC', feature_names=None)

# All regression plots
wandb.sklearn.plot_regressor(reg, X_train, X_test, y_train, y_test,  model_name='Ridge')

# All clustering plots
wandb.sklearn.plot_clusterer(kmeans, X_train, cluster_labels, labels=None, model_name='KMeans')
```

###  **지원 플롯**

#### **학습 곡선**

![](../.gitbook/assets/screen-shot-2020-02-26-at-2.46.34-am.png)

 다양한 길이의 데이터 세트에서 모델을 훈련하고 훈련 및 테스트 세트 모두에 대해서 교차 검증된 값 대 데이터 세트 사이즈의 플롯을 생성합니다.

`wandb.sklearn.plot_learning_curve(model, X, y)`

* model \(clf or reg\): 적합 회기기\(regressor\) 또는 분류기\(classifier\)에 포함
* X \(arr\): 데이터 세트 특징
* y \(arr\): 데이터 세트 라벨

#### ROC

![](../.gitbook/assets/screen-shot-2020-02-26-at-2.48.02-am.png)

ROC 곡선은 참 양성 비율\(true positive rate\) \(y-축\) 대 거짓 양성 비율\(x-축\)을 나타냅니다. 이상적인 값은 TPR = 1및 FPR =0으로, 왼쪽 상단의 포인트입니다. 일반적으로 R\)C 곡선 \(AUC-ROC\) 아래의 영역을 계산하며, AUC-ROC가 클수록 좋습니다.

`wandb.sklearn.plot_roc(y_true, y_probas, labels)`

* y\_true \(arr\): 테스트 세트 라벨
* y\_probas \(arr\): 테스트 세트 예측 확률.
* labels \(list\): 목표변수에 대해 명시된 라벨 \(y\).

####  **클래스 비율도**

![](../.gitbook/assets/screen-shot-2020-02-26-at-2.48.46-am.png)

훈련 및 테스트 세트의 목표 클래스의 분포를 작성합니다. 불균형 클래스 탐지 및 한 클래스가 모델에 불균형적인 영향을 미치지 않도록 하는 데 유용합니다.

`wandb.sklearn.plot_class_proportions(y_train, y_test, ['dog', 'cat', 'owl'])`

* y\_train \(arr\): 훈련 세트 라벨
* y\_test \(arr\): 테스트 세트 라벨
* labels \(list\): 목표 변수에 대해 명시된 라벨 \(y\).

####  **정밀 회수 곡선 \(Precision Recall Curve\)**

![](../.gitbook/assets/screen-shot-2020-02-26-at-2.48.17-am.png)

 서로 다른 임계값\(thresholds\)에 대해 정밀도와 호출 사이의 트레이드오프\(tradeoff\)를 계산합니다. 곡선 아래의 높은 영역은 높은 회수와 높은 정밀도 모두를 나타내며, 여기서 높은 정밀도는 낮은 거짓 양성 비율 \(false positive rate\)와 관련 있으며, 높은 회수는 낮은 거짓 음성 비율과 관련이 있습니다.

두 값이 다 높은 경우 분류기가 정확한 결과\(높은 정밀\)를 반환하고, 모든 양성 값 \(높은 회수\)의 대부분을 반환하고 있음을 나타냅니다. PR 곡선은 클래스가 매우 불균형적일 때 유용합니다.

`wandb.sklearn.plot_precision_recall(y_true, y_probas, labels)`

* y\_true \(arr\): 테스트 세트 라벨.
* y\_probas \(arr\): 테스트 세트 예측 확률.
* labels \(list\): 목표 변수에 대해 명시된 라벨 \(y\).

####  **특징 중요도\(Feature Importances\)**

![](../.gitbook/assets/screen-shot-2020-02-26-at-2.48.31-am.png)

 분류 작업에 대해 각 특징의 중요도를 평가 및 작성합니다. 오직 `featureimportances` 속성\(attribute\)을 포함한 분류기에서만 작동합니다.

`wandb.sklearn.plot_feature_importances(model, ['width', 'height, 'length'])`

* model \(clf\): 적합 분류기\(classifier\)의 포함
* feature\_names \(list\): 특징에 대한 이름. 특징 인덱스를 상응하는 이름으로 대체하여 플롯을 보다 읽기 쉽게 만듭니다.

#### **교정 곡선\(calibration curve\)**

![](../.gitbook/assets/screen-shot-2020-02-26-at-2.49.00-am.png)

분류기의 예측 확률을 얼마나 잘 교정했는지와 교정되지 않은 분류기를 교정하는 방법을 표시합니다. 추정된 예상 가능성을 기준 로지스틱 회귀 모델\(baseline logistic regression model\), 전달인자로써 전달된 모델 및 동위 교정\(isotonic calibration\) 및 시그모이드 교정\(sigmoid calibration\) 둘 다에 의해 비교됩니다.

 교정 곡선이 대각선에 가까울수록, 더 좋습니다. 전치 시그모이드 유사 곡선\(transposed sigmoid like curves\)은 과대 적합한 분류기\(overfitted classifier\)를 나타내며, 시그모이드 유사 곡선\(sigmoid like curve\)은 과소 적합 분류기\(underfitted classifier\)를 나타냅니다. 모델의 동위 및 시그노이드 교정을 훈련하고 해당 곡선을 비교함으로써 모델이 과대 또는 과소 적합 상태인지 확인하고, 만약 그렇다면, 어떤 교정 \(시그모이드\(sigmoid\) 또는 동위\(isotonic\)\)이 이를 해결하는 데 좋을지 파악할 수 있습니다.

 더 자세한 정보는 [sklearn's docs](https://scikit-learn.org/stable/auto_examples/calibration/plot_calibration_curve.html)을 확인하시기 바랍니다.

`wandb.sklearn.plot_calibration_curve(clf, X, y, 'RandomForestClassifier')`

* model \(clf\): 적합 분류기의 포함
* X \(arr\): 훈련 세트 특징
* y \(arr\): 훈련 세트 라벨
* model\_name \(str\): 모델 이름. 기본값은 ‘Classifier’입니다.

#### **혼동 행렬\(Confusion Matrix\)**

![](../.gitbook/assets/screen-shot-2020-02-26-at-2.49.11-am.png)

 분류의 정확성을 평가하기 위해 혼동 행렬을 계산합니다. 모델 예측의 질을 평가하고 모델이 잘못하는 예측에서 패턴을 찾는 데 유용합니다. 대각선은 모델이 정확하게 예측한 예측값을 나타냅니다. 즉, 실제 라벨은 예상 라벨과 동일한 경우입니다.

`wandb.sklearn.plot_confusion_matrix(y_true, y_pred, labels)`

* y\_true \(arr\): 테스트 세트 라벨
* y\_pred \(arr\): 테스트 세트 예측 라벨
* labels \(list\): 목표 변수에 대해 명시된 라벨 \(y\).

####  **요약 메트릭\(Summary Metrics\)**

![](../.gitbook/assets/screen-shot-2020-02-26-at-2.49.28-am.png)

요약 메트릭\(f1, 정확성\(accuracy\), 정밀성 및 분류에 대한 회수\) \(precision and recall for classification\) 및 회귀에 대한 mse, mae, r2 값\(mse, mae, and r2 score for regression\) 을 계산합니다.

`wandb.sklearn.plot_summary_metrics(model, X_train, X_test, y_train, y_test)`

* model \(clf or reg\): 적합 회기기\(regressor\) 또는 분류기\(classifier\) 포함
* X \(arr\): 훈련 세트 특징.
* y \(arr\): 훈련 세트 라벨.
  * X\_test \(arr\): 테스트 세트 특징.
* y\_test \(arr\): 테스트 세트 라벨

####  **엘보 플롯\(Elbow Plot\)**

![](../.gitbook/assets/screen-shot-2020-02-26-at-2.52.21-am.png)

훈련 시간과 함께 클러스터\(clusters\)의 수의 함수로 설명되는 분산 비율을 측정하고 나타냅니다. 최적 클러스터의 수를 선택하는 데 유용합니다.

`wandb.sklearn.plot_elbow_curve(model, X_train)`

* model \(clusterer\): 적합 클러스터의 테이크\(takes\)
* X \(arr\): 훈련 세트 특징.

####  **실루엣 플롯\(Silhouette Plot\)**

![](../.gitbook/assets/screen-shot-2020-02-26-at-2.53.12-am.png)

한 클러스터의 한 포인트가 이웃 클러스터의 포인트에 얼마나 가까운지 측정하고 나타냅니다. 클러스터의 두께는 클러스터 사이즈에 상응합니다. 수직선은 모든 포인트의 평균 실루엣값을 나타냅니다.

+1에 가까운 실루엣 계수\(Silhouette coefficients\)는 샘플이 이웃 클러스터로부터 멀리 떨어져 있음을 나타냅니다. 값이 0이면, 샘플이 두 이웃 클러스터 사이의 결정 경계\(decision boundary\)에 있거나 아주 가까움을 나타내며, 음수 값은 이러한 샘플이 잘못된 클러스터에 할당되었을 수도 있음을 나타냅니다.

일반적으로 모든 실루엣 클러스터값이 평균 이상\(붉은 선을 지나서\) 이고 가능한 1에 가까워야 합니다.

`wandb.sklearn.plot_silhouette(model, X_train, ['spam', 'not spam'])`

* model \(clusterer\): 적합한 클러스터의 포함
* X \(arr\): 훈련 세트 특징.
  * cluster\_labels \(list\): 클러스터 라벨에 대한 이름. 클러스터 인덱스를 상응하는 이름으로 대체하여 플롯을 보다 읽기 쉽게 만듭니다.

####  **이상치 후보군 플롯\(Outlier Candidates Plot\)**

![](../.gitbook/assets/screen-shot-2020-02-26-at-2.52.34-am.png)

쿡 거리\(Cook’s distance\)를 지나 회귀 모델에 대한 데이터 포인트의 영향을 측정합니다. 심하게 편향된 영향\(heavily skewed influences\)이 있는 인스턴스\(Instances\) 잠재적으로 이상치\(outliters\)가 될 수 있습니다. 이상치 감지에 유용합니다.

`wandb.sklearn.plot_outlier_candidates(model, X, y)`

* model \(regressor\): 적합한 분류기의 포함
* X \(arr\): 훈련 세트 특징
* y \(arr\): 훈련 세트 라벨

#### **잔류 플롯\(Residuals Plot\)**

![](../.gitbook/assets/screen-shot-2020-02-26-at-2.52.46-am.png)

 예측 목표 값\(predicted target values\) \(y-축\) 대 실제 및 예측 목표 값 \(x-축\)의 차이 및 잔류 오류의 분포를 측정하고 기록합니다.

일반적으로, 좋은 모델은 랜덤 오차\(random error\)를 제외하고 데이터 세트에서 대부분 현상을 차지 하므로 적합 모델\(well-fit model\)의 잔류물은 임의로 분포되어야 합니다.

`wandb.sklearn.plot_residuals(model, X, y)`

* model \(regressor\): 적합한 분류기의 포함
* X \(arr\):  트레이닝 세트 특징
* y \(arr\): 트레이닝 세트 라벨

   더 궁금한 점이 있으시면, 저희 [slack community](http://bit.ly/wandb-forum)에 문의해 주시기 바랍니다.  

##  **예시**

* [colab 에서 실행\(Run in colab\)](https://colab.research.google.com/drive/1tCppyqYFCeWsVVT4XHfck6thbhp3OGwZ): 시작을 위한 간단한 notebook
* [Wandb 대시보드\(Dashboard\)](https://app.wandb.ai/wandb/iris): W&B에서 결과 보기

