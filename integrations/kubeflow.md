# Kubeflow

##  **Kubeflow 통합**

특정 기능 사용은 추가 의존성\(dependencies\)를 필요로 합니다. `pip install wandb[kubeflow]`을 실행하여 모든 Kubeflow dependencies\(의존성\)을 설치하십시오.

###  **훈련 작업**

현재 W&B는 분포된 실행을 그룹화하기 위해 **TF\_CONFIG**를 자동으로 읽습니다.

### Arena

wandb 라이브러리는 자동으로 credentials를 컨테이너 환경에 추가하여 [arena](https://github.com/kubeflow/arena)에 통합됩니다. wandb wrapper롤 로컬에서 사용하고 싶으시다면, 다음을 `.bashrc`에 추가합니다.

```text
alias arena="python -m wandb.kubeflow.arena"
```

 로컬에 설치된 arena가 없는 경우, 위의 명령은 `wandb/arena` 도커 이미지를 사용하여 kubectl configs\(구성\)에 마운트를 시도합니다.

###  **파이프라인**

wandb는 [pipelines\(파이프라인\)](https://github.com/kubeflow/pipelines)에서 사용될 수 있는 `arena_launcher_op`을 제공합니다.

 여러분 만의 사용자 지정 런처 op\(launcher op\)를 설계하고 싶으시다면, 이[코드](https://github.com/wandb/client/blob/master/wandb/kubeflow/__init__.py)를 사용해서 pipeline\_metadata을 추가합니다. wandb를 인증하려면**WANDB\_API\_KEY**을 작업\(operation\)에 추가 하셔야 합니다. 그러면 런처는 같은 환경 변수를 훈련 컨테이너에 추가할 수 있습니다.

```python
import os
from kubernetes import client as k8s_client

op = dsl.ContainerOp( ... )
op.add_env_variable(k8s_client.V1EnvVar(
        name='WANDB_API_KEY',
        value=os.environ["WANDB_API_KEY"]))
```

