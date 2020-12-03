# LightGBM

コールバックを使用して、LightGBMのパフォーマンスを1行のコードで視覚化します。

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

[ 例のリポジトリ](https://github.com/wandb/examples/tree/master/examples/boosting-algorithms/lightgbm-regression)で、または[colab](https://colab.research.google.com/drive/1R6_vcVM90Ephyu0HDFlPAZa0SgEC_3bE)ノートブックとして完全なコード例を参照してください。

