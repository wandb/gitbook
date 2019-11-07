# XGBoost Example

This is a complete example of xgboost code that trains a gradient boosted tree and saves the results to W&B. You can find the code on [GitHub](https://github.com/wandb/examples/blob/master/xgboost-dermatology/train.py).

```python
import wandbimport numpy as npimport xgboost as xgbwandb.init()# label need to be 0 to num_class -1data = np.loadtxt('./dermatology.data', delimiter=',',        converters={33: lambda x:int(x == '?'), 34: lambda x:int(x) - 1})sz = data.shapetrain = data[:int(sz[0] * 0.7), :]test = data[int(sz[0] * 0.7):, :]train_X = train[:, :33]train_Y = train[:, 34]test_X = test[:, :33]test_Y = test[:, 34]xg_train = xgb.DMatrix(train_X, label=train_Y)xg_test = xgb.DMatrix(test_X, label=test_Y)# setup parameters for xgboostparam = {}# use softmax multi-class classificationparam['objective'] = 'multi:softmax'# scale weight of positive examplesparam['eta'] = 0.1param['max_depth'] = 6param['silent'] = 1param['nthread'] = 4param['num_class'] = 6wandb.config.update(param)watchlist = [(xg_train, 'train'), (xg_test, 'test')]num_round = 5bst = xgb.train(param, xg_train, num_round, watchlist, callbacks=[wandb.xgboost.wandb_callback()])# get predictionpred = bst.predict(xg_test)error_rate = np.sum(pred != test_Y) / test_Y.shape[0]print('Test error using softmax = {}'.format(error_rate))wandb.summary['Error Rate'] = error_rate
```

