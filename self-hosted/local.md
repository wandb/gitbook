---
description: Exécutez Weights and Biases sur vos propres machines en utilisant Docker
---

# Local

## Démarrer le serveur

Pour exécuter localement le serveur W&B, vous aurez besoin d’avoir installé [Docker](https://www.docker.com/products/docker-desktop). Puis, exécutez simplement :

```text
wandb local
```

Dans les coulisses, la librairie client wandb exécute l’image docker [_wandb/local_](https://hub.docker.com/repository/docker/wandb/local), ce qui fait passer le port 8080 à l’hôte, et configure votre machine pour envoyer des mesures à une instance locale plutôt que sur notre cloud hébergé. Si vous souhaitez exécuter notre container local manuellement, vous pouvez exécuter la commande docker suivante :

```text
docker run --rm -d -v wandb:/vol -p 8080:8080 --name wandb-local wandb/local
```

###  Hébergement centralisé

 Exécuter wandb en local est excellent pour les tests initiaux, mais pour faire plein usage des fonctionnalités collaboratives de wandb/local, vous devriez héberger le service sur un serveur central. Les instructions pour mettre en place un serveur centralisé sur des plateformes diverses peuvent être trouvées dans la section [Mise en place](https://docs.wandb.ai/self-hosted/setup).

###  Configuration de base

Exécuter `wandb local` configure votre machine locale pour qu’elle envoie les mesures à [http://localhost:8080](http://localhost:8080/). Si vous voulez héberger localement sur un port différent, vous pouvez passer l’argument –port dans wandb local. Si vous avez configuré le DNS avec votre instance locale, vous pouvez exécuter : `wandb login --host=http://wandb.myhost.com sur n’importe quelle machine sur laquelle vous voulez faire un rapport de mesures. Vous pouvez aussi paramétrer la variable d’environnement WANDB_BASE_URL sur un hôte ou l’IP de n’importe quelle machine que vous souhaitez voir faire un rapport à votre instance locale. Dans un environnement automatisé, vous voudrez également paramétrer la variable d’environnement WANDB_API_KEY à l’intérieur d’une clef api depuis votre page de paramètres. Pour restaurer une machine, de sorte qu’elle envoie les mesures sur notre solution hébergée par cloud, exécutez` wandb login --host=https://api.wandb.ai.

### Authentification

L’installation de base de wandb/local commence avec un utilisateur par défaut [local@wandb.com](mailto:local@wandb.com). Le mot de passe par défaut est **perceptron**. Le frontend essayera de se connecter avec cet utilisateur automatiquement et vous demandera de réinitialiser le mot de passe. Une version sans licence de wandb vous permet de créer jusqu’à 4 utilisateurs. Vous pouvez configurer les utilisateurs sur la page User Admin de wandb/local que vous trouverez ici : `http://localhost:8080/admin/users`

###  Persistance et scalabilité

Toutes les métadonnées et tous les fichiers envoyés à W&B sont stockées dans le répertoire `/vol. Si vous ne montez pas un volume persistant à cet endroit, toutes les données seront perdues lorsque le processus docker mourra.`Si vous achetez une licence pour wandb/local, vous pouvez stocker des métadonnées dans une base de données externe MySQL et des fichiers dans un bucket de stockage externe, ce qui retire le besoin d’un container à états, et cela vous donne la résilience nécessaire ainsi que les fonctionnalités de scalabilité typiquement nécessaires pour les volumes de travail de production.Bien que W&B puisse être utilisé en employant le volume persistant monté dans `/vol` comme précisé ci-dessus, cette solution n’est pas faite pour les volumes de travail de production. Si vous décidez d’utiliser W&B de cette manière, il est recommandé d’avoir assez d’espace alloué en avance pour stocker les besoins actuels et futurs des mesures, et il est fortement suggéré que le stockage de fichier sous-jacent puisse être modifié en taille comme sera nécessaire. En plus de cela, des alertes devaient être mises en place pour vous faire savoir lorsque les paliers de stockage minimum sont atteints pour modifier la taille du système fichier sous-jacent.À des fins d’essais, nous recommandons au moins 100 GB d’espace libre dans le volume sous-jacent pour les volumes de travail lourds, qui ne soient pas des images, des vidéos ou de l’audio. Si vous testez W&B avec de grands fichiers, le stockage sous-jacent doit avoir suffisamment d’espace pour s’accommoder à ces besoins. Dans tous les cas, l’espace alloué doit réfléchir les mesures et les sorties \(output\) de vos flux de travaux. 

### Mise à niveau

Nous apportons régulièrement de nouvelles versions de wandb/local dans dockerhub. Pour mettre à niveau, vous pouvez exécuter :

```text
$ wandb local --upgrade
```

Pour mettre manuellement votre instance à niveau, vous pouvez exécuter ce qui suit

```text
$ docker pull wandb/local
$ docker stop wandb-local
$ docker run --rm -d -v wandb:/vol -p 8080:8080 --name wandb-local wandb/local
```

###  Obtenir une licence

Si vous êtes intéressés par le fait de configurer des équipes, d’utiliser un stockage externe, ou de déployer wandb/local sur un cluster Kubernetes, envoyez-nous un email à [contact@wandb.com](mailto:contact@wandb.com)

