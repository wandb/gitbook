# LightGBM

저희 callback을 사용하여 LightGBM의 성능을 단 한줄의 코드로 시각화 할 수 있습니다.

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

 저희 [예시 repo](https://github.com/wandb/examples/tree/master/examples/boosting-algorithms/lightgbm-regression) 또는 [colab](https://colab.research.google.com/drive/1R6_vcVM90Ephyu0HDFlPAZa0SgEC_3bE) notebook에서 전체 코드 예시를 참조하시기 바랍니다.

