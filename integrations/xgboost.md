# XGBoost

Use our callback to compare results between different versions of your XGBoost model.

```python
bst = xgb.train(param, xg_train, num_round, watchlist,
                callbacks=[wandb.xgboost.wandb_callback()])
```

![](../.gitbook/assets/image%20%2812%29.png)

## Example

* [Example on Github](https://github.com/wandb/examples/tree/master/examples/boosting-algorithms/xgboost-dermatology): Multi-class dermatology classification example

