# Common Questions

##  Paramétrer le projet et l’entité

 Lorsque vous exécutez la commande pour commencer un balayage, vous devez fournir à cette commande un projet et une entité. Si vous en voulez d’autres, cela peut être spécifié de 4 manières différentes :

1. Arguments de ligne de commande dans `wandb sweep` \(arguments `--project` et `--entity` \).
2. Fichier de paramètres wandb \(clefs `project` et `entity)`
3.  [Configuration de balayage](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MSYLFRJY9U2Uk-CZtzR/v/francais/sweeps/configuration) \(clefs `project` et `entity)`
4. [Variables d’environnement ](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MSYLFRJY9U2Uk-CZtzR/v/francais/library/environment-variables)\(variables `WANDB_PROJECT` et `WANDB_ENTITY`\)

##  ****Les agents de balayages s’arrêtent après la fin des premiers essais

`wandb: ERROR Error while calling W&B API: anaconda 400 error: {"code":400,"message":"TypeError: bad operand type for unary -: 'NoneType'"}`

Une raison habituelle pour ceci est que la mesure que vous optimisez dans votre fichier de configuration YAML n’est pas une mesure que vous enregistrez. Par exemple, vous pourriez optimiser la mesure **f1**, mais enregistrez **validation\_f1**. Vérifiez bien que vous enregistrez exactement le même nom de mesure que celle que vous optimisez.

##  Paramétrer un nombre fixe d’essais à tenter

Une recherche aléatoire \(random\) continuera à s’exécuter jusqu’à ce que vous arrêtiez le balayage. Vous pouvez paramétrer une cible pour automatiquement arrêter le balayage lorsqu’il obtient une certaine valeur pour une mesure, ou vous pouvez spécifier un nombre fixe d’essai qu’un agent devrait tenter`wandb agent --count NUM SWEEPID`

wandb agent --count NUM SWEEPID

##  Exécuter un balayage sur Slurm

Nous vous recommandons d’exécuter `wandb agent --count 1 SWEEP_ID`qui exécutera une seule tâche d’entraînement puis quittera.

##  Exécuter de nouveau une recherche de grille

Si vous avez épuisé une recherche de grille mais que vous souhaitez de nouveau exécuter certains des essais, vous pouvez supprimer ceux que vous voulez relancer, puis cliquer sur le bouton reprendre \(resume\) sur la page de contrôle de balayage, puis lancer de nouveaux agents pour cet ID de balayage.

## Les balayages et les essais doivent être dans le même projet

`wandb: WARNING Ignoring project='speech-reconstruction-baseline' passed to wandb.init when running a sweep`

Vous ne pouvez pas paramétrer un projet avec wandb.init\(\) lorsque vous exécutez un balayage. Le balayage et les essais doivent être dans le même projet, le projet est donc paramétré par la création du balayage : wandb.sweep\(sweep\_config, project=“fdsfsdfs”\)

## Erreur uploading

Si vous voyez **ERROR Error uploading &lt;file&gt;: CommError, Run does not exist, il est possible que vous paramétriez un ID pour votre essai,** `wandb.init(id="some-string")` . Cet ID doit être unique dans votre projet, et s’il n’est pas unique, cela renverra une erreur. Dans le contexte des balayages, vous ne pouvez pas paramétrer un ID manuel pour vos essais parce que nous générons automatiquement des ID aléatoires et uniques pour ces essais.

Si vous essayez d’avoir un nom agréable qui s’affiche dans le tableau et sur les graphiques, nous vous recommandons d’utiliser **name** plutôt que **id**. Par exemple :

```python
wandb.init(name="a helpful readable run name")
```

##  Balayer avec des commandes personnalisées

Si vous exécutez normalement l’entraînement avec une commande et des arguments, par exemple :

```text
edflow -b <your-training-config> --batch_size 8 --lr 0.0001
```

Vous pouvez convertir ceci en config de balayage comme ceci :

```text
program:
  edflow
command:
  - ${env}
  - python
  - ${program}
  - "-b"
  - your-training-config
  - ${args}
```

La clef ${args} s’étend à tous les paramètres dans le fichier de configuration de balayage, étendus pour qu’ils puissent être parsés par argparse : --param1 value1 --param2 value2

Si vous avez des arguments en plus que vous ne voulez pas spécifier avec argparse, vous pouvez utiliser :  
parser = argparse.ArgumentParser\(\)  
args, unknown = parser.parse\_known\_args\(\)

**Exécuter des Balayages avec Python 3**

 Si vous avez une erreur où le balayage essaye d’utiliser Python 2, il est facile de spécifier qu’il devrait utiliser Python 3 à la place. Ajoutez simplement ceci à votre fichier YAML de configuration de balayage :

```text
program:
  script.py
command:
  - ${env}
  - python3
  - ${program}
  - ${args}
```

\*\*\*\*

## Détails d’optimisation Bayésienne

Le modèle de processus Gaussien qui est utilisé pour l’optimisation Bayésienne est définie dans notre [logique de balayage open-source.](https://github.com/wandb/client/tree/master/wandb/sweeps) Si vous aimeriez avoir plus de possibilités de configuration et plus de contrôle, essayez notre prise en charge de [Ray Tune](https://docs.wandb.com/sweeps/ray-tune).

Nous utilisons une Covariance de Matérn, qui est une généralisation de RBF – définie dans notre code open-source[ici](https://github.com/wandb/client/blob/541d760c5cb8776b1ad5fcf1362d7382811cbc61/wandb/sweeps/bayes_search.py#L30).

## Mettre des balayages en pause vs Arrêter le wandb.agent de balayage

 Y-a-t-il un moyen de faire en sorte que `wandb agent` s’arrête définitivement lorsqu’il n’y a plus de tâches disponibles parce que j’ai mis un balayage en pause ?  


Si vous arrêtez le balayage plutôt que de le mettre en pause, les agents quitteront la tâche. Pour la pause, nous voulons que les agents continuent à s’exécuter, pour que le balayage puisse être repris sans avoir besoin de lancer de nouveau les agents.

##  Manière recommandée de mettre en place les paramètres de config dans un balayage

`wandb.init(config=config_dict_that_could_have_params_set_by_sweep)`  
ou :  
`experiment = wandb.init()    
experiment.config.setdefaults(config_dict_that_could_have_params_set_by_sweep)`

L’avantage de faire ceci, c’est qu’il ignorera le paramétrage de toute clef qui a déjà été paramétrée par le balayage.

## Est-il possible d’ajouter une valeur de catégorie supplémentaire à un balayage, ou dois-je en commencer un nouveau ?

Une fois qu’un balayage a commencé, vous ne pouvez pas modifier la configuration de balayage, mais vous pouvez vous rendre dans n’importe quelle visualisation de tableau et utiliser les cases à cocher pour sélectionner les essais, puis utiliser l’option de menu "créer balayage" \(create sweep\) pour créer un nouveau balayage qui utilise des essais antérieurs.

