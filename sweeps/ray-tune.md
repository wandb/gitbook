---
description: >-
  ✨BETA✨ soporte para la búsqueda de barridos Ray Tune y la API del
  planificador.
---

# Ray Tune Sweeps

[**R**](https://ray.readthedocs.io/en/latest/tune.html)[**ay Tune**](https://ray.readthedocs.io/en/latest/tune.html) es una biblioteca de puesta a punto de hiperparámetros escalables. Estamos agregando soporte para Ray Tune a los Barridos de W&B, lo que facilita lanzar ejecuciones sobre muchas máquinas y visualizar los resultados en un lugar centralizado.

{% hint style="info" %}
¡También revisa las [integraciones de Ray Tune para W&B](https://docs.wandb.ai/integrations/ray-tune) para obtener una solución llena de características, y lista para usarse, que hace uso tanto de Ray Tune como de W&B!
{% endhint %}

{% hint style="info" %}
¡Esta característica está en estado beta! Amamos los comentarios, y realmente apreciamos tener noticias de personas que estén experimentando con nuestro producto de los Barridos.
{% endhint %}

Aquí hay un ejemplo simple:

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

 [Mira el ejemplo completo en GitHub →](https://github.com/wandb/examples/tree/master/examples/keras/keras-cnn-fashion)

## Compatibilidad de Características

### Algoritmos de Búsqueda

 [Algoritmos de Búsqueda de Ray/Tune](https://ray.readthedocs.io/en/latest/tune-searchalg.html)

| Algoritmo de Búsqueda | Soporte |
| :--- | :--- |
| [HyperOpt](https://ray.readthedocs.io/en/latest/tune-searchalg.html#hyperopt-search-tree-structured-parzen-estimators) | Soportado |
| [Grid Search and Random Search](https://ray.readthedocs.io/en/latest/tune-searchalg.html#variant-generation-grid-search-random-search) | Parcial |
| [BayesOpt](https://ray.readthedocs.io/en/latest/tune-searchalg.html#bayesopt-search) | Planificado |
| [Nevergrad](https://ray.readthedocs.io/en/latest/tune-searchalg.html#nevergrad-search) | Planificado |
| [Scikit-Optimize](https://ray.readthedocs.io/en/latest/tune-searchalg.html#scikit-optimize-search) | Planificado |
| [Ax](https://ray.readthedocs.io/en/latest/tune-searchalg.html#ax-search) | Planificado |
| [BOHB](https://ray.readthedocs.io/en/latest/tune-searchalg.html#bohb) | Planificado |

### HyperOpt

| Característica de HyperOpt | Soporte |
| :--- | :--- |
| hp.choice | Soporte |
| hp.randint | Planificado |
| hp.pchoice | Planificado |
| hp.uniform | Soporte |
| hp.uniformint | Planificado |
| hp.quniform | Planificado |
| hp.loguniform | Soporte |
| hp.qloguniform | Planificado |
| hp.normal | Planificado |
| hp.qnormal | Planificado |
| hp.lognormal | Planificado |
| hp.qlognormal | Planificado |

### Planificadores de Tune

Por defecto, Tune planifica las ejecuciones en un orden serial. También puedes especificar un algoritmo de planificación personalizado que puede detener las ejecuciones de forma temprana o pueda perturbar los parámetros. Lee más en la [**documentación de Tune →**](https://ray.readthedocs.io/en/latest/tune-schedulers.html)

| Scheduler | Soporte |
| :--- | :--- |
| [Population Based Training \(PBT\)](https://ray.readthedocs.io/en/latest/tune-schedulers.html#population-based-training-pbt) | En investigación |
| [Asynchronous HyperBand](https://ray.readthedocs.io/en/latest/tune-schedulers.html#asynchronous-hyperband) | Planificado |
| [HyperBand](https://ray.readthedocs.io/en/latest/tune-schedulers.html#hyperband) | En investigación |
| [HyperBand \(BOHB\)](https://ray.readthedocs.io/en/latest/tune-schedulers.html#hyperband-bohb) | En investigación |
| [Median Stopping Rule](https://ray.readthedocs.io/en/latest/tune-schedulers.html#median-stopping-rule) | En investigación |

