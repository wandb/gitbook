---
description: >-
  Weights & Biases pour un suivi de vos expériences, le versionnage de jeux de
  données et la gestion de vos modèles
---

# Library

Utilisez la bibliothèque Python `wandb` pour suivre vos expériences d’apprentissage automatique avec quelques lignes de code. Si vous utilisez un framework populaire comme [Pytorch](https://docs.wandb.ai/v/fr/integrations/pytorch) ou [Keras](https://docs.wandb.ai/v/fr/integrations/keras), nous mettons à votre disposition des [intégrations ](https://docs.wandb.ai/v/fr/integrations)simplifiées.

## **Intégrer W&B à votre script**

 Ci-dessous, vous trouverez des blocs de développement simples pour vous permettre de réaliser un suivi d’expérience avec W&B. Nous avons aussi un très grand nombre d’intégrations spécifiques pour [Pytorch](https://docs.wandb.ai/v/fr/integrations/pytorch), ****[Keras](https://docs.wandb.ai/v/fr/integrations/keras), [Scikit](https://docs.wandb.ai/v/fr/integrations/scikit), etc. Voir la rubrique[ **Intégrations**](https://docs.wandb.ai/v/fr/integrations)\*\*\*\*[\[NFR1\]](applewebdata://7B9852B0-959C-4F67-B6E9-3EB41201E5AA#_msocom_1) .

1.  [**wandb.init\(\)**](https://docs.wandb.ai/v/fr/library/init) ****: initialise un nouvel essai au début de votre script. Cela retourne un objet d’exécution \(Run object\) et crée un dossier local où tous vos enregistrements et vos fichiers sont sauvegardés, puis envoyés de manière asynchrone à un serveur W&B. Si vous préférez utiliser un serveur privé plutôt que le serveur cloud que nous hébergeons, nous proposons un système d[’auto-hébergement](https://docs.wandb.ai/v/fr/self-hosted).
2. \*\*\*\*[**wandb.config**](https://docs.wandb.ai/v/fr/library/config) : sauvegarde un dictionnaire d’hyperparamètres, comme le taux d’apprentissage ou le type de modèle. Les paramètres de modèle que vous capturez dans config seront utiles plus tard pour organiser et interroger vos résultats.
3. \*\*\*\*[**wandb.log\(\)**](https://docs.wandb.ai/v/fr/library/log) : enregistre des métriques comme le niveau de précision et les pertes pendant un cycle d’entrainement. Par défaut, quand vous appelez wandb.log\(\), une nouvelle étape est ajoutée à l’historique de l’objet et le sommaire de l’objet est mis à jour.
   * **historique** : Un ensemble d’objets, comme un dictionnaire, qui enregistre les mesures en fonction du temps. Ces séries de valeurs de temps sont montrées sous forme de graphique linéaire par défaut dans l’IU.
   * **synthèse** : ensemble d’objets similaire à un dictionnaire, qui enregistre les métriques au fil du temps. Ces valeurs de séries chronologiques sont affichées sous forme de graphique linéaire par défaut dans l’interface utilisateur.
4. [ Artéfacts ](https://docs.wandb.ai/v/fr/artifacts): enregistre les résultats d’un essai, comme les poids du modèle ou le tableau des prédictions. Ceci vous permet de suivre non seulement l’entraînement du modèle, mais aussi toutes les étapes dupipeline qui affectent votre modèle final.

##  **Les bonnes pratiques**

La bibliothèque `wandb` est incroyablement flexible. Voici quelques lignes directrices pour vous aiguiller.

1. **Config** : garde une trace de l’évolution des hyperparamètres, de l’architecture, du jeu de données, et tout ce que vous souhaitez utiliser pour reproduire votre modèle. Ces valeurs s’afficheront dans des colonnes – utilisez Configurer des colonnes pour regrouper, classifier et filtrer vos essais de manière dynamique dans l’application.
2. **Project** : un projet est un ensemble d’expériences que vous pouvez comparer les unes avec les autres. Chaque projet se voit attribué une page dédiée sur le tableau de bord, et vous pouvez facilement activer ou désactiver les différents groupes d’essais pour comparer les différentes versions du modèle.
3. **Notes** : message rapide de commit à votre attention. Cette note peut être paramétrée depuis votre script et éditée dans le tableau.
4. **Tags** : identifiez vos essais de base et vos essais favoris. Vous pouvez filtrer vos essais en utilisant ces étiquettes, qui peuvent être éditées dans le tableau.

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

Toutes les données enregistrées depuis votre script sont sauvegardées localement sur votre appareil dans un répertoire **wandb**, et ensuite synchronisées avec le cloud W&B ou avec votre [serveur privé](https://docs.wandb.ai/v/fr/self-hosted).

### Enregistrées automatiquement

* **Métriques du système** : utilisation CPU et GPU, réseau, etc. Ces mesures proviennent de [nvidia-smi](https://developer.nvidia.com/nvidia-system-management-interface) et sont montrées dans l’onglet Système, sur la page d’exécution \(Run page\).
* **Ligne de commande** : Les stdout et stderr sont relevés et sont affichés dans l’onglet Enregistrement sur la page d’exécution \(Run page\).

  Activez la [Sauvegarde de code](http://wandb.me/code-save-colab) sur la [page de Paramètres](https://wandb.ai/settings) de votre compte pour accéder aux :

* **Git commit** : relève le dernier git commit et l’affiche sur l’onglet Aperçu de la page d’exécution \(Run page\), ainsi qu’un fichier diff.patch s’il y a des changements qui n’ont pas été effectués.
* **Fichiers** : le fichier `requirements.txt` \(prérequis\) et tout fichier que vous sauvegardez dans le fichier **wandb** pour votre essai seront téléchargés et affichés dans l’onglet Fichiers de la page d’exécution \(Run page\).

###  **Enregistrées avec des requêtes spécifiques**

Concernant les métriques de données et de modèles, vous pouvez décider exactement ce que vous voulez enregistrer.

* **Jeu de données** : vous devez enregistrer des images spécifiques ou d’autres échantillons de jeu de données pour qu’ils soient envoyés à W&B.
* **Gradients PyTorch** : ajoutez `wandb.watch(model)` pour visualiser les gradients des poids sous forme d’histogrammes dans l’IU.
* **Config** :  Enregistrez les hyperparamètres, un lien vers votre dataset, ou le nom de l’architecture que vous utilisez en tant que paramètres de config, sous cette forme : `wandb.init(config=your_config_dictionary)`
* **Métriques** : utilisez `wandb.log()` pour visualiser les métriques de votre modèle. Si vous enregistrez des métriques comme le niveau de précision et les pertes dans votre cycle d’entraînement, vous pourrez voir l’évolution en temps réel de vos graphiques depuis l’interface utilisateur.

