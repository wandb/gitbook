# Summary

## wandb.sdk.wandb\_summary

 ​[\[소스\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_summary.py#L2)​ 

### SummaryDict Objects

```python
@six.add_metaclass(abc.ABCMeta)
class SummaryDict(object)
```

 ​[\[소스\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_summary.py#L18)​ 

SummarySubDict에 모든 중첩된 사전을 래핑\(wrap\)하는 dict-like 객체이며 속성 변경 시에 self.\_root.\_callback을 트리거 합니다.

### Summary Objects

```python
class Summary(SummaryDict)
```

 ​[\[소스\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_summary.py#L78)​

요약

요약 통계는 모델 당 단일 메트릭을 추적하는 데 사용됩니다. wandb.log\({'accuracy': 0.9}\)를 호출하면 코드가 수동으로 wandb.summary\['accuracy'\]를 변경하지 않는 한 자동으로 wandb.summary\['accuracy'\]을 0.9로 설정합니다.

wandb.summary\['accuracy'\]를 수동으로 설정하면 wandb.log\(\)를 사용하여 모든 단계의 정확도를 계속 기록하는 동안 최고 모델의 정확도를 계속 기록하려는 경우에 유용합니다.

 훈련이 완료된 후 평가 메트릭을 실행 요약\(runs summary\)에 저장하는 것이 좋습니다. 요약은 넘파이 배열\(numpy arrays\), pytorch tensor 또는 tensorflow tensors를 처리할 수 있습니다. 값이 이들 유형 중 하나인 경우, 전체 tensor를 이진 파일에 유지하고, 최고 수준 메트릭을 최소, 평균, 분산, 95 백분위수와 같은 요약 객체에 저장합니다.

 **예시**:

```text
wandb.init(config=args)

best_accuracy = 0
for epoch in range(1, args.epochs + 1):
test_loss, test_accuracy = test()
if (test_accuracy > best_accuracy):
wandb.run.summary["best_accuracy"] = test_accuracy
best_accuracy = test_accuracy
```

### SummarySubDict Objects

```python
class SummarySubDict(SummaryDict)
```

 ​[\[소스\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_summary.py#L128)​ 

요약 데이터 구조의 루트가 아닌 노드\(non-root node\)입니다. 루트에서 노드 자체로의 경로를 포함합니다.

