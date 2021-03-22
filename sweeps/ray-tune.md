---
description: ✨BETA✨ support for Ray Tune sweep search and scheduler API
---

# Ray Tune Sweeps

[Ray Tune](https://ray.readthedocs.io/en/latest/tune.html) is a scalable hyperparameter tuning library. We're adding support for Ray Tune to W&B Sweeps, which makes it easy to launch runs on many machines and visualize results in a central place.

{% hint style="info" %}
Also check out [the Ray Tune integrations for W&B](../integrations/ray-tune.md) for a feature complete, out-of-the-box solution for leveraging both Ray Tune and W&B!
{% endhint %}

{% hint style="info" %}
This feature is in beta! We love feedback, and we really appreciate hearing from folks who are experimenting with our Sweeps product.
{% endhint %}

Here's a quick example:

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

[See full example on GitHub →](https://github.com/wandb/examples/tree/master/examples/keras/keras-cnn-fashion)

## Feature Compatibility

### Search Algorithms

[Ray/Tune Search Algorithms](https://ray.readthedocs.io/en/latest/tune-searchalg.html)

| Search Algorithm | Support |
| :--- | :--- |
| [HyperOpt](https://ray.readthedocs.io/en/latest/tune-searchalg.html#hyperopt-search-tree-structured-parzen-estimators) | **Supported** |
| [Grid Search and Random Search](https://ray.readthedocs.io/en/latest/tune-searchalg.html#variant-generation-grid-search-random-search) | Partial |
| [BayesOpt](https://ray.readthedocs.io/en/latest/tune-searchalg.html#bayesopt-search) | Planned |
| [Nevergrad](https://ray.readthedocs.io/en/latest/tune-searchalg.html#nevergrad-search) | Planned |
| [Scikit-Optimize](https://ray.readthedocs.io/en/latest/tune-searchalg.html#scikit-optimize-search) | Planned |
| [Ax](https://ray.readthedocs.io/en/latest/tune-searchalg.html#ax-search) | Planned |
| [BOHB](https://ray.readthedocs.io/en/latest/tune-searchalg.html#bohb) | Planned |

### HyperOpt

| HyperOpt Feature | Support |
| :--- | :--- |
| hp.choice | Supported |
| hp.randint | Planned |
| hp.pchoice | Planned |
| hp.uniform | Supported |
| hp.uniformint | Planned |
| hp.quniform | Planned |
| hp.loguniform | Supported |
| hp.qloguniform | Planned |
| hp.normal | Planned |
| hp.qnormal | Planned |
| hp.lognormal | Planned |
| hp.qlognormal | Planned |

### Tune Schedulers

By default, Tune schedules runs in serial order. You can also specify a custom scheduling algorithm that can stop runs early or perturb parameters. Read more in the [Tune docs →](https://ray.readthedocs.io/en/latest/tune-schedulers.html)

| Scheduler | Support |
| :--- | :--- |
| [Population Based Training \(PBT\)](https://ray.readthedocs.io/en/latest/tune-schedulers.html#population-based-training-pbt) | Investigating |
| [Asynchronous HyperBand](https://ray.readthedocs.io/en/latest/tune-schedulers.html#asynchronous-hyperband) | Planned |
| [HyperBand](https://ray.readthedocs.io/en/latest/tune-schedulers.html#hyperband) | Investigating |
| [HyperBand \(BOHB\)](https://ray.readthedocs.io/en/latest/tune-schedulers.html#hyperband-bohb) | Investigating |
| [Median Stopping Rule](https://ray.readthedocs.io/en/latest/tune-schedulers.html#median-stopping-rule) | Investigating |

