---
description: >-
  Exécuter des algorithmes de recherche et d’arrêt localement, plutôt que
  d’utiliser notre service hébergé sur cloud
---

# Local Controller

Par défaut, le contrôleur d’hyperparamètres est hébergé sur W&B comme service cloud. Les agents de W&B communiquent avec le contrôleur pour déterminer le prochain set de paramètres à utiliser pour l’entraînement. Ce contrôleur est aussi responsable de l’exécution des algorithmes d’arrêts précoces pour déterminer quels sont les essais qui peuvent être arrêtés.La fonctionnalité de contrôleur local permet à l’utilisateur d’exécuter des algorithmes de recherche et d’arrêt localement. 

Le contrôleur local donne à l’utilisateur la capacité d’inspecter et d’instrumenter son code de manière à déboguer les problèmes ainsi qu’à développer de nouvelles fonctionnalités qui peuvent être incorporées dans le service cloud.

{% hint style="info" %}
Le contrôleur local est pour l’instant limité à l’exécution d’un seul agent.
{% endhint %}

##  Configuration de contrôleur local

Pour activer le contrôleur local, ajoutez ce qui suit à votre fichier de configuration de balayage :

```text
controller:
  type: local
```

##  Exécuter le contrôleur local

La commande qui suit lancera le contrôleur de balayage :

```text
wandb controller SWEEP_ID
```

Vous pouvez également lancer le contrôleur lorsque vous initialisez le balayage :

```text
wandb sweep --controller sweep.yaml
```

