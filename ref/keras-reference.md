---
description: wandb.keras
---

# Keras Reference

 [소스](https://github.com/wandb/client/blob/master/wandb/keras/__init__.py#L148)**​**​

```text
WandbCallback(self,              monitor='val_loss',              verbose=0,              mode='auto',              save_weights_only=False,              log_weights=False,              log_gradients=False,              save_model=True,              training_data=None,              validation_data=None,              labels=[],              data_type=None,              predictions=36,              generator=None,              input_type=None,              output_type=None,              log_evaluation=False,              validation_steps=None,              class_colors=None,              log_batch_frequency=None,              log_best_prefix='best_')
```

WandbCallback은 자동으로 kreas를 wandb와 통합시킵니다.

**예시:**

```text
model.fit(X_train, y_train,  validation_data=(X_test, y_test),callbacks=[WandbCallback()])
```

WandbCallback은 자동으로 keras: loss and anything passed into keras\_model.compile\(\)가 수집한 모든 데이터에서 히스토리 데이터를 로그합니다.

 WandbCallback은 “best”가 `monitor` 및`mode` 속성에 의해 정의되는 “best” 훈련 단계와 연관된 실행에 대한 요약 메트릭을 설정합니다.

WandbCallback은 경사\(gradient\) 및 매개변수 히스토그램을 선택적으로 로그합니다.

WandbCallback은 wandb가 시각화할 훈련 및 검증 데이터를 선택적으로 저장합니다.

**전달인자**:

* `monitor` _str_ -  **모니터링할 메트릭의 이름. 기본값은 val\_loss입니다.**
* `mode` _str_ -  ****{"auto"\(자동\), "min"\(최소\), "max"\(최대\)} 중 하나입니다. “min” – monitor가 최소화될 때 모델을 저장합니다. "max" – monitor가 최대화될 때 모델을 저장합니다. "auto" – 모델이 언제 저장될지 추측합니다 \(기본값\). save\_model: True – monitor가 모든 이전의 에포크\(epochs\)를 능가할 때 모델을 저장합니다. False – 모델을 저장하지 않습니다.
* `save_weights_only` _boolean_ - True인 경우, 모델의 가중치\(weights\)만 저장되며`(model.save_weights(filepath))`, 그렇지 않으면 전체 모델이 저장됩니다 `(model.save(filepath))`.
* `log_weights` - \(boolean\(불린\)\) Ture인 경우 모델 레이어의 가중치\(weights\) 히스토그램을 저장합니다.
* `log_gradients` - \(boolean\(불린\)\) True인 경우 훈련 경사\(training gradients\)의 히스토그램을 로그합니다. 모델은 `total_loss`를 정의해야만 합니다.
* `training_data` -   ****\(tuple\(튜플\)\) model.fit에 전달된 포맷 \(X.y\)와 동일합니다. 이 값은 경사\(gradients\)를 계산하는 데 필요합니다. 이는 `log_gradients`가 `True`일 경우 필수입니다.
* `validation_data` - \(tuple\(튜플\)\) model.fit에 전달된 포맷\(X.y\)와 동일합니다. wandb가 시각화할 데이터 집합입니다. 이것이 설정된 경우, 모든 에포크\(epoch\), wandb는 적은 수의 예측을 수행하고 추후의 시각화를 위한 결과를 저장합니다.
* `generator` _generator_ - wandb가 시각화할 검증 데이터를 반환하는 생성기\(generator\)입니다. 이 생성기는 튜플 \(X.y\)를 반환해야만 합니다. validate\_data 또는 생성기는 wandb가 특정 데이터 예시를 시각화하도록 설정되어야 합니다.
* `validation_steps` _int_ -  `validation_data` 가 생성기인 경우, 전체 검증 세트에 대해 생성기를 실행하는 단계의 수입니다.
* `labels` _list_ - wandb를 사용하여 데이터를 시각화 하는 경우, 다중 분류기\(multiclass classifier\)를 만들고 있는 경우 라벨의 리스트는 숫자 출력을 이해 할 수 있는 스트링으로 변환합니다. 이항 분류기\(binary classifier\)를 만들고 있는 경우 두 레이블\["label for false", "label for true"\]의 리스트에 전달 할 수 있습니다. validate\_data 및 생성기 둘 다 false인 경우, 아무것도 하지 않습니다.
* `predictions` _int_ - 각 에포트\(epoch\) 시각화를 위해 할 예측의 수이며, 최대는 100입니다.
* `input_type` _string_ - 시각화를 지원하는 모델 입력의 유형이며, 다음 중 하나입니다: \("image", "images", "segmentation\_mask"\).
* `output_type` _string_ - 시각화를 지원하는 모델 출력의 유형이며, 다음 중 하나입니다: \("image", "images", "segmentation\_mask"\).
* `log_evaluation` _boolean_ - True인 경우, 훈련 종료 시 전체 검증 결과를 포함한 데이터프레임을 저장합니다.
* `class_colors` _\[float, float, float\]_ - 입력 또는 출력이 분할 마스크\(segmentation mask\)인 경우, 각 클래스에 대한 rgb 튜플\(tuple\) \(범위 0-1\)을 포함한 배열입니다.
* `log_batch_frequency` _integer_ - None일 경우, callback은 모든 에포크\(epoch\)를 로그합니다. 정수\(integer\)로 설정된 경우, callback은 모든 log\_batch\_frequency을 일괄적으로 함께 묶는\(batch\) 훈련 메트릭을 로그합니다.
* `log_best_prefix` _string_ - None인 경우, 추가 요약 메트릭이 저장되지 않습니다. 스트링으로 설정되면, 모니터링된 메트릭 및 에포크는 이 값 앞에 추가되고 요약 메트릭으로 저장됩니다.

