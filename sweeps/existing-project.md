---
description: >-
  Si vous utilisez déjà wandb.init, wandb.config et wandb.log dans votre projet,
  commencez à ce stade-ci !
---

# Sweep from an existing project

 Si vous avez déjà un projet W&B existant, il est facile de commencer à optimiser vos modèles avec des balayages d’hyperparamètres. Je vais vous accompagner étape par étape avec un exemple concret – vous pouvez accéder à mon [tableau de bord W&B](https://app.wandb.ai/carey/pytorch-cnn-fashion) ici. J’utilise ce code depuis[ ce répertoire d’exemple](https://github.com/wandb/examples/tree/master/examples/pytorch/pytorch-cnn-fashion)s, qui entraîne un réseau neuronal convolutif PyTorch pour classifier des images à partir [d’un jeu de données Fashion MNIST](https://github.com/zalandoresearch/fashion-mnist).  


## 1.  Créer un projet

Exécutez votre premier essai de comparaison manuellement pour vous assurer que l’enregistrement sur W&B fonctionne correctement. Vous téléchargerez cet exemple simple de modèle, vous l’entraînerez pendantquelques minutes et verrez l’exemple s’afficher sur le tableau de bord web.

* Clonez ce répertoire `git clone https://github.com/wandb/examples.git`
* Ouvrez cet exemple `cd examples/pytorch/pytorch-cnn-fashion`
* Exécutez un essai manuellement `python train.py`

[Voir un exemple de page de projet →](https://app.wandb.ai/carey/pytorch-cnn-fashion)

## 2. Créer un balayage

Depuis la page de votre projet, ouvrez l’onglet Balayage \(Sweep\) dans le panneau latéral et cliquez sur Créer un balayage \(Create Sweep\).

![](../.gitbook/assets/sweep1.png)

La configuration auto-générée devine les valeurs à balayer en se basant sur les essais que vous avez déjà effectués. Éditez la configuration pour spécifier quelles plages d’hyperparamètres vous voulez essayer. Lorsque vous lancez le balayage, cela commence un nouveau processus dans notre serveur de balayage W&B hébergé. Le service centralisé coordonne les agents – c-à-d, vos ordinateurs qui sont en train d’exécuter les tâches d’entraînement.

![](../.gitbook/assets/sweep2.png)

## 3. Lancer des agents

Ensuite, lancez un agent localement \(launch agent\). Vous pouvez lancer des douzaines d’agents simultanément sur différents ordinateurs si vous voulez répartir la charge de travail et finir le balayage plus rapidement. L’agent affichera le set de paramètres qu’il va essayer par la suite.

![](../.gitbook/assets/sweep3.png)

Et voilà ! Vous êtes en train d’effectuer un balayage. Voici à quoi ressemble le tableau de bord lorsque mon exemple de balayage commence. [Voir un exemple de page de projet →](https://app.wandb.ai/carey/pytorch-cnn-fashion)​

![](https://paper-attachments.dropbox.com/s_5D8914551A6C0AABCD5718091305DD3B64FFBA192205DD7B3C90EC93F4002090_1579066494222_image.png)

##  **Ensemencer \(seed\) un nouveau balayage avec des essais existants**

Lancez un nouveau balayage en utilisant des essais existants que vous avez déjà enregistrés.

1. Ouvrez votre tableau de projet.
2. Sélectionnez les essais que vous voulez utiliser sur les cases à cocher sur le côté gauche du tableau.
3. Cliquez sur le menu déroulant pour créer un nouveau balayage \(create sweep\).

 Votre balayage sera alors mis en place sur notre serveur. Tout ce que vous aurez à faire, c’est de lancer un ou plusieurs agents pour commencer l’exécution des essais.

![](../.gitbook/assets/create-sweep-from-table%20%281%29%20%281%29.png)

