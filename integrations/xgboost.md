# XGBoost

저희의 callback을 사용해서 다른 버전의 XGBoost 모델 간 결과를 비교하실 수 있습니다.

```python
bst = xgb.train(param, xg_train, num_round, watchlist,
                callbacks=[wandb.xgboost.wandb_callback()])
```

![](../.gitbook/assets/image%20%2812%29.png)

## **예시** 

* [Github에서의 예시](https://github.com/wandb/examples/tree/master/examples/boosting-algorithms/xgboost-dermatology): 멀티클래스\(multi-class\) 피부과\(dermatology\) 분류 예시

