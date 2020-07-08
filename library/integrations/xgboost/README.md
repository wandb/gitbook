# XGBoost

Use our callback to compare results between different versions of your XGBoost model.

```python
bst = xgb.train(param, xg_train, num_round, watchlist,
                callbacks=[wandb.xgboost.wandb_callback()])
```

Try a[ live code example **→**](http://bit.ly/wandb-xgboost)\*\*\*\*

![](../../../.gitbook/assets/image%20%2812%29.png)

