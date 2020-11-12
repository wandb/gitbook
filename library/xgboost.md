# XGBoost

 利用我们的回调函数比较不同版本XGBoost模型的结果。

```python
bst = xgb.train(param, xg_train, num_round, watchlist,
                callbacks=[wandb.xgboost.wandb_callback()])
```

![](../.gitbook/assets/image%20%2812%29.png)

## **范例**

*  [Github上的范例](https://github.com/wandb/examples/tree/master/examples/boosting-algorithms/xgboost-dermatology)：多类皮肤病分类范例

