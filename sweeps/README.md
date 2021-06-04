---
description: Recherche d’hyperparamètre et optimisation de modèle
---

# Sweeps

Utilisez les **balayages \(sweeps\)** Weights & Biases pour automatiser l’optimisation d’hyperparamètre et explorer l’espace des modèles possibles.

## **Les avantages de l’utilisation des balayages W&B**

1. **Configuration rapide** : avec un codage de seulement quelques lignes, vous pouvez lancer des balayages W&B.
2. **Transparence :** nous citons tous les algorithmes que nous utilisons, [et notre code est en open-source.](https://github.com/wandb/client/tree/master/wandb/sweeps)​
3. **Puissance :** nos balayages sont entièrement personnalisables et configurables. Vous pouvez lancer un balayage sur des douzaines d’ordinateurs aussi simplement que pour un seul lancement sur votre ordinateur portable.

## **Cas d’utilisations fréquents**

1. **Exploration** : échantillonnage efficace de l’espace de vos combinaisons d’hyperparamètres pour découvrir des régions prometteuses et développer une intuition sur votre modèle.
2. **Optimisation :** utilisation des balayages pour trouver un set d’hyperparamètres ayant des performances optimales.
3.  **Validation croisée à k blocs** : voici un [bref exemple de codage](https://github.com/wandb/examples/tree/master/examples/wandb-sweeps/sweeps-cross-validation) de validation croisée à k blocs avec les balayages W&B.

## Approche

1. **Ajoutez wandb** : dans votre script Python, ajoutez quelques lignes de code pour enregistrer des hyperparamètres et des métriques de sortie \(output\) à partir de votre script. [Commencer maintenant →](https://docs.wandb.ai/v/fr/sweeps/quickstart)​
2.  **Écrivez votre configuration** : définissez les variables et les plages à balayer. Choisissez une stratégie de recherche – nous prenons en charge les recherches par grille, aléatoires et bayésiennes, ainsi que les arrêts prématurés. Consultez quelques exemples de configuration [ici](https://github.com/wandb/examples/tree/master/examples/keras/keras-cnn-fashion).
3. **Initialisez le balayage :** lancez le serveur de balayage. Nous hébergeons ce contrôleur central et nous coordonnons les agents qui exécutent le balayage.
4.  **Lancer l’agent/les agents** : exécutez cette commande sur chaque ordinateur que vous voudriez utiliser pour entraîner les modèles dans le balayage. Les agents demandent au serveur central de balayage quels hyperparamètres essayer ensuite, ensuite, ils exécutent tous les essais.
5. **Visualisez les résultats :** ouvrez notre tableau de bord en direct pour voir tous vos résultats en un seul endroit centralisé.

![](../.gitbook/assets/central-sweep-server-3%20%282%29%20%282%29%20%283%29%20%283%29%20%282%29%20%281%29%20%282%29.png)

{% page-ref page="quickstart.md" %}

{% page-ref page="existing-project.md" %}

{% page-ref page="configuration.md" %}

{% page-ref page="local-controller.md" %}

{% page-ref page="python-api.md" %}

{% page-ref page="sweeps-examples.md" %}

