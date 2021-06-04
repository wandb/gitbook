---
description: >-
  La solution Enterprise pour les cloud privés ou les hébergements locaux
  (on-premise hosting) de Weights & Biases
---

# Self Hosted

Utilisez la version **Local** pour héberger l’application W&B sur vos propres serveurs, que ce soit en cloud privé ou en hébergement on-premise. Pour tester rapidement **Local**, vous pouvez même tester l’application sur votre ordinateur portable. **Local** vous donne la possibilité de réaliser un suivi et des visualisations sur W&B sans avoir à envoyer les données aux serveurs W&B.

Nous proposons également [W&B Enterprise Cloud](https://docs.wandb.ai/v/francais/self-hosted/cloud), qui exécute une infrastructure entièrement extensible au sein du compte AWS ou GCP de votre entreprise. Ce système est un bon choix pour le suivi d’une expériencemassivement extensible.

## Fonctionnalités

* Essais, expériences et rapports illimités
* Garde vos données en sécurité sur le réseau de votre propre entreprise
* S’intègre au système d’authentification de votre entreprise
* Assistance technique Premium de l’équipe d’ingénieurs de W&B

Le serveur auto-hébergé est une image Docker unique qui est simple à déployer. Vos données W&B sont sauvegardées sur un volume persistant ou sur une base de données externe pour que les données puissent être préservées au fil des versions containers.

##  **Guide de démarrage rapide**

**1. Exécuter tout localement avec un Docker W&B**

Sur n’importe quelle machine disposant de [Docker](https://www.docker.com/) et [Python](https://www.python.org/), lancez ****`wandb local` our extraire la toute dernière version de [notre image Docker image sur Docker Hub](https://hub.docker.com/r/wandb/local) et exécutez le conteneur sur votre système. La commande complète, dans le cas où vous voudriez modifier le port ou d’autre détails, est  `docker run --rm -d -v wandb:/vol -p 8080:8080 --name wandb-local wandb/local`

Cela lance une instance locale de wandb sur votre machine : tous les journaux et les métriques de vos entraînements seront stockés dans /vol et visibles au [http://localhost:8080](http://localhost:8080/) de votre machine.

Une fois que votre conteneur est lancé, vous pourrez créer une nouveau compte utilisateur et il vous sera demandé de vous connecter sur wandb en allant chercher la clé API de ce nouveau compte via ce lien [http://localhost:8080/authorize](http://localhost:8080/authorize). Pour commencer à vous connecter localement, [installez le package Python de wandb](https://github.com/wandb/client) \(ex : à partir de la ligne de commande pip install wandb\). Ensuite, lancez n’importe quel script comme l’exemple simpliste ci-dessous \(or [l’un de ces scripts](https://github.com/wandb/examples)\).

`import wandb`

`# 1. Commencer une nouvelle expérience d’essai W&B`

`wandb.init(project="my_project")`

`# 2. Sauvegarder tout modèle de configuration`

`wandb.config.learning_rate = 0.01`

`# [entraîner votre modèle]`

`# 3. Enregistrer des métriques pour visualiser la performance du modèle`

`pour i dans une plage(10) :`

            `wandb.log({"loss" : 10-i, "acc" : float(i)/10.0})`

**2. Créer et mettre à l’échelle une instance partagée**

Cette instance privée de wandb est excellente pour un test initial. Pour bénéficier pleinement des puissantes fonctionnalités collaboratives de wandb, vous devez avoir une instance partagée sur un serveur central, que vous pouvez [configurer sur AWS, GCP, Azure, Kubernetes ou Docker](https://docs.wandb.ai/self-hosted/setup).

**Risques de pertes de données**

Il est crucial que vous configuriez un fichier système extensible afin d’éviter les pertes de données : attribuez un espace supplémentaire, redimensionnez la taille du fichier système à mesure que vous enregistrez des données, et configurez des espaces de stockage de métadonnées et d’objets pour la sauvegarde. Si votre disque dur vient à manquer d’espace, l’instance s’arrêtera de fonctionner et les données additionnelles seront perdues.

**3. Configurer un nouveau compte utilisateur**

Pour les instances locales individuelles, vous pouvez créer un nouveau compte utilisateur lorsque vous lancez une instance locale pour la première fois. Vous pouvez voir et modifier les paramètres de votre compte ici : [http://localhost:8080/settings](http://localhost:8080/settings).

Pour les instances locales partagées, l’utilisateur par défaut est local et le mot de passe par défaut est un perceptron. Suivez les instructions pour réinitialiser votre mot de passe.

Pour changer votre identifiant \(login\), vous pouvez à tout moment lancer

wandb login –relogin

**4. Contrôle de l’emplacement d’enregistrement : local ou cloud wandb**

Si vous utilisez wandb sur plusieurs ordinateurs ou si vous avez pour habitude de passer d’une instance locale au cloud de wandb, il y a plusieurs manières de contrôler où vos essais seront enregistrés. Si vous voulez envoyer des métriques à une instance **locale** partagée et que vous avez configurer un DNS, vous pouvez

* configurer le drapeau hôte à l’adresse de l’instance locale à chaque fois que vous vous identifiez :

wandb login --host=http://wandb.your-shared-local-host.com

* configurer la variable d’environnement `WANDB_API_KEY` à l’adresse de l’instance locale :

export WANDB\_BASE\_URL = "http://wandb.your-shared-local-host.com"

Dans un environnement automatisé, vous pouvez configurer WANDB\_API\_KEY que vous trouverez sur le lien suivant [wandb.your-shared-local-host.com/authorize](http://wandb.your-shared-local-host.com/authorize).

Pour passer à l’instance **cloud** de wandb, configurez l’hôte à l’adresse api.wandb.ai :

wandb login --host=https://api.wandb.ai

ou

export WANDB\_BASE\_URL = [https://api.wandb.ai](https://api.wandb.ai/)

Vous pouvez également passer à la clé API de votre cloud, disponible sur le lien suivant [https://wandb.ai/settings](https://wandb.ai/settings), lorsque vous êtes connecté à votre compte hébergé sur le cloud wandb dans votre navigateur.

**5. Demander une licence d’équipe**

Contactez-nous au [support@wandb.com](mailto:support@wandb.com) pour demander une montée en gamme de votre licence. Nous pouvons vous offrir un essai gratuit allant jusqu’à 3 utilisateurs sur votre instance locale. Nous offrons aussi des accès d’équipe gratuits pour les projets d’équipe académiques et open source. Contactez-nous pour discuter des options.

## Ressources d’auto-hébergement

{% page-ref page="local.md" %}

{% page-ref page="setup.md" %}

{% page-ref page="configuration.md" %}

{% page-ref page="local-common-questions.md" %}

{% page-ref page="cloud.md" %}

