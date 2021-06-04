---
description: >-
  Foire aux questions sur la configuration des versions hébergées localement de
  notre application
---

# Local FAQ

## **Mon serveur nécessite-t-il une connexion Internet ?**

Non, wandb/local peut fonctionner dans des environnements air gap. Le seul prérequis est que les ordinateurs sur lesquels vous entraînez vos modèles puissent se connecter à ce serveur par le réseau.

## Où sont stockées mes données ?

L’image Docker par défaut exécute MySQL et Minio au sein du conteneur et inscrit toutes les données dans des sous-dossiers de `/vol` . Vous pouvez configurer un moteur de stockage MySQL et un stockage d’objet externes en obtenant une licence. Envoyez-nous un e-mail à l’adresse [contact@wandb.com](mailto:contact@wandb.com) pour plus d’informations.

## **À quel fréquence sortez-vous des mises à jour ?**

 Nous nous efforçons de sortir des versions mises à jour de notre serveur au moins une fois par mois.

##  **Que se passe-t-il si mon serveur tombe en panne ?**

Les expériences en cours de progression entreront dans une boucle backoff de nouvel essai et continuera d’essayer de se connecter pendant 24 heures.

##  **Que se passe-t-il si mon espace de stockage est insuffisant ?**

Assurez-vous de configurer **des stockages d’objets et de métadonnées externes** afin d’éviter les pertes de données. Aucune sauvegarde de données n’est prévue en cas de saturation de l’espace de stockage du disque dur. L’instance va s’arrêter de fonctionner.

## **Quelles sont les caractéristiques d’extensibilité de ce service ?**

Une seule instance de wandb/local sans stockage MySQL externe mettra à l’échelle jusqu’à 10 expériences concomitantes retracées simultanément. Les instances connectées à un stockage MySQL externe mettront à l’échelle des centaines d’essais concomitants. Si vous souhaitez faire un suivi d’un plus grand nombred’expériences concomitantes, contactez-nous au [contact@wandb.com](mailto:contact@wandb.com) pour découvrir nos options d’installations multi-instances à disponibilité élevée.

##  Comment réinitialiser aux paramètres d’usine si je ne peux pas accéder à mon instance ?

Si vous n’arrivez pas à vous connecter à votre instance, vous pouvez la mettre en mode restauration en paramétrant la variable d’environnement LOCAL\_RESTORE lorsque vous lancez Local. Si vous lancez wandb Local en utilisant notre interface en ligne de commande \(CLI\), vous pouvez le faire avec l’argument `wandb local -e LOCAL_RESTORE=true`. Consultez les fichiers journaux affichés au démarrage pour un nom d’utilisateur et un mot de passe temporaires pour accéder à l’instance.

##   **Comment repasser au cloud après avoir utilisé l’appli Local ?**

Pour restaurer un ordinateur de façon à ce qu’il envoie les métriques à notre solution hébergée en cloud,exécutez wandb login --host=[https://api.wandb.ai](https://api.wandb.ai).

**Un serveur wandb doit-il avoir une autorisation de lecture et d’écriture pour accéder au compartiment S3 ?**

Oui, les deux. Le serveur wandb doit être capable de lire dans le compartiment pour générer des URL signées destinées à l’usage des clients, et il doit avoir une autorisation d’écriture pour mettre à jour les fichiers de métadonnées. Dans la mesure où le serveur wandb génère des URL temporaires à l’usage des clients, il n’est pas nécessaire de mettre le compartiment s3 en **mode public** ou d’attribuer explicitement des autorisations aux utilisateurs finaux.

