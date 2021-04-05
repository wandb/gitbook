---
description: >-
  Weights & Biases permet un suivi de vos expériences, d’avoir de multiples
  versions de dataset, et de gérer votre modèle
---

# Library

Utilisez la librairie Python `wandb` pour suivre vos expériences d’apprentissage automatique avec quelques lignes de code. Si vous utilisez un framework populaire comme [Pytorch](https://docs.wandb.ai/integrations/pytorch) ou [Keras](https://docs.wandb.ai/integrations/keras), nous avons des [intégrations](https://docs.wandb.ai/integrations) simplifiées.

## Intégrer W&B à votre script

Ci-dessous, vous trouverez des blocs de construction simples pour vous permettre de suivre une expérience avec W&B. Nous avons aussi un très grand nombre d’intégrations spéciales pour [Pytorch](https://docs.wandb.ai/integrations/pytorch), [Keras](https://docs.wandb.ai/integrations/keras), [Scikit](https://docs.wandb.ai/integrations/scikit), etc. Voir nos [**Intégrations**](https://docs.wandb.ai/integrations).

1.  [**wandb.init\(\)** ](https://docs.wandb.ai/library/init): Initialise un nouvel essai au début de votre script. Cela retourne un objet Run et crée un dossier local où tous vos enregistrements et vos fichiers sont sauvegardés, puis envoyés de manière asynchrone à un serveur W&B. Si vous voulez utiliser un serveur privé plutôt que notre serveur cloud dédié, nous proposons un [auto-hébergement](https://docs.wandb.ai/self-hosted).
2. [**wandb.config** ](https://docs.wandb.ai/library/config): Sauvegarde un dictionnaire d’hyperparamètres, comme le taux d’apprentissage ou le type de modèle. Les paramètres du modèle que vous capturez en config sont utiles plus tard, pour organiser et faire des recherches dans vos résultats.
3. [**wandb.log\(\)** ](https://docs.wandb.ai/library/log): Enregistre des mesures pendant un loop d’entrainement, comme la précision et les pertes. Par défaut, quand vous appelez wandb.log\(\), une nouvelle étape est ajoutée à l’historique de l’objet et le sommaire de l’objet est mis à jour.
   * **historique** : Un ensemble d’objets, comme un dictionnaire, qui enregistre les mesures en fonction du temps. Ces séries de valeurs de temps sont montrées sous forme de graphique linéaire par défaut dans l’IU.
   * **sommaire** : Par défaut, la valeur finale d’une mesure enregistrée avec wandb.log\(\). Vous pouvez régler le sommaire d’une mesure manuellement pour capturer la plus grande précision ou la moins grande perte, à la place de la valeur finale. Ces valeurs sont utilisées dans le tableau, et sur des graphiques linéaires qui comparent les essais – par exemple, vous pouvez visualiser la précision finale de tous vos essais pour votre projet.
4.  [Artéfacts](https://docs.wandb.ai/artifacts) : Enregistre les résultats d’un essai, comme les poids du modèle ou le tableau des prédictions. Ceci vous permet d’observer non seulement le modèle en entraînement, mais aussi les étapes internes en pipeline qui affectent votre modèle final.

##  Meilleures utilisations

La librairie `wandb` est incroyablement flexible. Voici quelques lignes directrices pour vous aiguiller.

1. **Config**: Garde une trace de l’évolution des hyperparamètres, de l’architecture, du dataset, et tout ce que vous souhaitez utiliser pour reproduire votre modèle. Ces valeurs s’afficheront dans des colonnes – utilisez Configurer Colonnes pour regrouper, classifier et filtrer vos essais de manière dynamique dans l’application.
2. **Project**: Un projet est un ensemble d’expériences que vous pouvez comparer les unes avec les autres. Chaque projet obtient une page dédiée sur le tableau de bord, et vous pouvez facilement activer ou désactiver les différents groupes d’essai pour comparer les différentes versions de modèle.
3. **Notes**: Un rapide message commit à votre attention. Cette note peut être paramétrée depuis votre script et est éditable dans le tableau.
4. **Tags**: Identifiez vos essais de base et vos essais favoris. Vous pouvez filtrer en utilisant ces étiquettes, et elles sont éditables dans le tableau.

```python
import wandb

config = dict (
  learning_rate = 0.01,
  momentum = 0.2,
  architecture = "CNN",
  dataset_id = "peds-0192",
  infra = "AWS",
)

wandb.init(
  project="detect-pedestrians",
  notes="tweak baseline",
  tags=["baseline", "paper1"],
  config=config,
)
```

##  Quelles sont les données enregistrées ?

 Toutes les données de votre script sont enregistrées localement sur votre appareil dans un dossier **wandb**, puis synchronisées avec le cloud W&B ou avec votre [serveur privé](https://docs.wandb.ai/self-hosted).

### Enregistrées automatiquement

* **Mesures du système** : Utilisation CPU et GPU, réseau, etc. Ces mesures viennent de [nvidia-smi](https://developer.nvidia.com/nvidia-system-management-interface) et sont montrées dans l’onglet Système, sur la page run.
* **Ligne de commande** : Les stdout et stderr sont relevés et sont affichés dans l’onglet Enregistrement sur la page run.

 Activer la [Sauvegarde de Code](http://wandb.me/code-save-colab) sur votre [page de Paramètres](https://wandb.ai/settings) pour obtenir :

* **Git commit** : Relève le dernier git commit et l’affiche sur l’onglet Vision d’ensemble sur la page run, ainsi qu’un fichier diff.patch s’il y a des changements qui n’ont pas été effectués.
* **Fichiers** : Le fichier `requirements.txt` \(prérequis\) et tout fichier que vous sauvegardez dans le fichier **wandb**pour votre essai seront téléchargés et seront affichés dans l’onglet Fichiers sur la page run.

###  Enregistrées avec demandes spécifiques

Lorsqu’il est question des mesures de données et de modèles, vous pouvez décider exactement ce que vous voulez enregistrer.

* **Dataset**: Vous devez enregistrer des images spécifiques ou d’autres échantillons de dataset pour qu’ils soient envoyés à W&B.
* **PyTorch gradients**: Ajoutez `wandb.watch(model)` pour visualiser les gradients des poids sous forme d’histogrammes dans l’IU.
* **Config**:  Enregistrez les hyperparamètres, un lien vers votre dataset, ou le nom de l’architecture que vous utilisez en tant que paramètres de config, sous cette forme : `wandb.init(config=your_config_dictionary)`
* **Metrics**:  Utilisez `wandb.log()` pour visualiser les mesures de votre modèle. Si vous enregistrez des mesures comme la précision et les pertes depuis l’intérieur de votre boucle d’entraînement, vous pourrez voir l’évolution de vos graphiques en direct depuis l’IU.

