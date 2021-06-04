# Jupyter

Utilisez Weights & Biases dans vos notebooks Jupyter pour obtenir des visualisations interactifs et réaliser des analyses personnalisées sur vos essais d’entraînement.

##  ****Cas d’utilisation pour W&B avec les notebooks Jupyter

1. **Expérimentations itératives :** exécuter et réexécuter des expériences, ajuster les paramètres, faire enregistrer tous vos essais automatiquement sur W&B sans devoir prendre des notes manuellement tout au long du processus.
2. **Sauvegarde de code :** lorsque vous reproduisez un modèle, il est compliqué de savoir quelles cellules d’un notebook ont été exécutées, et dans quel ordre. Activez la sauvegarde de code sur votre [page de paramètres](https://wandb.ai/settings) pour sauvegarder un enregistrement de l’exécution des cellules pour chaque expérience.
3.  **Analyse personnalisée :** une fois que vos essais sont enregistrés sur W&B, il est facile d’obtenir une DataFrame à partir de l’API et de faire une analyse personnalisée, puis d’enregistrer ces résultats sur W&B pour les sauvegarder et les partager via des rapports.

## **Configuration des notebooks**

Commencez votre notebook avec le code suivant pour installer W&B et lier votre compte :

```python
!pip install wandb -qqq
import wandb
wandb.login()
```

Ensuite, configurez votre expérience et sauvegardez les hyperparamètres :

```python
wandb.init(project="jupyter-projo",
           config={
               "batch_size": 128,
               "learning_rate": 0.01,
               "dataset": "CIFAR-100",
           })
```

Après avoir exécuté `wandb.init()` , lancez une nouvelle cellule avec `%%wandb` pour voir des graphiques en direct dans le notebook. Si vous exécutez cette cellule plusieurs fois, les données seront annexées à l’essai.

```python
%%wandb

# Your training loop here
```

Essayez-le par vous-même dans cet [exemple de script rapide →](https://bit.ly/wandb-jupyter-widgets-colab)​

![](../.gitbook/assets/jupyter-widget.png)

 En tant qu’alternative au décorateur `%%wandb` , après avoir exécuté `wandb.init()`, vous pouvez finir n’importe quelle cellule avec wandb.run pour montrer des graphiques en ligne.

```python
# Initialize wandb.run first
wandb.init()

# If cell outputs wandb.run, you'll see live graphs
wandb.run
```

## Fonctionnalités additionnelles de Jupyter dans W&B

1. **Colab**:  Lorsque vous appelez `wandb.init()` pour la première fois dans un Colab, nous authentifions automatiquement votre runtime si vous êtes connecté à W&B dans votre navigateur à cet instant. Sur l’onglet d’aperçu de votre page d’essai, vous verrez un lien vers le Colab. Si vous activez la sauvegarde de code dans les [paramètres](https://app.wandb.ai/settings), vous pourrez aussi voir les cellules qui ont été exécutées pour l’essai de votre expérience, ce qui permet une meilleure reproductibilité.
2.  **Lancer Docker Jupyter :** Appelez `wandb docker –jupyter` pour lancer un conteneur docker, monter votre code dedans, vous assurer que Jupyter est installé, et lancer sur le port 8888.
3. **run.finish\(\)**: Par défaut, nous attendons jusqu’à ce que wandb.init\(\) soit de nouveau appelée pour marquer un essai comme fini. Cela vous permet d’exécuter des cellules de manière individuelle et de toutes les enregistrer sous le même essai. Pour manuellement marquer qu’un essai est achevé, utilisez la fonctionnalité **run.finish\(\)**.

```python
import wandb
run = wandb.init()
# Training script and logging goes here
run.finish()
```

### Mettre les messages d’info W&B en silencieux

Pour désactiver les messages d’information, exécutez le code suivant dans une cellule de notebook :

```python
import logging
logger = logging.getLogger("wandb")
logger.setLevel(logging.ERROR)
```

## Questions fréquentes

### Nom de notebook

Si vous voyez le message d’erreur "Failed to query for notebook name, you can set it manually with the WANDB\_NOTEBOOK\_NAME environment variable" \("Impossible d’obtenir le nom du notebook, vous pouvez le paramétrer manuellement avec la variable d’environnement WANDB\_NOTEBOOK\_NAME"\), vous pouvez résoudre cette erreur en paramétrant la variable d’environnement depuis votre script comme ceci:`os.environ['WANDB_NOTEBOOK_NAME'] = 'some text here'`

