# Databricks

 W&B는 W&B Jupyter notebook experience을 Databriks 환경에서 사용자 정의 하여 [Databricks](https://www.databricks.com/)와 통합됩니다.

##  **Databricks 구성**

###  **클러스터에 wandb 설치**

 클러스터 구성으로 이동하여, 클러스터를 선택하고, 라이브러리를 클릭한 후 새로 설치\(Install New\)에서 PyPI를 선택하고 패키지 `wandb`를 추가합니다.

###  **인증**

W&B 계정을 인증하시기 위해, notebooks에서 쿼리할 수 있는 databricks secret을 추가하실 수 있습니다.

```bash
# install databricks cli
pip install databricks-cli

# Generate a token from databricks UI
databricks configure --token

# Create a scope with one of the two commands (depending if you have security features enabled on databricks):
# with security add-on
databricks secrets create-scope --scope wandb
# without security add-on
databricks secrets create-scope --scope wandb --initial-manage-principal users

# Add your api_key from: https://app.wandb.ai/authorize
databricks secrets put --scope wandb --key api_key
```

##  **예시**

### Simple

```python
import os
import wandb

api_key = dbutils.secrets.get("wandb", "api_key")
wandb.login(key=api_key)

wandb.init()
wandb.log({"foo": 1})
```

###  **스윕\(Sweeps\)**

wandb.sweep\(\) 또는 wandb.agent\(\)를 사용하려는 notebooks에 필요한 설정 \(임시\) 은 다음과 같습니다:

```python
import os
# These will not be necessary in the future
os.environ['WANDB_ENTITY'] = "my-entity"
os.environ['WANDB_PROJECT'] = "my-project-that-exists"
```

Notebook에서 스윕\(Sweep\)을 실행하는 범에 대한 자세한 설명은 다음에서 확인하실 수 있습니다:

{% page-ref page="../sweeps/python-api.md" %}

