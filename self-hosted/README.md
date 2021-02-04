---
description: Installations d’auto-hébergement pour les projets aux données sensibles
---

# Self Hosted

W&B Local est la version auto-hébergée de [Weights & Biases](https://app.wandb.ai/). Cela rend possible le retraçage d’expériences collectives pour les équipes d’apprentissage automatique d’entreprises, en vous donnant une manière de garder toutes vos données d’entraînement et vos métadonnées à l’intérieur du réseau de votre organisation.

 [Demander une démo pour essayer W&B Local →](https://www.wandb.com/demo)

Nous proposons également [W&B Enterprise Cloud](cloud.md), qui fait tourner une structure complètement extensible à l’intérieur du compte AWS ou GCP de votre entreprise. Ce système peut être étendu à n’importe quel niveau d’utilisation.

## Fonctionnalités

* Essais, expériences et rapports illimités
* Garde vos données en sécurité sur le réseau de votre propre entreprise
* S’intègre au système d’authentification de votre entreprise
* Support technique de qualité par l’équipe d’ingénieurs de W&B

Le serveur auto-hébergé est une image Docker unique qui est simple à déployer. Vos données W&B sont sauvegardées sur un volume persistant ou sur une base de données externe pour que les données puissent être préservées au fil des versions containers.

##  Prérequis de serveur

Le serveur auto-hébergé W&B requiert une instance avec au moins 4 cœurs et une mémoire de 8 GB.

## Ressources d’auto-hébergement

{% page-ref page="local.md" %}

{% page-ref page="setup.md" %}

{% page-ref page="configuration.md" %}

{% page-ref page="local-common-questions.md" %}

{% page-ref page="cloud.md" %}

