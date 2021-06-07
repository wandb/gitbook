---
description: Exécuter des balayages à partir de notebooks Jupyter
---

# Sweep from Jupyter Notebook

##  Initialiser un balayage

```python
import wandb

sweep_config = {
  "name": "My Sweep",
  "method": "grid",
  "parameters": {
        "param1": {
            "values": [1, 2, 3]
        }
    }
}

sweep_id = wandb.sweep(sweep_config)
```

{% hint style="info" %}
Utilisez les méthodes suivantes pour spécifier l’entité ou le projet pour le balayage :

* Arguments pour wandb.sweep\(\) Par exemple :`wandb.sweep(sweep_config, entity="user", project="my_project")`
*  [Variables d’environnement](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MSYLFRJY9U2Uk-CZtzR/v/francais/library/environment-variables)`WANDB_ENTITY` et `WANDB_PROJECT`
*  [Interface de Ligne de commande](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MSYLFRJY9U2Uk-CZtzR/v/francais/library/cli) en utilisant la commande `wandb init`
* [Configuration de balayage](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MSYLFRJY9U2Uk-CZtzR/v/francais/sweeps/configuration) en utilisant les clefs "entity" et "project"
{% endhint %}

##  Exécuter un agent

 Lorsque vous exécuter un agent depuis python, l’agent exécute une fonction spécifiée plutôt que d’utiliser la clef program depuis le fichier de configuration de balayage.  


```python
import wandb
import time

def train():
    run = wandb.init()
    print("config:", dict(run.config))
    for epoch in range(35):
        print("running", epoch)
        wandb.log({"metric": run.config.param1, "epoch": epoch})
        time.sleep(1)

wandb.agent(sweep_id, function=train)
```

* Quick overview: [Run in colab](https://github.com/wandb/examples/blob/master/examples/wandb-sweeps/sweeps-python/notebook.ipynb)
* Complete walkthrough of using sweeps in a project: [Run in colab](https://colab.research.google.com/drive/181GCGp36_75C2zm7WLxr9U2QjMXXoibt)

### wandb.agent\(\)

{% hint style="danger" %}
L’utilisation de wandb.agent\(\) avec les environnements jupyter notebook peut rencontrer des difficultés lors de l’utilisation GPU.

Il peut y avoir une mauvaise interaction entre wandb.agent\(\) et les environnements jupyter, causée par la manière dont les ressources GPU/CUDA sont initialisées par les frameworks.

Une manière temporaire de contourner le problème \(jusqu’à ce qu’on puisse réparer ces interactions\) est d’éviter d’utiliser l’interface python pour exécuter l’agent. À la place, utilisez l’interface de ligne de commande en paramétrant la cle`program dans la configuration de balayage, et exécutez` !wandb agent SWEEP\_ID dans votre notebook.
{% endhint %}

**Arguments**

* **sweep\_id \(dict\):** ID de balayage généré par l’IU, la CLI, ou l’API de balayage
* **entity \(str, optional\):** nom d’utilisateur ou équipe où vous voulez envoyer vos essais
* **project \(str, optional\):** projet où vous voulez envoyer vos essais
* **function \(dir, optional\):**  Configure la fonction de balayage

##  Exécuter un contrôleur local

Si vous voulez développer vos propres algorithmes de recherche de paramètres, vous pouvez exécuter votre contrôleur depuis Python.

La manière la plus simple d’exécuter un contrôleur :

```python
sweep = wandb.controller(sweep_id)
sweep.run()
```

Si vous voulez davantage de contrôle sur la boucle du contrôleur :

```python
import wandb
sweep = wandb.controller(sweep_id)
while not sweep.done():
    sweep.print_status()
    sweep.step()
    time.sleep(5)
```

Ou encore plus de contrôle sur les paramètres servis :

```python
import wandb
sweep = wandb.controller(sweep_id)
while not sweep.done():
    params = sweep.search()
    sweep.schedule(params)
    sweep.print_status()
```

Si vous voulez davantage de contrôle sur la boucle du contrôleur :

```python
import wandb
from wandb.sweeps import GridSearch,RandomSearch,BayesianSearch

sweep = wandb.controller()
sweep.configure_search(GridSearch)
sweep.configure_program('train-dummy.py')
sweep.configure_controller(type="local")
sweep.configure_parameter('param1', value=3)
sweep.create()
sweep.run()
```

Ou encore plus de contrôle sur les paramètres servis :

Si vous souhaitez spécifier votre balayage entièrement avec du code, vous pouvez faire quelque chose comme ce qui suit :

