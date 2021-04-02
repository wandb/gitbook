# PyTorch Lightning

 PyTorch Lightning은 PyTorch 코드 구성과 [분배된 훈련 \(distributed training\)](https://pytorch-lightning.readthedocs.io/en/latest/multi_gpu.html) 및 [16 비트 정밀도\(16-bit precision\)](https://pytorch-lightning.readthedocs.io/en/latest/amp.html)와 같은 고급 기능을 쉽게 추가하기 위한 lightweight wrapper를 제공합니다. W&B는 ML 실험 로깅을 위한 lightweight wrapper를 제공합니다. 저희는 PyTorch Lightning 라이브러리로 직접 통합되어, 언제든지 [해당 문서](https://pytorch-lightning.readthedocs.io/en/latest/loggers.html#weights-and-biases)를 확인하실 수 있습니다.

## ⚡ **단 두 줄로 빠르게 진행하세요:**

```python
from pytorch_lightning.loggers import WandbLogger
from pytorch_lightning import Trainer

wandb_logger = WandbLogger()
trainer = Trainer(logger=wandb_logger)
```

## ✅ **실제 예시를 확인하세요!**

통합\(integration\)이 어떻게 작동하는지 확인하시도록 몇 가지 예를 만들어보았습니다:

*  ​[Google Colab의 데모](https://colab.research.google.com/drive/1GHWwfzAsWx_Q1paw73hngAvA7-U9QHi-): 통합을 시도 해보기 위한 간단한 데모
* [사용 지침서](https://colab.research.google.com/github/wandb/examples/blob/master/colabs/pytorch-lightning/Supercharge_your_Training_with_Pytorch_Lightning_%2B_Weights_%26_Biases.ipynb): Supercharge your Training with Pytorch Lightning + Weights & Biases을 통한 당신의 훈련에 대한 과급\(supercharge\)
*  ​[Lightning을 통한 의미 분할\(Semantic Segmentation with Lightning](https://app.wandb.ai/borisd13/lightning-kitti/reports/Lightning-Kitti--Vmlldzo3MTcyMw): 자율주행차에 대한 신경망 최적화[\)](https://app.wandb.ai/borisd13/lightning-kitti/reports/Lightning-Kitti--Vmlldzo3MTcyMw)
*  Lightning 모델 퍼포먼스 추적에 대한 [단계별 가이드](https://app.wandb.ai/cayush/pytorchlightning/reports/Use-Pytorch-Lightning-with-Weights-%26-Biases--Vmlldzo2NjQ1Mw)​

## **💻 API 참조**

### `WandbLogger`

선택적 초매개변수:

* **name** \(_str_\) – 실행에 대한 표시 이름.
* **save\_dir** \(_str_\) – 데이터가 저장되는 경로.
* **offline** \(_bool_\) – 오프라인에서 실행 \(데이터는 나중에 wandb 서버로 스트리밍 될 수 있습니다\)
* **version** \(_str_\) – 주로 이전 실행을 재개하는데 사용되는 버전을 설정.
* **anonymous** \(_bool_\) – 익명 로깅을 활성화 또는 명시적으로 비활성화
* **project** \(_str_\) – 이 실행이 속하는 프로젝트의 이름.
* **log\_model** \(_bool_\) – 이 실행과 관련된 태그.
* **tags** \(_list of str_\) – 이 실행과 관련된 태그.

### **`WandbLogger.watch`**

모델 토폴로지\(topology\) 및 선택적으로 경사\(gradients\) 및 가중치\(weights\)를 로그합니다.

```python
wandb_logger.watch(model, log='gradients', log_freq=100)
```

초매개변수:

* **model** \(_nn.Module_\) – 로그 될 모델
* **log** \(_str_\) – “gradients”\(기본값\), “parameters”, “all” 또는 None 일 수 있습니다
* **log\_freq** \(_int_\) – 경사\(gradients\) 와 매개변수\(parameters\)의 로깅 사이의 단계 개수

### **`WandbLogger.log_hyperparams`**

记录超参数配置。

注意：当使用`LightningModule.save_hyperparameters()时此函数会被自动调用`

```python
wandb_logger.log_hyperparams(params)
```

Parameters:

* **params（**字典型）——字典型数据，超参数名称作为“键”，配置值作为“值”。

### `WandbLogger.log_metrics`

 记录训练指标。

注意：此函数由`LightningModule.log('metric', value)`自动调用。

```python
wandb_logger.log_metrics(metrics, step=None)
```

**参数:**

* **metric** \(数字型）——字典型数据，指标名称作为“键”，测试量作为“值”。
* **step** \(int\|None\) – 메트릭이 기록되어야 하는 단계 개수

\*\*\*\*

