---
description: Track your trees with W&B.
---

# XGBoost

The `wandb` library includes a special callback for [XGBoost](https://xgboost.readthedocs.io/en/latest/index.html). It's also easy to use the generic logging features of Weights & Biases to track large experiments, like hyperparameter sweeps.

```python
from wandb.xgboost import wandb_callback
import xgboost as xgb

...

bst = xgb.train(param, train_data, num_round, watchlist,
                callbacks=[wandb_callback()])
```

{% hint style="info" %}
Looking for working code examples? Check out [our repository of examples on GitHub](https://github.com/wandb/examples/tree/master/examples/boosting-algorithms) or try out a [Colab notebook](https://colab.research.google.com/github/wandb/examples/blob/master/colabs/boosting/Credit\_Scorecards\_with\_XGBoost\_and\_W%26B.ipynb)
{% endhint %}

## Tuning your hyperparameters with Sweeps

Attaining the maximum performance out of models requires tuning hyperparameters, like tree depth and learning rate. Weights & Biases includes [Sweeps](../sweeps/), a powerful toolkit for configuring, orchestrating, and analyzing large hyperparameter testing experiments.

{% hint style="info" %}
To learn more about these tools and see an example of how to use Sweeps with XGBoost, check out [this interactive Colab notebook](http://wandb.me/xgb-sweeps-colab) or try this XGBoost & Sweeps [python script here](https://github.com/wandb/examples/blob/master/examples/wandb-sweeps/sweeps-xgboost/xgboost\_tune.py)
{% endhint %}

![tl;dr: trees outperform linear learners on this classification dataset.](<../../.gitbook/assets/image (112).png>)
