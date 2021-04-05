---
description: Ressources pour les personnes qui commencent l’Apprentissage Automatique
---

# Beginner FAQ

## Vous apprenez l’apprentissage automatique ?

Wandb est un outil pour visualiser l’entraînement, et nous espérons qu’il est utile pour tout le monde, experts et débutants inclus. Si vous avez des questions générales sur l’apprentissage automatique, vous êtes bienvenu sur notre Channel [Slack](http://wandb.me/slack) pour les poser. Nous avons aussi préparé quelques [tutoriels vidéo](https://www.wandb.com/tutorials) gratuits avec du code d’exemple, faits pour vous permettre de bien démarrer.

Une très bonne manière d’apprendre l’apprentissage automatique est de commencer avec un projet intéressant. Si vous n’avez pas de projet à l’esprit, un bon endroit pour trouver des projets est sur notre page de [Benchmarks](https://www.wandb.com/benchmarks) – nous avons une grande variété de tâches d’apprentissage automatique avec des données et du code fonctionnel que vous pouvez améliorer.

## Ressources en ligne

Il existe bon nombre de ressources en lignes excellentes pour apprendre l’apprentissage automatique. Merci de nous envoyer un message si nous devrions ajouter quelque chose ici.

* [fast.ai ](https://www.fast.ai)- Cours pratiques d’apprentissage automatique excellents avec une communauté amicale.
* [deep learning book](http://www.deeplearningbook.org) - Un livre détaillé, disponible gratuitement en ligne.
* [Stanford CS229](https://see.stanford.edu/Course/CS229) - Leçons d’un très bon cours, disponibles en ligne.

##  Rechercher un biais dans des modèles

Si vous entraînez un modèle d’apprentissage automatique, vous voudrez être capable de visualiser quelles sont ses performances avec des entrées \(inputs\) différentes. Un problème commun, surtout lorsqu’on vient de commencer, c’est qu’il est compliqué de réussir à mettre en place ces visuels. C’est là qu’intervient Weights & Biases. Nous rendons facile l’obtention de mesures pour comprendre les performances de votre modèle.

 Voici un exemple hypothétique – vous entraînez un modèle à identifier des objets sur la route. Votre dataset est un ensemble d’images libellées avec des voitures, des piétons, des vélos, des arbres, des immeubles, etc. Pendant que vous entraînez votre modèle, vous pouvez visualiser les différentes précisions de classe. Cela signifie que vous pouvez voir si votre modèle est bon pour trouver des voitures mais mauvais pour trouver des piétons. Ceci peut être un biais dangereux, surtout sur un modèle de voiture autonome.

Vous seriez intéressé de voir un exemple en direct ? Voici un rapport qui compare la précision du modèle sur l’identification d’images de différents types de plantes et d’animaux – oiseaux, mammifères, champignons etc. Les graphiques Weights & Biases permettent de facilement voir comment chaque version du modèle \(chaque ligne sur le graphique\) se débrouille sur des classes différentes.

 [Voir le rapport dans W&B →](https://app.wandb.ai/stacey/curr_learn/reports/Species-Identification--VmlldzoxMDk3Nw)

![](../.gitbook/assets/image%20%2818%29%20%283%29%20%283%29.png)

