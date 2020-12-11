---
description: ✨베타✨ Ray Tune 스윕 검색 및 스케줄러 API 지원
---

# Ray Tune Sweeps

[  
Ray Tune](https://ray.readthedocs.io/en/latest/tune.html)은 확장 가능한 초매개변수 튜닝 라이브러리입니다. 저희는 W&B 스윕에 Ray Tune에 대한 지원을 추가하고 있으며, 즉, 다양한 머신에서 실행을 시작하고 중앙에서 결과 시각화를 쉽고 간편하게 하실 수 있습니다.

{% hint style="info" %}
Ray Tune과 W&B를 모두 활용을 위한 완벽하고 창의적인 솔루션에 대해서 [W&B용 Ray Tune 통합](https://docs.wandb.com/library/integrations/ray-tune)을 확인해보시기 바랍니다!
{% endhint %}

{% hint style="info" %}
이 기능은 베타 버전입니다! 저희는 여러분의 피드백을 기다리고 있습니다! 저희 스윕 제품으로 실험을 하고 계시는 분들의 의견을 들을 수 있다면 정말로 감사하겠습니다.
{% endhint %}

 다음은 간단한 예시입니다:

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

 [GitHub에서 전체 예시 보기 →](https://github.com/wandb/examples/tree/master/examples/keras/keras-cnn-fashion)​

##  **기능 호환성**

###  **검색 알고리듬**

 [Ray/Tune 검색 알고리듬](https://ray.readthedocs.io/en/latest/tune-searchalg.html)

| 검색 알고리듬 | 지원여부 |
| :--- | :--- |
| [HyperOpt](https://ray.readthedocs.io/en/latest/tune-searchalg.html#hyperopt-search-tree-structured-parzen-estimators) | **지원됨** |
| [Grid Search and Random Search](https://ray.readthedocs.io/en/latest/tune-searchalg.html#variant-generation-grid-search-random-search) | 부분 지원 |
| [BayesOpt](https://ray.readthedocs.io/en/latest/tune-searchalg.html#bayesopt-search) | 계획됨 |
| [Nevergrad](https://ray.readthedocs.io/en/latest/tune-searchalg.html#nevergrad-search) | 계획됨 |
| [Scikit-Optimize](https://ray.readthedocs.io/en/latest/tune-searchalg.html#scikit-optimize-search) | 계획됨 |
| [Ax](https://ray.readthedocs.io/en/latest/tune-searchalg.html#ax-search) | 계획됨 |
| [BOHB](https://ray.readthedocs.io/en/latest/tune-searchalg.html#bohb) | 계획됨 |

### HyperOpt

| HyperOpt 기능 | 지원 |
| :--- | :--- |
| hp.choice | 지원됨 |
| hp.randint | 계획됨 |
| hp.pchoice | 계획됨 |
| hp.uniform | 지원됨 |
| hp.uniformint | 계획됨 |
| hp.quniform | 계획됨 |
| hp.loguniform | 지원됨 |
| hp.qloguniform | 계획됨 |
| hp.normal | 계획됨 |
| hp.qnormal | 계획됨 |
| hp.lognormal | 계획됨 |
| hp.qlognormal | 계획됨 |

### **Tune 스케줄러**

기본값으로 Tune 스커줄러는 순차적으로 실행됩니다. 또한 실행을 조기에 중지하거나 매개변수를 교란\(perturb\)시킬 수 있는 사용자 지정 스케줄링 알로리듬을 지정할 수도 있습니다. [Tune 문서](https://ray.readthedocs.io/en/latest/tune-schedulers.html)에서 더 자세한 내용을 읽어보시기 바랍니다.

| 스케줄러 | 지원 |
| :--- | :--- |
| [Population Based Training \(PBT\)](https://ray.readthedocs.io/en/latest/tune-schedulers.html#population-based-training-pbt) | 조사중 |
| [Asynchronous HyperBand](https://ray.readthedocs.io/en/latest/tune-schedulers.html#asynchronous-hyperband) | 계획됨 |
| [HyperBand](https://ray.readthedocs.io/en/latest/tune-schedulers.html#hyperband) | 조사중 |
| [HyperBand \(BOHB\)](https://ray.readthedocs.io/en/latest/tune-schedulers.html#hyperband-bohb) | 조사중 |
| [Median Stopping Rule](https://ray.readthedocs.io/en/latest/tune-schedulers.html#median-stopping-rule) | 조사중 |

