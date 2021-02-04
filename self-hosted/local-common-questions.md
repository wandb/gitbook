---
description: >-
  Foire aux questions sur la mise en place des versions localement hébergées de
  notre app
---

# Local FAQ

## Mon serveur a-t-il besoin d’une connexion à internet ?

Non, wandb/local peut fonctionner dans des environnements air gap. Le seul prérequis est que les machines qui entraînent vos modèles puissent se connecter à ce serveur par le réseau.

## Où sont stockées mes données ?

L’image docker par défaut exécute MySQL et Minio à l’intérieur du conteneur et inscrit toutes les données dans des sous-dossiers de `/vol` . Vous pouvez configurer un MySQL et un Stockage d’Object externe en obtenant une licence. Envoyez un email à [contact@wandb.com](mailto:contact@wandb.com) pour obtenir plus de détails.

## Vous sortez souvent des mises à niveau \(upgrade\) ?

 Nous nous efforçons de sortir des versions mises à niveau de notre serveur au moins une fois par mois.

## Que se passe-t-il si mon serveur s’éteint ?

Les expériences en cours de progression entreront dans une boucle backoff de nouvel essai et continuera d’essayer de se connecter pendant 24 heures.

##  Quelles sont les caractéristiques scalables de ce service ?

Une seule instance de wandb/local sans stockage MySQL externe scalera jusqu’à 10 expériences concomitantes retracées à la fois. Les instances connectées à un stockage MySQL externe scaleront des centaines d’essais concomitants. Si vous avez besoin de garder la trace de plus d’expériences concomitantes, envoyez-nous un message à [contact@wandb.com](mailto:contact@wandb.com) pour vous renseigner sur nos options d’installations à haute disponibilité multi-instances.

##  Comment réinitialiser aux paramètres d’usine si je ne peux pas accéder à mon instance ?

Si vous n’arrivez pas à vous connecter à votre instance, vous pouvez la placer en mode restauration en paramétrant la variable d’environnement LOCAL\_RESTORE lorsque vous lancez local. Si vous lancez wandb local en utilisant notre cli, vous pouvez le faire avec `wandb local -e LOCAL_RESTORE=true`Regardez les logs imprimés au startup pour obtenir un nom d’utilisateur et un mot de passe temporaires pour accéder à l’instance.

##  Comment repasser au cloud après avoir utilisé local ?

Pour restaurer une machine de sorte qu’elle envoie les mesures à notre solution hébergée en cloud, exécutez `wandb log`

