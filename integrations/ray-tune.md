# Ray Tune

W&B s’intègre avec [Ray](https://github.com/ray-project/ray) en offrant deux intégrations très légères.

La première est le`WandbLogger`, qui enregistre automatiquement les mesures rapportées à Tune dans l’API Wandb. L’autre est le décorateur `@wandb_mixin` qui peut être utilisé avec l’API fonction. Elle initialise automatiquement l’API Wandb avec les informations d’entraînement de Tune. Vous pouvez utiliser l’API Wandb comme vous le feriez normalement, e.g. en utilisant `wandb.log()` pour enregistrer votre processus d’entraînement.

## WandbLogger

```python
from ray.tune.integration.wandb import WandbLogger
```

 La configuration de Wandb se fait en passant une clef wandb dans le paramètre de configuration de `tune.run()` \(voir exemple plus bas\).

Le contenu de la config entry de wandb est passé dans `wandb.init()` en tant qu’arguments mots-clefs. Les exceptions sont les options suivantes, qui sont utilisées pour configurer le `WandbLogger` en lui-même :

###  Paramètres

`api_key_file (str)` – Chemin vers le fichier qui contient la `Wandb API KEY` \(clef API Wandb\).

`api_key (str)` – Clef API Wandb. Alternative à la mise en place de `api_key_file`.

`excludes (list)` – Liste de mesures qui devront être exclues du `log`.

`log_config (bool)` – Booléen qui indique si le paramètre de config des résultats dict doit être enregistré. C’est logique si des paramètres doivent changer pendant l’entraînement, e.g. avec `PopulationBasedTraining`. Par défaut, False.

### Exemple

```python
from ray.tune.logger import DEFAULT_LOGGERS
from ray.tune.integration.wandb import WandbLogger
tune.run(
    train_fn,
    config={
        # define search space here
        "parameter_1": tune.choice([1, 2, 3]),
        "parameter_2": tune.choice([4, 5, 6]),
        # wandb configuration
        "wandb": {
            "project": "Optimization_Project",
            "api_key_file": "/path/to/file",
            "log_config": True
        }
    },
    loggers=DEFAULT_LOGGERS + (WandbLogger, ))
```

## wandb\_mixin

```python
ray.tune.integration.wandb.wandb_mixin(func)
```

 Ce `mixin` Ray Tune Trainable aide à l’initialisation de l’API Wandb pour être utilisé avec la classe Trainable ou avec `@wandb_mixin` pour l’API fonction.

Pour une utilisation basique, ajoutez simplement le décorateur `@wandb_mixin` à votre fonction d’entraînement :

```python
from ray.tune.integration.wandb import wandb_mixin

@wandb_mixin
def train_fn(config):
    wandb.log()
```

La configuration de Wandb se fait en passant une `wandb key` dans le paramètre de `config` de `tune.run()` \(voir exemple plus bas\).

 Le contenu de la config entry de wandb est passé dans `wandb.init()` en tant qu’arguments mots-clefs. Les exceptions sont les options suivantes, qui sont utilisées pour configurer le `WandbTrainableMixin` en lui-même :

### Paramètres

`api_key_file (str)` – Chemin vers le fichier qui contient la Wandb `API KEY` \(clef API Wandb\).

`api_key (str)` – Clef API Wandb. Alternative à la mise en place de `api_key_file`.

 Les `group`, `run_id` et `run_name` de Wandb sont automatiquement sélectionnés par Tune, mais peuvent être remplacés en remplissant leurs valeurs de configuration respectives.

Veuillez vous reporter à cette page pour voir tous les autres réglages de configuration valide : [https://docs.wandb.com/library/init](https://docs.wandb.com/library/init)

### Exemple:

```python
from ray import tune
from ray.tune.integration.wandb import wandb_mixin

@wandb_mixin
def train_fn(config):
    for i in range(10):
        loss = self.config["a"] + self.config["b"]
        wandb.log({"loss": loss})
        tune.report(loss=loss)

tune.run(
    train_fn,
    config={
        # define search space here
        "a": tune.choice([1, 2, 3]),
        "b": tune.choice([4, 5, 6]),
        # wandb configuration
        "wandb": {
            "project": "Optimization_Project",
            "api_key_file": "/path/to/file"
        }
    })
```

### Exemples de code

Nous avons créé quelques exemples pour que vous puissiez voir comment fonctionne cette intégration :

* [Colab](https://colab.research.google.com/drive/1an-cJ5sRSVbzKVRub19TmmE4-8PUWyAi?usp=sharing): Une démo simple pour essayer l’intégration
* [Tableau de bord ](https://app.wandb.ai/authors/rayTune?workspace=user-cayush): Voir le tableau de bord généré par cet exemple

