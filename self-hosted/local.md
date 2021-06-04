---
description: Exécutez Weights and Biases sur vos propres machines en utilisant Docker
---

# Local

Utilisez W&B Local pour auto-héberger l’application Weights & Biases. Vous pouvez utiliser l’application localement ou en l’hébergeant dans un cloud privé. Pour les tâches conséquentes, nous vous encourageons à configurer et administrer un système de fichiers extensible. Pour les clients Entreprise, nous fournissons une assistance technique très étendue et de fréquentes mises à jour d’installation pour les instances auto-hébergées.

Le serveur auto-hébergé consiste en une seule image Docker, qui est facile à déployer. Vos données W&B sont sauvegardées sur un volume persistant \(PV\)ou une base de données externe, ainsi vos données pourront être préservées à travers les versions successives du conteneur. Le serveur requiert une instance ayant au minimum 4 cœurs et 8 Go de mémoire.

### Démarrer le serveur

Pour exécuter le serveur W&B localement, vous devez installer [Docker](https://www.docker.com/products/docker-desktop). Ensuite, il vous suffira de lancer :

```text
wandb local
```

En arrière-plan, la bibliothèque client wandb exécute l’image docker [wandb/local](https://hub.docker.com/repository/docker/wandb/local), ce qui fait passer le port 8080 à l’hôte, et configure votre machine pour envoyer des métriques vers votre instance locale plutôt que sur notre cloud hébergé. Si vous souhaitez exécuter notre conteneur local manuellement, vous pouvez lancer la commande docker suivante :

```text
docker run --rm -d -v wandb:/vol -p 8080:8080 --name wandb-local wandb/local
```

###  Hébergement centralisé

Exécuter wandb dans un hôte local est parfait pour les tests initiaux, mais pour bénéficier pleinement des fonctionnalités collaboratives de wandb/local, vous devriez héberger le service sur un serveur central. Les instructions pour mettre en place un serveur centralisé sur différentes plateformes peuvent être trouvées dans la section [Mise en place](https://docs.wandb.ai/v/fr/self-hosted/setup).

**Risques de perte de données**

**Utilisez un système de fichiers extensible** pour toute tâche conséquente. Plus vous enregistrez des données, plus vos besoins de stockage s’accroîtront. Attribuez des espaces par anticipation et configurez des alertes de façon à pouvoir redimensionner votre système de fichiers en fonction de l’accroissement de votre usage.

###  Configuration de base

**Test rapide de configuration**

Exécuter `wandb local` configure votre machine locale pour qu’elle envoie les métriques à l’adresse[http://localhost:8080](http://localhost:8080/). Si vous voulez héberger localement sur un port différent, vous pouvez ajouter l’argument –port dans wandb local.

Si vous avez configuré le DNS avec votre instance locale, vous pouvez exécuter wandb login --host=[http://wandb.myhost.com](http://wandb.myhost.com) sur n’importe quel ordinateur duquel vous souhaitez avoir un rapport de métriques.

Vous pouvez aussi paramétrer la variable d’environnement WANDB\_BASE\_URL sur un hôte ou l’IP de n’importe quel ordinateur duquel vous souhaitez avoir un rapport à votre instance locale.

Dans un environnement automatisé, vous devrez également paramétrer la variable d’environnement WANDB\_API\_KEY dans une clé api depuis votre page de paramètres. Pour restaurer une machine de sorte qu’elle envoie les métriques sur notre solution hébergée sur un cloud, lancez `wandb login --host=https://api.wandb.ai`.

**Configuration évolutive**

Bien que W&B puisse être utilisé en exploitant un volume persistent configuré avec /vol comme mentionné plus haut, cette solution n’est pas conçue pour les grandes charges de travail. Si vous décidez d’utiliser W&B dans ce sens, il est recommandé d’allouer suffisamment d’espace à l’avance pour stocker les besoins actuels et futurs des métriques, et nous conseillons vivement de configurer les fichiers sous-jacents de façon à ce qu’ils puissent être redimensionnés au besoin. Par ailleurs, des alertes doivent être mises en place pour vous informer de l’atteinte des seuils minimaux de stockage afin que vous puissiez redimensionner la taille des fichiers sous-jacents.

### Authentification

L’installation de base de wandb/local commence avec un utilisateur par défaut [local@wandb.com](mailto:local@wandb.com). Le mot de passe par défaut est un **perceptron**. Le frontend essayera de se connecter automatiquement avec cet utilisateur et vous demandera de réinitialiser votre mot de passe. Contactez-nous à l’adressesupport@wandb.com pour demander une montée en gamme gratuite de votre licence, comprenant 1 à 3 utilisateurs.

###  **Persistance et extensibilité**

Toutes les métadonnées et tous les fichiers envoyés à W&B sont stockées dans le répertoire /vol. Si vous ne configurez pas un volume persistant à cet emplacement, toutes les données seront perdues à la fin du processus docker.

Si vous achetez une licence pour wandb/local, vous pouvez stocker des métadonnées dans une base de données externe MySQL et des fichiers dans un compartiment \(bucket\) de stockage externe, ce qui élimine le besoin d’un conteneur avec état et vous donne la résilience nécessaire ainsi que les fonctionnalités de scalabilité typiquement nécessaires pour les charges de travail de production.

Bien que W&B puisse être utilisé en exploitant un volume persistent configuré avec /vol comme mentionné plus haut, cette solution n’est pas conçue pour les grandes charges de travail. Si vous décidez d’utiliser W&B dans ce sens, il est recommandé d’allouer suffisamment d’espace à l’avance pour stocker les besoins actuels et futurs des métriques, et nous conseillons vivement de configurer les fichiers sous-jacents de façon à ce qu’ils puissent être redimensionnés au besoin. Par ailleurs, des alertes doivent être mises en place pour vous informer de l’atteinte des seuils minimaux de stockage afin que vous puissiez redimensionner la taille des fichiers sous-jacents.

Pour les essais proposés, nous recommandons au moins 100 Go d’espace libre dans le volume sous-jacent destiné aux lourdes charges de travail, qui n’incluent pas d’image, ni de vidéo, ni de l’audio. Si vous testez W&B avec de grands fichiers, le stockage sous-jacent doit avoir suffisamment d’espace pour s’accommoder à ces besoins. Dans tous les cas, l’espace alloué doit refléter les métriques et les sorties \(output\) de vos flux de travaux.

### **Mise à jour**

Nous publions régulièrement de nouvelles versions de wandb/local dans dockerhub. Pour vous mettre à jour, vous pouvez exécuter :

```text
$ wandb local --upgrade
```

Pour mettre manuellement votre instance à jour, vous pouvez exécuter :

```text
$ docker pull wandb/local
$ docker stop wandb-local
$ docker run --rm -d -v wandb:/vol -p 8080:8080 --name wandb-local wandb/local
```

###  Obtenir une licence

Si vous êtes intéressés par la configuration pour des équipes, l’utilisation d’un stockage externe, ou le déploiement de wandb/local sur un cluster Kubernetes, envoyez-nous un e-mail à l’adresse [contact@wandb.com](mailto:contact@wandb.com)

