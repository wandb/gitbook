---
description: >-
  Si vous utilisez déjà wandb.init, wandb.config, et wandb.log dans votre
  projet, commencez ici !
---

# Sweep from an existing project

Si vous avez déjà un projet W&B existant, il est facile de commencer à optimiser vos modèles avec des balayages d’hyperparamètres. Je vais vous accompagner étape par étape avec un exemple concret – vous pouvez ouvrir mon [Tableau de Bord W&B](https://app.wandb.ai/carey/pytorch-cnn-fashion). J’utilise ce code depuis[ ce répertoire d’exemple](https://github.com/wandb/examples/tree/master/examples/pytorch/pytorch-cnn-fashion), qui entraîne un réseau neuronal convolutif PyTorch pour classifier des images du

## 1.  Créer un projet

Exécutez votre premier essai de comparaison manuellement pour vous assurer que l’enregistrement W&B fonctionne correctement. Vous téléchargerez cet exemple simple de modèle, l’entraînerez pour quelques minutes, et verrez l’exemple apparaître sur le tableau de bord web.

* Clonez ce répertoire `git clone https://github.com/wandb/examples.git`
* Ouvrez cet exemple `cd examples/pytorch/pytorch-cnn-fashion`
* Exécutez un essai manuellement `python train.py`

[Voir un exemple de page de projet →](https://app.wandb.ai/carey/pytorch-cnn-fashion)

## 2. Créer un balayage

Depuis votre page de projet, ouvrez l’onglet Balayage \(Sweep\) dans la barre latérale et cliquez sur "Créer Balayage" \(Create Sweep\).

![](../.gitbook/assets/sweep1.png)

La config auto-générée devine les valeurs à balayer en se basant sur les essais que vous avez déjà effectués. Éditez la config pour spécifier quelles plages d’hyperparamètres vous voulez essayer. Lorsque vous lancez le balayage, cela commence un nouveau processus dans notre serveur de balayage W&B hébergé. Le service centralisé coordonne les agents – vos machines qui sont en train d’exécuter les tâches d’entraînement.

![](../.gitbook/assets/sweep2.png)

## 3. Lancer des agents

Puis, lancez un agent de manière locale \(launch agent\). Vous pouvez lancer des douzaines d’agents sur des machines différentes en parallèle si vous voulez distribuer le travail et finir le balayage plus rapidement. L’agent imprimera le set de paramètres qu’il essaye ensuite.

![](../.gitbook/assets/sweep3.png)

 C’est tout ! Vous êtes en train d’effectuer un balayage. Voici ce à quoi ressemble le tableau de bord lorsque mon exemple de balayage commence. [Voir un exemple de page de projet →](https://app.wandb.ai/carey/pytorch-cnn-fashion)

![](https://paper-attachments.dropbox.com/s_5D8914551A6C0AABCD5718091305DD3B64FFBA192205DD7B3C90EC93F4002090_1579066494222_image.png)

##  Seeder un nouveau balayage avec des essais existants

Lancez un nouveau balayage en utilisant des essais existants que vous avez déjà enregistrés.

1. Ouvrez votre tableau de projet.
2. Sélectionnez les essais que vous voulez utiliser avec les cases à cocher sur le côté gauche du tableau.
3. Cliquez sur le menu déroulant pour créer un nouveau balayage \(create sweep\).

 Votre balayage sera alors mis en place sur notre serveur. Tout ce qu’il vous faut faire, c’est de lancer un ou plusieurs agents pour commencer à exécuter des essais.

![](../.gitbook/assets/create-sweep-from-table%20%281%29%20%281%29.png)

