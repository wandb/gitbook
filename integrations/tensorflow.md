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

Consultez nos [exemples de projets](https://docs.wandb.ai/v/fr/examples) pour avoir un exemple de script complet.

##  **Métriques personnalisés**

Si vous avez besoin d’enregistrer des mesures personnalisées supplémentaires qui ne sont pas enregistrées sur TensorBoard, vous pouvez appeler `wandb.log` dans votre code `wandb.log({"custom": 0.8})`

La configuration de l’argument d’étape wandb.log est désactiver lors de la synchronisation avec TensorBoard. Si vous souhaitez configurer un compte d’étape différent, vous pouvez enregistrer les métriques avec une étape de métrique comme suit :`wandb.log({"custom": 0.8, "global_step"=global_step})`

##  **Hook \(crochet\) TensorFlow**

 Si vous voulez avoir plus de contrôle sur ce qui est enregistré, wandb fournit également un hook \(crochet\) pour les estimateurs TensorFlow. Cela enregistrera toutes vos valeurs `tf.summary` dans le graphique.

```python
import tensorflow as tf
import wandb

wandb.init(config=tf.FLAGS)

estimator.train(hooks=[wandb.tensorflow.WandbHook(steps_per_log=1000)])
```

##  Enregistrement manuel

 La manière la plus simple d’enregistrer des métriques dans TensorFlow est de placer tf.summary dans l’enregistreur \(logger\) TensorFlow :

```python
import wandb

with tf.Session() as sess:
    # ...
    wandb.tensorflow.log(tf.summary.merge_all())
```

 Avec TensorFlow 2, le mode d’entraînement préconisé pour un modèle ayant boucle personnalisée est celui utilisant `tf.GradientTap`e . Vous pouvez en apprendre plus[ ici](https://www.tensorflow.org/tutorials/customization/custom_training_walkthrough). Si vous voulez incorporer wandb pour enregistrer des métriques dans vos boucles d’entraînement personnalisées TensorFlow, vous pouvez suivre ce snippet – 

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

Vous trouverez un exemple complet [ici](https://www.wandb.com/articles/wandb-customizing-training-loops-in-tensorflow-2).

### En quoi W&B est-il différent de TensorBoard ?

Nous aspirons à améliorer les outils de suivi d’expérience pour tout le monde. Lorsque nos cofondateurs ont commencé à travailler sur W&B, ils ont voulu construire un outil pour les utilisateurs frustrés de TensorBoard qui travaillaient chez OpenAI. Voici quelques points sur lesquels nous avons concentré nos efforts d’amélioration :

1. **Reproduire les modèles** : Weights & Biases est efficace pour expérimenter, explorer et reproduire les modèles ultérieurement. Nous enregistrons non seulement les métriques, mais aussi les hyperparamètres et la version du code, et nous pouvons sauvegarder les checkpoints de votre modèle pour vous pour que votre projet soit reproductible.
2.  **Organisation automatique** : si vous passez un projet à un collaborateur ou que vous partez en vacances, W&B facilite la visualisation de tous les modèles que vous avez déjà essayés, ce qui vous évite de passer des heures à réexécuter d’anciennes expériences.
3.  **Intégration rapide et flexible** : ajoutez W&B à votre projet en 5 minutes. Installez notre package Python gratuitement en open-source et ajoutez quelques lignes à votre code, et à chaque fois que vous essaierez votre modèle, vous aurez d’excellents enregistrements de données et de métriques.
4. **Tableau de bord centralisé permanent** : quel que soit l’emplacement où vous souhaitez entraîner vos modèles, que ce soit sur votre ordinateur local, dans la grappe de serveurs \(cluster\) de votre Lab, ou pour des instances ponctuelles dans le cloud, nous vous fournissons le même tableau de bord centralisé. Vous n’avez pas besoin de passer votre temps à copier et à organiser des fichiers TensorBoard depuis différentes machines.
5. **Tableau puissant** : recherchez, filtrez, organisez et regroupez vos résultats depuis différents modèles. Il facilite la visualisation de milliers de versions de modèle et la recherche de ceux qui offrent les meilleures performances dans différentes tâches. TensorBoard n’est pas conçu pour bien fonctionner sur de grands projets.
6. **Des outils dédiés à la collaboration** : utilisez W&B pour organiser des projets complexes d’apprentissage automatique. Il est facile de partager un lien vers W&B, et vous pouvez utiliser la fonction d’équipe privée pour que tout le monde puisse envoyer des résultats sur un projet en commun. Nous soutenons aussi la collaboration via des rapports – ajoutez des visuels interactifs et décrivez votre travail dans un Markdown. C’est une excellente manière de maintenir un journal de bord, partager vos résultats avec votre superviseur, ou de présenter vos résultats à votre Lab.

Commencez en créant un [compte personnel gratuit →](http://app.wandb.ai/)

##  Exemples

Nous avons créé quelques exemples pour vous montrer comment cette intégration fonctionne :

* [Exemple sur Github ](https://github.com/wandb/examples/blob/master/examples/tensorflow/tf-estimator-mnist/mnist.py): exemple MNIST utilisant les estimateurs TensorFlow
* [Exemple sur Github ](https://github.com/wandb/examples/blob/master/examples/tensorflow/tf-cnn-fashion/train.py): exemple Fashion MNIST utilisant Raw TensorFlow
* [Tableau de bord Wandb ](https://app.wandb.ai/l2k2/examples-tf-estimator-mnist/runs/p0ifowcb): visualisez les résultats sur W&B
* Personnaliser des boucles d’entraînement dans TensorFlow 2 – [Article](https://www.wandb.com/articles/wandb-customizing-training-loops-in-tensorflow-2) \| [Colab Notebook](https://colab.research.google.com/drive/1JCpAbjkCFhYMT7LCQ399y35TS3jlMpvM) \|[Tableau de bord](https://app.wandb.ai/sayakpaul/custom_training_loops_tf)

