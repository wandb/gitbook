---
description: >-
  Exécuter localement des algorithmes de recherche et d’arrêt, au lieu
  d’utiliser notre service hébergé sur cloud
---

# Local Controller

Par défaut, le contrôleur d’hyperparamètres est hébergé sur W&B comme service cloud. Les agents de W&B communiquent avec le contrôleur pour déterminer le prochain set de paramètres à utiliser pour l’entraînement. Ce contrôleur est également responsable de l’exécution des algorithmes d’arrêts prématurés pour déterminer quels sont les essais qui peuvent être arrêtés.

Le contrôleur local donne à l’utilisateur la capacité d’inspecter et d’instrumenter son code de manière à déboguer les problèmes ainsi qu’à développer de nouvelles fonctionnalités qui peuvent être incorporées dans le service cloud.

{% hint style="info" %}
Le contrôleur local donne à l’utilisateur la capacité d’inspecter et d’instrumenter son code dans le but dedéboguer les problèmes et de développer de nouvelles fonctionnalités qui peuvent être incorporées dans le service cloud.
{% endhint %}

Le contrôleur local est pour l’instant limité à l’exécution d’un seul agent.

## **Configuration d’un contrôleur local**

Pour activer le contrôleur local, ajoutez le code suivant à votre fichier de configuration de balayage :

```text
controller:
  type: local
```

##  **Exécuter un contrôleur local**

 La commande suivante lancera un contrôleur de balayage :

```text
wandb controller SWEEP_ID
```

Vous pouvez également lancer un contrôleur lorsque vous initialisez le balayage :

```text
wandb sweep --controller sweep.yaml
```

