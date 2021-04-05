# XGBoost

Utilisez notre callback pour comparer les résultats entre les différentes versions de votre modèle XGBoost.

```python
bst = xgb.train(param, xg_train, num_round, watchlist,
                callbacks=[wandb.xgboost.wandb_callback()])
```

![](../.gitbook/assets/image%20%2812%29.png)

##  Exemple

*  [Exemple sur Github](https://github.com/wandb/examples/tree/master/examples/boosting-algorithms/xgboost-dermatology) : Exemple de classification multi-classe en dermatologie 

