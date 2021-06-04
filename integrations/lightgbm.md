# LightGBM

Utilisez notre fonction de rappel \(callback\) pour visualiser les performances de votre LightGBM en une seule ligne de code.

```python
from wandb.lightgbm import wandb_callback
import lightgbm as lgb

....

gbm = lgb.train(params,
                lgb_train,
                num_boost_round=20,
                valid_sets=lgb_eval,
                valid_names=('validation'),
                callbacks=[wandb_callback()])
```

Voir un exemple de code complet dans notre [répertoire d’exemples](https://github.com/wandb/examples/tree/master/examples/boosting-algorithms/lightgbm-regression), ou sous forme de notebook [colab](https://colab.research.google.com/drive/1R6_vcVM90Ephyu0HDFlPAZa0SgEC_3bE).

