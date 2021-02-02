---
description: >-
  Prise en charge ✨BETA✨ pour la recherche de balayage Ray Tune et le Scheduler
  API
---

# Ray Tune Sweeps

[Ray Tune](https://ray.readthedocs.io/en/latest/tune.html) est une librairie de réglages d’hyperparamètres évolutive. Nous ajoutons la prise en charge de Ray Tune sur les Balayages W&B, ce qui rend plus facile le lancement d’essais sur de nombreuses machines et la visualisation des résultats en un point central.

{% hint style="info" %}
Jetez aussi un œil [aux intégrations Ray Tune pour W&B](https://docs.wandb.ai/integrations/ray-tune) pour une solution complète prête à l’emploi, pour exploiter à la fois Ray Tune et W&B !
{% endhint %}

{% hint style="info" %}
Cette fonctionnalité est en beta ! Nous adorons les retours, et nous aimons vraiment entendre les critiques de personnes qui expérimentent avec nos produits de Balayage.
{% endhint %}

Voici un exemple rapide :

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

 [Voir exemple complet sur GitHub →](https://github.com/wandb/examples/tree/master/examples/keras/keras-cnn-fashion)

##  Compatibilité de fonctionnalités

### Algorithmes de recherche

 [Algorithmes de recherche Ray/Tune ](https://ray.readthedocs.io/en/latest/tune-searchalg.html)

| **Algorithme de recherche** | **Prise en charge** |
| :--- | :--- |
| [HyperOpt](https://ray.readthedocs.io/en/latest/tune-searchalg.html#hyperopt-search-tree-structured-parzen-estimators) | **Pris en charge** |
|  [Recherche grille et Recherche aléatoire](https://ray.readthedocs.io/en/latest/tune-searchalg.html#variant-generation-grid-search-random-search) | Partielle |
| [BayesOpt](https://ray.readthedocs.io/en/latest/tune-searchalg.html#bayesopt-search) | Planifiée |
| [Nevergrad](https://ray.readthedocs.io/en/latest/tune-searchalg.html#nevergrad-search) | Planifiée |
| [Scikit-Optimize](https://ray.readthedocs.io/en/latest/tune-searchalg.html#scikit-optimize-search) | Planifiée |
| [Ax](https://ray.readthedocs.io/en/latest/tune-searchalg.html#ax-search) | Planifiée |
| [BOHB](https://ray.readthedocs.io/en/latest/tune-searchalg.html#bohb) | Planifiée |

### HyperOpt

|  | **Prise en charge** |
| :--- | :--- |
| hp.choice | Pris en charge |
| hp.randint | Planifiée |
| hp.pchoice | Planifiée |
| hp.uniform | Pris en charge |
| hp.uniformint | Planifié |
| hp.quniform | Planifié |
| hp.loguniform | Pris en charge |
| hp.qloguniform | Planifié |
| hp.normal | Planifié |
| hp.qnormal | Planifié |
| hp.lognormal | Planifié |
| hp.qlognormal | Planifié |

###  Schedulers Tune

Par défaut, Tune prévoit les essais dans un ordre de série. Vous pouvez aussi spécifier un algorithme de prévision personnalisé qui peut arrêter les essais de manière précoce ou perturber les paramètres. Lisez-en plus à ce sujet dans la [docu Tune →](https://ray.readthedocs.io/en/latest/tune-schedulers.html)

| Scheduler | **Prise en charge** |
| :--- | :--- |
| [Population Based Training \(PBT\)](https://ray.readthedocs.io/en/latest/tune-schedulers.html#population-based-training-pbt) | En cours d’étude |
| [Asynchronous HyperBand](https://ray.readthedocs.io/en/latest/tune-schedulers.html#asynchronous-hyperband) |   |
| [HyperBand](https://ray.readthedocs.io/en/latest/tune-schedulers.html#hyperband) |  |
| [HyperBand \(BOHB\)](https://ray.readthedocs.io/en/latest/tune-schedulers.html#hyperband-bohb) |  En cours d’étude |
| [Median Stopping Rule](https://ray.readthedocs.io/en/latest/tune-schedulers.html#median-stopping-rule) | En cours d’étude |

