---
description: ✨BETA✨ Ray Tuneスイープ検索とスケジューラAPIのサポート
---

# Ray Tune Sweeps

 [Ray Tune](https://ray.readthedocs.io/en/latest/tune.html)は、スケーラブルなハイパーパラメータ調整ライブラリです。Ray TuneのサポートをW＆Bスイープに追加します。これにより、多くのマシンで実行を簡単に開始し、結果を一元的に視覚化できます。

{% hint style="info" %}
また、[W＆BのRay Tune統合](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MNdlQobOrN8f63KfkRZ/v/japanese/integrations/ray-tune)をチェックして、Ray TuneとW＆Bの両方を活用するための完全なすぐに使えるソリューションを確認してください。
{% endhint %}

{% hint style="info" %}
 この機能はベータ版です！私たちはフィードバックを望んでおり、私たちのスイープ製品を実験している人々からの連絡を楽しみにしています。
{% endhint %}

 簡単な例を次に示します。

```python
import wandb
from wandb.sweeps.config import tune
from wandb.sweeps.config.tune.suggest.hyperopt import HyperOptSearch
from wandb.sweeps.config.hyperopt import hp

tune_config = tune.run(
    "train.py",
    search_alg=HyperOptSearch(
        dict(
            width=hp.uniform("width", 0, 20),
            height=hp.uniform("height", -100, 100),
            activation=hp.choice("activation", ["relu", "tanh"])),
        metric="mean_loss",
        mode="min"),
    num_samples=10)

# Save sweep as yaml config file
tune_config.save("sweep-hyperopt.yaml")

# Create the sweep
wandb.sweep(tune_config)
```

[GitHubで完全な例を参照してください→](https://github.com/wandb/examples/tree/master/examples/keras/keras-cnn-fashion)

##  機能の互換性

### 検索アルゴリズム

 [Ray/Tune検索アルゴリズム](https://ray.readthedocs.io/en/latest/tune-searchalg.html)

| 検索アルゴリズム |  サポート |
| :--- | :--- |
| [HyperOpt](https://ray.readthedocs.io/en/latest/tune-searchalg.html#hyperopt-search-tree-structured-parzen-estimators) | **Supported** |
| [Grid Search and Random Search](https://ray.readthedocs.io/en/latest/tune-searchalg.html#variant-generation-grid-search-random-search) |  部分的 |
| [BayesOpt](https://ray.readthedocs.io/en/latest/tune-searchalg.html#bayesopt-search) | 予定 |
| [Nevergrad](https://ray.readthedocs.io/en/latest/tune-searchalg.html#nevergrad-search) | 予定 |
| [Scikit-Optimize](https://ray.readthedocs.io/en/latest/tune-searchalg.html#scikit-optimize-search) | 予定 |
| [Ax](https://ray.readthedocs.io/en/latest/tune-searchalg.html#ax-search) | 予定 |
| [BOHB](https://ray.readthedocs.io/en/latest/tune-searchalg.html#bohb) | 予定 |

###  スケジューラーの調整

| スケジューラーサポート  |  サポート |
| :--- | :--- |
| hp.choice | 調査中 |
| hp.randint | 予定 |
| hp.pchoice | 予定 |
| hp.uniform |  調査中 |
| hp.uniformint |  予定 |
| hp.quniform | 予定 |
| hp.loguniform | 調査中 |
| hp.qloguniform |  予定 |
| hp.normal | 予定 |
| hp.qnormal | 予定 |
| hp.lognormal | 予定 |
| hp.qlognormal | 予定 |

###  スイープ結果を視覚化します

By default, Tune schedules runs in serial order. You can also specify a custom scheduling algorithm that can stop runs early or perturb parameters. Read more in the [Tune docs →](https://ray.readthedocs.io/en/latest/tune-schedulers.html)

| Scheduler | Support |
| :--- | :--- |
| [Population Based Training \(PBT\)](https://ray.readthedocs.io/en/latest/tune-schedulers.html#population-based-training-pbt) | Investigating |
| [Asynchronous HyperBand](https://ray.readthedocs.io/en/latest/tune-schedulers.html#asynchronous-hyperband) | Planned |
| [HyperBand](https://ray.readthedocs.io/en/latest/tune-schedulers.html#hyperband) | Investigating |
| [HyperBand \(BOHB\)](https://ray.readthedocs.io/en/latest/tune-schedulers.html#hyperband-bohb) | Investigating |
| [Median Stopping Rule](https://ray.readthedocs.io/en/latest/tune-schedulers.html#median-stopping-rule) | Investigating |

