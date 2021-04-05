# Tensorflow

TesorBoard를 이미 사용 중이신 경우, wandb와 쉽게 통합하실 수 있습니다.

```python
import tensorflow as tf
import wandb
wandb.init(config=tf.flags.FLAGS, sync_tensorboard=True)
```

전체 스크립트 예시를 보시려면 [예시 프로젝트](https://docs.wandb.com/examples)를 참조해 주시기 바랍니다.

## **사용자 정의 메트릭**

`wandb.log({"custom": 0.8}, step=global_step)` ****TensorBoard에 로그되지 않은 추가 사용자 정의 메트릭을 로그해야 하시는 경우, TensorBoard가 사용 하고 있는 같은 단계의 전달인자\(the same step argument\)로 코드에서 `wandb.log`를 호출하실 수 있습니다. 

즉, `wandb.log({"custom": 0.8}, step=global_step)`입니다.

##  **TensorFlow 훅\(Hook\)**

로그된 항목을 좀 더 제어하고 싶으신 경우, wandb는 또한 TensorFlow estimator\(추정량\)에 훅을 제공합니다. 이는 모든`tf.summary` 값을 그래프에 로그합니다.

```python
import tensorflow as tf
import wandb

wandb.init(config=tf.FLAGS)

estimator.train(hooks=[wandb.tensorflow.WandbHook(steps_per_log=1000)])
```

## **수동 로깅\(Manual Logging\)**

TensorFlow에 메트릭을 로그하는 가장 손쉬운 방법은 TensorFlow logger\(로거, 기록장치\)로 `tf.summary`를 로그하는 것입니다.

```python
import wandb

with tf.Session() as sess:
    # ...
    wandb.tensorflow.log(tf.summary.merge_all())
```

 TensorFlow 2를 통해서, 사용자 정의 루프를 통해 모델을 훈련 시키는 방법 중 `tf.GradientTape`를 추천해 드립니다. 자세한 내용은 [이 곳에서](https://www.tensorflow.org/tutorials/customization/custom_training_walkthrough) 확인하실 수 있습니다. 메트릭을 사용자 지정 TensorFlow 훈련 루프에 로그하기 위해 `wandb`를 통합하고 싶으신경우, 다음의 스니펫\(snippet\)을 활용하실 수 있습니다.

```python
    with tf.GradientTape() as tape:
        # Get the probabilities
        predictions = model(features)
        # Calculate the loss
        loss = loss_func(labels, predictions)

    # Log your metrics
    wandb.log("loss": loss.numpy())
    # Get the gradients
    gradients = tape.gradient(loss, model.trainable_variables)
    # Update the weights
    optimizer.apply_gradients(zip(gradients, model.trainable_variables))
```

전체 예시는 [여기](https://www.wandb.com/articles/wandb-customizing-training-loops-in-tensorflow-2)에서 확인하실 수 있습니다.

## **W&B는 TensorBoard와 어떻게 다른가요?**

저희는 여러분 모두를 위해 실험 추적 툴을 개선하기 위해 열심히 최선을 다하고 있습니다. 공동창립사들이 W&B 개발을 시작했을 때, OpenAI에서 좌절한 TensorBoard 사용자들을 위한 툴 개발을 위한 영감을 얻었습니다. 다음은 저희가 개선하기 위해 노력하고 있는 몇 가지 항목들입니다.

1.  **모델 재현**: Weights & Biases는 실험, 탐색 및 추후 모델 재현에 유용합니다. 저희는 메트릭뿐만 아니라 초매개변수 및 코드의 버전을 캡처하고, 프로젝트를 재현할 수 있도록 모델 체크포인트를 저장하실 수 있습니다.
2. **자동 구성**: 프로젝트를 공동작업자에게 넘기거나, 휴가를 낸 경우, W&B는 여러분이 시도한 모든 모델을 쉽게 확인할 수 있도록해드립니다. 따라서 오래된 실험 재실행에 시간을 허비하지 않으셔도 됩니다.
3.   **빠르고 유연한 통합**: 5분 안에 W&B를 여러분의 프로젝트에 추가하세요. 저희의 무료 오픈소스 Python 패키지를 설치하시고, 여러분의 코드에 몇 줄만 추가하세요. 그리고 나면 모델을 실행하실 때마다, 훌륭한 로그된 메트릭과 레코드를 가지게 됩니다.
4.  **지속적인, 중앙 집중식 대시보드**: 로컬 머신, 랩 클러스터\(lab cluster\) 또는 클라우드의 스팟 인스턴스\(spot instances\) 등 모델을 훈련하는 모든 곳에서 동일한 중앙 집중식 대시보드를 제공합니다. 여러 머신에서 TensorBoard 파일을 복사 및 정리하는 데 시간을 쓰실 필요가 없습니다.
5.   **강력한 테이블**: 여러 모델에서의 결과를 검색, 필터, 정렬 및 그룹화합니다. 수 천개의 모델 버전을 살펴보고, 다양한 작업에 대한 최고 퍼포먼스 모델을 쉽게 찾으실 수 있습니다. TensorBoard는 대규모 프로젝트에서 잘 작동하도록 설계되어 있지는 않습니다.
6.  **공동작업을 위한 툴**: W&B를 사용해서 복잡한 머신 러닝 프로젝트를 구성하세요. W&B로 링크를 쉽게 공유하실 수 있으며, 개인 팀\(private teams\)을 활용해 모두가 공유 프로젝트에 결과를 보내도록 할 수 있습니다. 또한 리포트\(reports\)를 통해 공동작업을 지원합니다. 양방향 시각화\(interactive visualizations\)를 추가하고, 마크다운\(markdown\)에서 여러분의 작업 내용을설명하세요. 작업 로그 보관, 상사와의 결과 공유 또는 여러분의 연구실에 결과를 제시할 수 있는 유용한 방법입니다.

 [무료 개인 계정](http://app.wandb.ai/)으로 시작하기 →

## **예시**

저희는 여러분을 위한 통합\(integration\)의 작동 방식을 확인할 수 있는 위한 몇 가지 예시를 만들어보았습니다.

* ​[Github의 예시](https://github.com/wandb/examples/blob/master/examples/tensorflow/tf-estimator-mnist/mnist.py): TensorFlow Estimators\(추정량\)를 사용한 MNIST 예시
*  ​[Github의 예시](https://github.com/wandb/examples/blob/master/examples/tensorflow/tf-cnn-fashion/train.py): Raw TensorFlow를 사용한Fashion MNIST 예시
*  ​[Wandb 대시보드\(Dashboard\)](https://app.wandb.ai/l2k2/examples-tf-estimator-mnist/runs/p0ifowcb): W&B 결과 보기
*  TensorFlow 2에서 훈련 루프 사용자 정의하기 – [기사\(Article\)](https://www.wandb.com/articles/wandb-customizing-training-loops-in-tensorflow-2) \| [Colab Notebook](https://colab.research.google.com/drive/1JCpAbjkCFhYMT7LCQ399y35TS3jlMpvM) \| [대시보드\(Dashboard\)](https://app.wandb.ai/sayakpaul/custom_training_loops_tf)​

