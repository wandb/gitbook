---
description: >-
  Visualisez des données hautement dimensionnelles le long de vos expériences
  d’apprentissage automatique
---

# Parallel Coordinates

Voici un exemple de graphique de coordonnées parallèles. Chaque axe représente quelque chose de différent. Dans ce cas, j’ai choisi quatre axes verticaux. Je visualise la relation entre différents hyperparamètres et la précision finale de mon modèle.

* **Axes : Différents hyperparamètres de** [**wandb.config**](file:////library/config) **et mesures de** [**wandb.log\(\)**](file:////library/log)\*\*\*\*
*  **Lignes : Chaque ligne représente un essai unique. Passez la souris sur une ligne pour voir une info-bulle avec les détails de cet essai. Toutes les lignes qui correspondent à la sélection actuelle de filtres seront montrées, mais si vous désactivez l’œil, les lignes seront grisées.**

 **Paramètres de panneau**

Configurez ces fonctionnalités dans les paramètres de panneau – cliquez sur le bouton éditer en haut à droite du panneau.

* **Info-bulle** \(Tooltip\) : Une légende montre une info-bulle pour chaque essai lorsque vous passez la souris dessus.
*  **Titres** \(Titles\) : Éditez les titres des axes pour qu’ils soient plus lisibles
* **Dégradés** \(Gradients\) : Personnalisez le dégradé pour qu’il soit de la couleur que vous souhaitez
* **Échelle logarithmique** \(Log scale\) : Chaque axe peut être visualisé de manière indépendante sur une échelle logarithmique
* **Retourner les axes** \(Flip axis\) : Changez la direction des axes – c’est utile lorsque vous avez votre précision et vos pertes sous forme de colonnes.[Voir en direct →](https://app.wandb.ai/example-team/sweep-demo/reports/Zoom-in-on-Parallel-Coordinates-Charts--Vmlldzo5MTQ4Nw)

![](../../../.gitbook/assets/2020-04-27-16.11.43.gif)

