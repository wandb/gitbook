---
description: Visualize PyTorch Lightning models with W&B
---

# PyTorch Lightning

PyTorch Lightning fournit un wrapper léger pour organiser votre code PyTorch et facilement ajouter des caractéristiques avancées comme [l’entraînement distribué](https://pytorch-lightning.readthedocs.io/en/latest/multi_gpu.html) ou la [précision 16-bit](https://pytorch-lightning.readthedocs.io/en/latest/amp.html). W&B fournit un wrapper léger pour enregistrer vos expériences d’apprentissage automatique. Nous sommes incorporés directement depuis la librairie de PyTorch Lightning, et vous pouvez toujours vous référer à [leur documentation](https://pytorch-lightning.readthedocs.io/en/latest/loggers.html#weights-and-biases). 

## ⚡ Allez à la vitesse de l’éclair en deux lignes à peine :

```python
from pytorch_lightning.loggers import WandbLogger
from pytorch_lightning import Trainer

wandb_logger = WandbLogger()
trainer = Trainer(logger=wandb_logger)
```

## ✅ Consultez de vrais exemples !

Nous avons créé quelques exemples pour que vous puissiez voir comment fonctionne cette intégration :

*  [Démo dans Google Colab](https://colab.research.google.com/drive/16d1uctGaw2y9KhGBlINNTsWpmlXdJwRW?usp=sharing) avec optimisation d’hyperparamètres
* [Tutoriel ](https://colab.research.google.com/github/wandb/examples/blob/master/colabs/pytorch-lightning/Supercharge_your_Training_with_Pytorch_Lightning_%2B_Weights_%26_Biases.ipynb): Boostez votre Entraînement avec Pytorch Lightning + Weights & Biases
* [Segmentation sémantique avec Lightning ](https://app.wandb.ai/borisd13/lightning-kitti/reports/Lightning-Kitti--Vmlldzo3MTcyMw): optimiser les réseaux neuronaux pour les voitures autonomes
*  [Un guide pas-à-pas](https://app.wandb.ai/cayush/pytorchlightning/reports/Use-Pytorch-Lightning-with-Weights-%26-Biases--Vmlldzo2NjQ1Mw) pour retracez les performances de votre modèle Lightning

## **💻** Référence API

### `WandbLogger`

Paramètres optionnels :

* **name** \(_str_\) – affiche le nom pour l’essai.
* **save\_dir** \(_str_\) – chemin où les données sont enregistrées \(par défaut, dossier wandb\).
* **offline** \(_bool_\) – exécuter hors-ligne \(les données peuvent être transmises plus tard aux serveurs wandb\)
* **id** \(_str_\) – fixe la version, surtout utilisé pour reprendre un essai précédent.
* **version** \(_str_\) – même chose que version \(legacy\).
* **anonymous** \(_bool_\) – permet ou empêche explicitement l’enregistrement anonyme.
* **project** \(_str_\) – le nom du projet auquel cet essai se rapportera.
* **log\_model** \(_bool_\) – sauvegarde les checkpoints dans le wandb dir pour les envoyer aux serveurs W&B.
* **prefix** \(_str_\) – chaîne à mettre au début des clefs de mesure.
* **sync\_step** \(_bool_\) - synchronise les étapes Trainer avec les étapes wandb \(par défaut, True\).
* **\*\*kwargs** – des ****arguments additionnels comme `entity`, `group`, `tags`, etc. utilisés par wandb.init peuvent être passés comme arguments mots-clefs dans ce logger. 

### **`WandbLogger.watch`**

Enregistrez la topologie de votre modèle ainsi que des dégradés et des poids optionnels.

```python
wandb_logger.watch(model, log='gradients', log_freq=100)
```

Paramètres :

* **model** \(_nn.Module_\) – modèle à enregistrer.
* **log** \(_str_\) – peut être "gradients" \(dégradés, par défaut\), "parameters" \(paramètres\), "all" \(tout\) ou None \(Aucun\).
* **log\_freq** \(_int_\) – compteur d’étape entre l’enregistrement des dégradés et des paramètres \(par défaut, 100\).

### **`WandbLogger.log_hyperparams`**

Enregistrez la configuration d’hyperparamètres.

Note : cette fonction est automatiquement appelée lorsque _`LightningModule.save_hyperparameters()`_ est utilisée.

```python
wandb_logger.log_hyperparams(params)
```

 Paramètres :

* **params** \(dict\)  – dictionnaire avec les noms d’hyperparamètres en tant que clefs et les valeurs de configuration en tant que valeurs

### `WandbLogger.log_metrics`

Enregistrez les mesures d’entraînement.

_Note : cette fonction est automatiquement appelée par   `LightningModule.log('metric', value)`_

```python
wandb_logger.log_metrics(metrics, step=None)
```

Paramètres :

* **metric** \(numeric\) – dictionnaire avec les noms de mesures en tant que clefs et les quantités mesurées en tant que valeurs
* **step** \(int\|None\) – nombre d’étapes auxquelles les mesures doivent être enregistrées.

\*\*\*\*

