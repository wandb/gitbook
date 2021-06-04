# Docker

## Intégration Docker

W&B peut stocker un pointeur sur l’image Docker sur laquelle votre code s’est exécuté, vous donnant la possibilité de restaurer une expérience antérieure dans l’environnement exact dans laquelle elle a été effectuée. La bibliothèque wandb cherche la variable d’environnement **WANDB\_DOCKER** pour persister dans cet état. Nous vous fournissons quelques aides pour configurer cet état automatiquement.

###  Développement local

 Cette commande monte le répertoire courant dans le répertoire "/app" du conteneur, vous pouvez changer ceci avec le flag "--dir". `wandb docker` est une commande qui initie un conteneur docker, passe dans les variables d’environnement wandb, monte votre code, et s’assure que wandb est installé. Par défaut, cette commande utilise une image docker avec les installations de TensorFlow, PyTorch, Keras, et Jupyter. Vous pouvez utiliser la même commande pour initier votre propre image docker : `wandb docker my/image:latest` . Cette commande monte le répertoire en cours dans le répertoire "/app" du conteneur, vous pouvez changer ceci avec le drapeau "--dir".

### Production

 La commande `wandb docker-run` est fournie pour les charges de travail de production. Elle a été conçue pour remplacer directement le nvidia-docker. C’est un simple wrapper pour la commande `docker run` qui ajoute vos références \(credentials\) et la variable d’environnement **WANDB\_DOCKER** à l’appel. Si vous n’ajoutez pas le drapeau "--runtime" et que nvidia-docker est disponible sur votre ordinateur, il s’assure aussi que l’environnement d’exécution \(runtime\) soit paramétré sur nvidia. 

### Kubernetes

Si vous exécutez vos entraînements de charge de travail sur Kubernetes et que l’API k8s est exposée à votre Pod \(ce qui est le cas par défaut\), wandb fera une requête de l’API pour le condensé \(digest\) des imagesdocker et configurera automatiquement la variable d’environnement **WANDB\_DOCKER**.

##  **Restauration**

Si un essai a été instrumenté avec la variable d’environnement **WANDB\_DOCKER**, appeler `wandb restore username/project:run_id` ajoutera une nouvelle branche pour restaurer votre code, puis lancera l’image docker exacte utilisée pour l’entraînement prérempli avec la commande intitiale.

