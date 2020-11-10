---
description: ✨BETA✨支持Ray Tune扫描搜索和scheduler API
---

# Ray Tune Sweeps

[Ray Tune](https://ray.readthedocs.io/en/latest/tune.html) 是可扩展的超参数调试库。 我们在W＆B Sweeps中增加了对Ray Tune的支持，这使得在多台机器上可以轻松启动运行，并在中央位置可视化结果。

{% hint style="info" %}
另外，请查看适用于[W＆B的Ray Tune集成](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MKaPhwzNIegNuInaekR/library/integrations/ray-tune)，以获取功能完整的、即用型的解决方案，以同时利用Ray Tune和W＆B！
{% endhint %}

{% hint style="info" %}
这个功能处于测试阶段！ 我们喜欢反馈，非常感谢听到正在试用我们的Sweeps产品的人们的来信。
{% endhint %}

这是一个简单的例子：

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

[在GitHub查看完整示例→](https://github.com/wandb/examples/tree/master/examples/keras/keras-cnn-fashion)

## **特征兼容性**

**搜索算法**

[Ray/Tune搜索算法](https://docs.ray.io/en/latest/tune-searchalg.html)

| 搜索算法 |  |
| :--- | :--- |
| [HyperOpt](https://ray.readthedocs.io/en/latest/tune-searchalg.html#hyperopt-search-tree-structured-parzen-estimators) | 支持 |
| [Grid Search and Random Search](https://ray.readthedocs.io/en/latest/tune-searchalg.html#variant-generation-grid-search-random-search) | 部分支持 |
| [BayesOpt](https://ray.readthedocs.io/en/latest/tune-searchalg.html#bayesopt-search) | 计划支持 |
| [Nevergrad](https://ray.readthedocs.io/en/latest/tune-searchalg.html#nevergrad-search) | 计划支持 |
| [Scikit-Optimize](https://ray.readthedocs.io/en/latest/tune-searchalg.html#scikit-optimize-search) | 计划支持 |
| [Ax](https://ray.readthedocs.io/en/latest/tune-searchalg.html#ax-search) | 计划支持 |
| [BOHB](https://ray.readthedocs.io/en/latest/tune-searchalg.html#bohb) | 计划支持 |

## HyperOpt

| HyperOpt 特征 | 是否支持 |
| :--- | :--- |
| hp.choice | 支持 |
| hp.randint | 计划支持 |
| hp.pchoice | 计划支持 |
| hp.uniform | 支持 |
| hp.uniformint | 计划支持 |
| hp.quniform | 计划支持 |
| hp.loguniform | 支持 |
| hp.qloguniform | 计划支持 |
| hp.normal | 计划支持 |
| hp.qnormal | 计划支持 |
| hp.lognormal | 计划支持 |
| hp.qlognormal | 计划支持 |

**调试安排**

_\*\*_默认情况下，调试安排以顺序运行。 您还可以指定自定义调度算法，该算法可以提前停止运行或扰动参数。 在 [Tune docs ](https://ray.readthedocs.io/en/latest/tune-schedulers.html)中阅读更多内容→

| 安排表 | 是否支持 |
| :--- | :--- |
| [Population Based Training \(PBT\)](https://ray.readthedocs.io/en/latest/tune-schedulers.html#population-based-training-pbt) | 调查中 |
| [Asynchronous HyperBand](https://ray.readthedocs.io/en/latest/tune-schedulers.html#asynchronous-hyperband) | 计划支持 |
| [HyperBand](https://ray.readthedocs.io/en/latest/tune-schedulers.html#hyperband) | 调查中 |
| [HyperBand \(BOHB\)](https://ray.readthedocs.io/en/latest/tune-schedulers.html#hyperband-bohb) | 调查中 |
| [Median Stopping Rule](https://ray.readthedocs.io/en/latest/tune-schedulers.html#median-stopping-rule) | 调查中 |

