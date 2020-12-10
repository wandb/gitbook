# Jupyter

 여러분의 Jupyter notebooks에 Weights & Biases를 사용하여 양방향 시각화를 얻고 훈련 실행에 대한 사용자 정의 분석을 하실 수 있습니다.

##  **Jupyter notebooks으로 W&B에 대한 케이스 사용**

1. **반복 실험**: 매개변수를 수정하여, 실험을 실행 및 재실행하고, 수행 도중 수동 메모\(manual notes\)를 할 필요 없이 수행하는 모든 실험을 자동으로 W&B에 저장합니다.
2. **코드 저장\(code saving\)**: 모델을 재생산할 때, notebook에서 어떤 셀\(cell\)이 실행 되었는지, 어떤 순서로 실행되었는지 파악하기 어렵습니다. [설정 page](https://app.wandb.ai/settings)에서 코드 저장\(code saving\)을 각 실험에 대한 설정하여 셀 실행 기록\(a record of cell execution\)을 저장합니다
3. **사용자 정의** **분석**: 일단 실행이 W&B에 로그되면, API에서 데이터프레임\(dataframe\)을 가져와 사용자 정의 분석을 하고, 이러한 결과를 W&B에 쉽게 로그 하여 리포트에 저장 및 공유하실 수 있습니다.

## **notebooks 구성**

다음의 코드로 notebook을 시작하여 W&B를 설치하고 계정을 연결하세요:

```python
!pip install wandb -qqq
import wandb
wandb.login()
```

다음, 실험을 설정하고 초매개변수를 저장하세요:

```python
wandb.init(project="jupyter-projo",
           config={
               "batch_size": 128,
               "learning_rate": 0.01,
               "dataset": "CIFAR-100",
           })
```

`wandb.init()`을 실행한 후, 새 셀을 `%%wandb`로 실행하여 라이브 그래프를 notebook에서 확인힙니다. 이 셀을 여러 번 실행한 경우, 데이터는 해당 실행에 추가됩니다.

```python
%%wandb

# Your training loop here
```

이 [예제 스크립트](https://bit.ly/wandb-jupyter-widgets-colab)에서 직접 해보세요 →

![](../.gitbook/assets/jupyter-widget.png)

 `%%wandb` 데코레이터\(decorator\)의 대안으로, `wandb.init()` 뒤에 라인을 추가하여 인라인 그래프\(in-line graph\)를 나타낼 수 있습니다.

```python
# Initialize wandb.run first
wandb.init()

# If cell outputs wandb.run, you'll see live graphs
wandb.run
```

##  **W&B의 추가 Jupyter 기능**

1. **Colab**: Colab에서 `wandb.init()`을 처음으로 호출할 때, 여러분의 브라우저에서 현재 W&B에 로그인 한 경우, 저희는 자동으로 여러분의 런타임을 인증합니다. 실행 페이지의 개요\(overview\) 탭에서 Colab 링크를 확인하실 수 있습니다. [설정](https://app.wandb.ai/settings)에서 코드 저장\(code saving\)을 켜시면, 실험을 실행하기 위해 실행된 셀도 확인하실 수 있으며, 재현성이 향상됩니다.
2. **Docker Jupyter 실행**: `wandb docker –jupyter`을 호출하여 docker 컨테이너를 실행하고, 코드를 마운트하고, Jupyter가 설치되었는지 확인하고, port 8888에서 실행합니다.
3. **run.finish\(\)**: 기본값으로, 실행을 완료로 표시하기 위해 다음 번에 wandb.init\(\)이 호출될 때까지 기다립니다. 이를 통해서 개별 셀을 실행하고 동일 실행에 모두 로그 되도록 할 수 있습니다. Jupyter notebook에서 수동으로 완료 표시를 하시려면, **run.finish\(\)** 기능을 사용하시기 바랍니다.

```python
import wandb
run = wandb.init()
# Training script and logging goes here
run.finish()
```

### **W&B 정보 메시지 음소거하기**

 정보 메시지를 비활성화 하시려면, 다음을 notebook 셀에서 실행합니다:

```python
import logging
logger = logging.getLogger("wandb")
logger.setLevel(logging.ERROR)
```

##  **공통 질문**

###  **Notebook 이름**

“Failed to query for notebook name you can set it manually with the WANDB\_NOTEBOOK\_NAME environment variable \(notebook 이름에 대한 쿼리를 수행하지 못했습니다,. WANDB\_NOTEBOOK\_NAME 환경 변수로 수동으로 설정 하실 수 있습니다.” 라는 오류 메시지가 나타나는 경우, 스크립트의 환경변수를 다음과 같이 설정해서 해결하실 수 있습니다: `os.environ['WANDB_NOTEBOOK_NAME'] = 'some text here'`

