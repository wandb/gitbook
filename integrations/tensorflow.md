---
description: How to integrate a TensorFlow script to log metrics to W&B
---

# TensorFlow

Si vous utilisez déjà TensorBoard, c’est facile de l’intégrer avec wandb.

```python
import tensorflow as tf
import wandb
wandb.init(config=tf.flags.FLAGS, sync_tensorboard=True)
```

Regardez nos [exemples de projets](https://docs.wandb.ai/examples) pour avoir un exemple de script complet.

##  Mesures personnalisées

Si vous avez besoin d’enregistrer des mesures personnalisées supplémentaires qui ne sont pas enregistrées sur TensorBoard, vous pouvez appeler `wandb.log` dans votre code au même argument d’étape que celui que TensorBoard utilise : ie.`wandb.log({"custom": 0.8}, step=global_step)`

##  Crochet TensorFlow

 Si vous voulez avoir plus de contrôle sur ce qui est enregistré, wandb fournit également un crochet \(hook\) pour les estimateurs TensorFlow. Cela enregistrera toutes vos valeurs `tf.summary` dans le graphique.

```python
import tensorflow as tf
import wandb

wandb.init(config=tf.FLAGS)

estimator.train(hooks=[wandb.tensorflow.WandbHook(steps_per_log=1000)])
```

##  Enregistrement manuel

 La manière la plus simple d’enregistrer des mesures dans TensorFlow est de placer `tf.summary` dans le logger TensorFlow :

```python
import wandb

with tf.Session() as sess:
    # ...
    wandb.tensorflow.log(tf.summary.merge_all())
```

Avec TensorFlow 2, la manière recommandée d’entraîner un modèle avec une boucle personnalisée est d’utiliser `tf.GradientTape` . Vous pouvez en apprendre plus[ ici](https://www.tensorflow.org/tutorials/customization/custom_training_walkthrough). Si vous voulez incorporer `wandb` pour enregistrer des mesures dans vos boucles d’entraînement personnalisées TensorFlow, vous pouvez suivre cet extrait – 

```python
    with tf.GradientTape() as tape:
        # Get the probabilities
        predictions = model(features)
        # Calculate the loss
        loss = loss_func(labels, predictions)

    # Log your metrics
    wandb.log("loss": loss.numpy())
    # Get the gradients
    gradients = tape.gradient(loss, model.trainable_variables)
    # Update the weights
    optimizer.apply_gradients(zip(gradients, model.trainable_variables))
```

Un exemple complet est disponible [ici](https://www.wandb.com/articles/wandb-customizing-training-loops-in-tensorflow-2).

### En quoi W&B est-il différent de TensorBoard ?

Nous avons voulu améliorer les outils de traçage d’expérience pour tout le monde. Lorsque nos cofondateurs ont commencé à travailler sur W&B, ils ont voulu construire un outil pour les utilisateurs frustrés de TensorBoard qui travaillaient à OpenAI. Voici quelques points sur lesquels nous avons concentré nos efforts d’amélioration :

1. **Reproduire les modèles** : Weights & Biases est efficace pour expérimenter, explorer, et reproduire les modèles plus tard. Nous enregistrons non seulement les mesures, mais aussi les hyperparamètres et la version du code, et nous pouvons sauvegarder les checkpoints de votre modèle pour vous pour que votre projet soit reproductible.
2.  **Organisation automatique** : Si vous passez un projet à un collaborateur ou que vous partez en vacances, W&B rend facile la visualisation de tous les modèles que vous avez déjà essayés, pour que vous ne passiez pas des heures à remodéliser d’anciennes expériences.
3.  **Intégration rapide et flexible** : Ajoutez W&B à votre projet en 5 minutes. Installez notre package Python gratuit et open-source et ajoutez quelques lignes à votre code, et à chaque fois que vous essaierez votre modèle, vous aurez de magnifiques enregistrements de données et de mesures.
4. **Tableau de bord centralisé persistant** : Où que vous souhaitiez entraîner vos modèles, que ce soit sur votre machine locale, dans votre laboratoire, ou pour des exemples ponctuels dans le cloud, nous vous offrons le même tableau de bord centralisé. Vous n’avez pas besoin de passer votre temps à copier et à organiser des fichiers TensorBoard depuis différentes machines.
5. **Tableau puissant** : Recherchez, filtrez, organisez, et regroupez vos résultats depuis différents modèles. Il est facile de visualiser des milliers de versions de modèle et de trouver ceux qui offrent les meilleures performances dans différentes tâches. TensorBoard n’est pas construit pour bien fonctionner sur de grands projets.
6. **Des outils pour la collaboration** : Utilisez W&B pour organiser des projets complexes d’apprentissage automatique. Il est facile de partager un lien vers W&B, et vous pouvez utiliser des équipes privées pour que tout le monde envoie des résultats sur un projet en commun. Nous soutenons aussi la collaboration par les rapports – ajoutez des visuels interactifs et décrivez votre travail dans un Markdown. C’est une manière excellente de garder un journal de travail, de partager vos découvertes avec votre superviseur, ou de présenter vos découvertes à votre laboratoire.

Commencez en créant un [compte personnel gratuit →](http://app.wandb.ai/)

##  Exemples

Nous avons créé quelques exemples pour que vous puissiez voir comment l’intégration fonctionne :

* [Exemple sur Github ](https://github.com/wandb/examples/blob/master/examples/tensorflow/tf-estimator-mnist/mnist.py): Exemple MNIST utilisant les estimateurs TensorFlow
* [Exemple sur Github ](https://github.com/wandb/examples/blob/master/examples/tensorflow/tf-cnn-fashion/train.py): Exemple Fashion MNIST utilisant Raw TensorFlow
* [Tableau de bord Wandb ](https://app.wandb.ai/l2k2/examples-tf-estimator-mnist/runs/p0ifowcb): Visualisez les résultats sur W&B
* Personnaliser des boucles d’entraînement dans TensorFlow 2 – [Article](https://www.wandb.com/articles/wandb-customizing-training-loops-in-tensorflow-2) \| [Colab Notebook](https://colab.research.google.com/drive/1JCpAbjkCFhYMT7LCQ399y35TS3jlMpvM) \|[Tableau de bord](https://app.wandb.ai/sayakpaul/custom_training_loops_tf)

