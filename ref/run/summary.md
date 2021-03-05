# Summary

​[​![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)](https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_summary.py#L82-L133)[GitHub에서 소스 확인하기](https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_summary.py#L82-L133)​​

Summary\(요약\)는 각 실행에 대한 단일 값을 추적합니다. 기본값으로 summary는 히스토리의 최종 값으로 설정됩니다.

```text
summary(    get_current_summary_callback: t.Callable)
```

예를 들어, `wandb.log({'accuracy': 0.9})`는 히스토리에 새 단계를 추가하고 summary를 최종값으로 업데이트합니다. 경우에 따라서, 최종 값 대신 메트릭의 최대 또는 최소치를 갖는 것이 더 유용합니다. 히스토리를 수동으로 설정할 수 있습니다 `(wandb.summary['accuracy'] = best_acc)`.

UI에서 요약 메트릭\(summary metrics\)이 테이블에 나타나 여러 실행을 비교합니다. 요약 메트릭은 또한 산점도 및 평행 좌표 차트와 같은 시각화에도 사용될 수 있습니다.

훈련이 완료된 후, 평가 메트릭을 실행에 저장하고 싶을 수 있습니다. Summary는 넘파이 배열\(numpy array\)과 PyTorch/TensorFlow tensor를 처리할 수 있습니다. 이러한 유형 중 하나를 Summary에 저장하면, 전체 tensor를 이진 파일로 유지하고, 상위 수준 메트릭을 최소, 평균, 분산, 제95 백분위와 같은 요약 객체에 저장합니다.

## **예시:** <a id="examples"></a>

```text
wandb.init(config=args)​best_accuracy = 0for epoch in range(1, args.epochs + 1):test_loss, test_accuracy = test()if (test_accuracy > best_accuracy):    wandb.run.summary["best_accuracy"] = test_accuracy    best_accuracy = test_accuracy
```



