# MLflow \(beta\)

> MLFlow 통합은 현재 베터 버전이며, 공식적인 wandb python 패키지의 일부가 아닙니다. 이 통합\(integration\)을 해보시려면, 아래를 실행하여 저희 git 브랜치\(branch\)에서 wandb를 설치하십시오:

```bash
pip install --upgrade git+git://github.com/wandb/client.git@feature/mlflow#egg=wandb
```

##  **MLflow 통합**

I 이미 [MLflow](https://www.mlflow.org/docs/latest/tracking.html)를 사용하여 실험을 추적하고 계신 경우, W&B로 간편하게 시각화 하실 수 있습니다. 여러분의 mlflow 스크립트에 `import wandb`를 호출하시기만 하면, 저희가 모든 메트릭, 매개변수, 아티펙트를 W&B로 미러링합니다. mlflow [python 라이브러리](https://github.com/mlflow/mlflow)를 패치하여 이 작업을 수행합니다. 현재 통합은 쓰기 전용입니다. 모든 데이터는, mlflow 용으로 여러분이 구성한 [backend](https://www.mlflow.org/docs/latest/tracking.html#where-runs-are-recorded)에 작성됩니다.

##  **컨셉 맵핑**

 데이터를 wandb와 mlflow 추적 백엔드\(backend\) 모두에 미러링할 때, 다음의 컨셉이 각각 맵핑됩니다.

| MLflow | W&B |
| :--- | :--- |
| [Experiment](https://www.mlflow.org/docs/latest/tracking.html#organizing-runs-in-experiments) | [Project](../app/pages/project-page.md) |
| [mlflow.start\_run](https://www.mlflow.org/docs/latest/python_api/mlflow.html#mlflow.start_run) | [wandb.init](../library/init.md) |
| [mlflow.log\_params](https://www.mlflow.org/docs/latest/python_api/mlflow.html#mlflow.log_param) | [wandb.config](../library/config.md) |
| [mlflow.log\_metrics](https://www.mlflow.org/docs/latest/python_api/mlflow.html#mlflow.log_metric) | [wandb.log](../library/log.md) |
| [mlflow.log\_artifacts](https://www.mlflow.org/docs/latest/python_api/mlflow.html#mlflow.log_artifact) | [wandb.save](../library/save.md) |
| [mlflow.start\_run\(nested=True\)](https://mlflow.org/docs/latest/python_api/mlflow.html#mlflow.start_run) | [Grouping](../library/grouping.md) |

##  **풍부한 메트릭 로깅**

이미지, 비디오, 플롯과 같은 풍부한 미디어를 로그하고 싶으시다면, [wandb.log](https://docs.wandb.com/library/log)을 여러분의 코드에서도 호출하실 수 있습니다. 단계 전달인자\(step argument\)를 로그할 호출에 전달하여, mlflow로 로깅하는 메트릭과 정렬되도록 하셔야 합니다.

##  **고급 구성**

  기본값으로 wandb는 메트릭, 매개변수, 아티펙트만을 로그합니다. wandb를 통해 아티펙트를 저장하고 싶지 않으시다면 `WANDB_SYNC_MLFLOW=metrics,params` 을 설정하실 수 있습니다. wandb로의 모든 데이터 미러링을 비활성화 하고 싶으시다면, `WANDB_SYNC_MLFLOW=false` 을 설정하시면 됩니다.

