---
description: Recherche d’hyperparamètre et optimisation de modèle
---

# Sweeps

Utilisez les Balayages Weights & Biases pour automatiser l’optimisation d’hyperparamètre et explorer l’espace des modèles possibles.

## Bénéfices d’utiliser les Balayages W&B

1. **Mise en place rapide** : Avec quelques lignes de code à peine, vous pouvez exécuter des balayages W&B.
2. **Transparence :** Nous citons tous les algorithmes que nous utilisons, [et notre code est en open-source.](https://github.com/wandb/client/tree/master/wandb/sweeps)
3. **Puissance :** Nos balayages sont complètement personnalisables et configurables. Vous pouvez lancer un balayage sur des douzaines de machines, et c’est aussi facile que d’en lancer un sur votre ordinateur portable.

## Utilisations fréquentes

1. **Explorer** : Échantillonner efficacement l’espace de vos combinaisons d’hyperparamètres pour découvrir des régions prometteuses et construire une intuition sur votre modèle.
2. **Optimiser :** Utilisez des balayages pour trouver un set d’hyperparamètres avec des performances optimales.
3.  **Validation croisée à k blocs** : Voici un [bref exemple de code](https://github.com/wandb/examples/tree/master/examples/wandb-sweeps/sweeps-cross-validation) de validation croisée à k blocs avec les Balayages W&B. 

## Approche

1. **Ajoutez wandb** : Dans votre script Python, ajoutez quelques lignes de code pour enregistrer des hyperparamètres et de mesures de sortie \(output\) de votre script. [Commencez maintenant →](https://docs.wandb.ai/sweeps/quickstart)
2.  **Écrivez votre config** : Définissez les variables et les plages sur lesquelles balayer. Choisissez une stratégie de recherche – nous prenons en charge les recherches par grille, aléatoires, et bayésiennes, ainsi que les arrêts précoces. Consultez quelques exemples de configs [ici](https://github.com/wandb/examples/tree/master/examples/keras/keras-cnn-fashion).
3. **Initialisez le balayage :** Lancez le serveur de balayage. Nous hébergeons ce contrôleur central et nous coordonnons entre les agents qui exécute le balayage.
4.  **Lancer l’agent/les agents** : Exécutez cette commande sur chaque machine que vous voudriez utiliser pour entraîner les modèles dans le balayage. Les agents demandent au serveur central de balayage quels hyperparamètres utiliser ensuite, puis ils exécutent tous les essais.
5. **Visualisez les résultats :** Ouvrez notre tableau de bord en direct pour voir tous vos résultats en un seul endroit centralisé.

![](../.gitbook/assets/central-sweep-server-3%20%282%29%20%282%29%20%283%29%20%283%29%20%282%29%20%281%29%20%282%29.png)

{% page-ref page="quickstart.md" %}

{% page-ref page="existing-project.md" %}

{% page-ref page="configuration.md" %}

{% page-ref page="local-controller.md" %}

{% page-ref page="python-api.md" %}

{% page-ref page="sweeps-examples.md" %}

