# History

## wandb.sdk.wandb\_history

 ​[\[소스\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_history.py#L3)​

History는 시간 경과에 따라 로그된 데이터를 추적합니다. 스크립트의 히스토리를 사용하려면 훈련 루프의 단일 시간 단계 또는 여러 번 wandb.log\({"key": value}\)을 호출합니다. 이를 통해 저장된 스칼라 또는 히스토리에 저장되는 미디어의 시계열이 생성됩니다.

UI에서, 스칼라를 여러 시간 단계에 로그 하려는 경우, W&B는 기본값으로 이러한 히스토리 메트릭을 라인 플롯으로 렌더링 합니다. 히스토리에 단일 값을 로그 하는 경우, 여러 실행을 막대그래프를 통해 비교합니다.

전체 시계열 및 단일 요약 값을 추적에 유용합니다. 히스토리의 모든 단계의 정확도 및 요약의 최고 정확도 그 예입니다. 기본값으로, 요약은 히스토리의 최종 값으로 설정됩니다.

### History Objects

```python
class History(object)
```

 ​[\[소스\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_history.py#L23)​ 

실행에 대한 시계열 데이터입니다. 이는 근본적으로 dicts의 리스트이며 여기서 각 dict는 로그된 요약 통계의 세트입니다.

