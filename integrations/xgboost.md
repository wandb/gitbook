# XGBoost

 コールバックを使用して、XGBoostモデルの異なるバージョン間の結果を比較します。

```python
bst = xgb.train(param, xg_train, num_round, watchlist,
                callbacks=[wandb.xgboost.wandb_callback()])
```

![](../.gitbook/assets/image%20%2812%29.png)

## **例**

*  [Githubの例](https://github.com/wandb/examples/tree/master/examples/boosting-algorithms/xgboost-dermatology)：マルチクラス皮膚科学分類の例

