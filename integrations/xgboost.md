# XGBoost

Utiliza nuestro callback para comparar los resultados entre las diferentes versiones de tu modelo XGBoost.

```python
bst = xgb.train(param, xg_train, num_round, watchlist,
                callbacks=[wandb.xgboost.wandb_callback()])
```

![](../.gitbook/assets/image%20%2812%29.png)

##  Ejemplo

* [Ejemplo en Github](https://github.com/wandb/examples/tree/master/examples/boosting-algorithms/xgboost-dermatology): Ejemplo de clasificación dermatológica de múltiples clases

