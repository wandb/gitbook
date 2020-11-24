# LightGBM

Use our callback to visualize your LightGBM’s performance in just one line of code.

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

See a complete code example in our [examples repo](https://github.com/wandb/examples/tree/master/examples/boosting-algorithms/lightgbm-regression), or as a [colab](https://colab.research.google.com/drive/1R6_vcVM90Ephyu0HDFlPAZa0SgEC_3bE) notebook.

