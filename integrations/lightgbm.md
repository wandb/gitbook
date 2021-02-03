# LightGBM

Utiliza nuestro callback para visualizar el desempeño de tu LightGBM con sólo una línea de código.

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

Mira el código de ejemplo completo en nuestro [repositorio de los ejemplos](https://github.com/wandb/examples/tree/master/examples/boosting-algorithms/lightgbm-regression), o como una notebook de [colab](https://colab.research.google.com/drive/1R6_vcVM90Ephyu0HDFlPAZa0SgEC_3bE).

