# XGBoost

利用我们的回调函数比较你的XGBoost模型的不同版本之间的结果。

```python
bst = xgb.train(param, xg_train, num_round, watchlist,
                callbacks=[wandb.xgboost.wandb_callback()])
```

![](../.gitbook/assets/image%20%2812%29.png)

## **示例**

* [Github上的示例](https://github.com/wandb/examples/tree/master/examples/boosting-algorithms/xgboost-dermatology)：多类皮肤病分类示例

