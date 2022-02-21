---
description: Track your trees with W&B.
---

# XGBoost & LightGBM

The `wandb` library includes special callbacks for both of the most popular libraries for training gradient-boosted machines: [XGBoost ](https://xgboost.readthedocs.io/en/latest/index.html)and [LightGBM](https://lightgbm.readthedocs.io/en/latest/). It's also easy to use the generic logging features of Weights & Biases to track large experiments, like hyperparameter sweeps.

{% tabs %}
{% tab title="XGBoost" %}
```python
from wandb.xgboost import wandb_callback
import xgboost as xgb

...

bst = xgb.train(param, train_data, num_round, watchlist,
                callbacks=[wandb_callback()])
```
{% endtab %}

{% tab title="LightGBM" %}
```python
from wandb.lightgbm import wandb_callback, log_summary
import lightgbm as lgb

# Log metrics to W&B
gbm = lgb.train(..., callbacks=[wandb_callback()])

# Log feature importance plot and upload model checkpoint to W&B 
log_summary(gbm, save_model_checkpoint=True)
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Looking for working code examples? Check out [our repository of examples on GitHub](https://github.com/wandb/examples/tree/master/examples/boosting-algorithms) or try out a Colab notebook ([XGBoost](http://wandb.me/xgb-colab), [LightGBM](http://wandb.me/lightgbm-colab)).
{% endhint %}

## Tuning your hyperparameters with Sweeps

Attaining the maximum performance out of models requires tuning hyperparameters, like tree depth and learning rate. Weights & Biases includes [Sweeps](../sweeps/), a powerful toolkit for configuring, orchestrating, and analyzing large hyperparameter testing experiments.

{% hint style="info" %}
To learn more about these tools and see an example of how to use Sweeps with XGBoost, check out [this interactive Colab notebook.](http://wandb.me/xgb-sweeps-colab)
{% endhint %}

![tl;dr: trees outperform linear learners on this classification dataset.](<../../.gitbook/assets/image (70).png>)
