# Docker

## Intégration Docker

W&B peut stocker un pointeur à l’image Docker sur laquelle votre code s’est exécuté, vous donnant la possibilité de restaurer une expérience antérieure dans l’environnement exact dans laquelle elle a été effectuée. La librairie wandb cherche la variable d’environnement **WANDB\_DOCKER** pour persister dans cet état. Nous vous fournissons quelques aides pour automatiquement régler cet état.

###  Développement local

 The command mounts the current directory into the "/app" directory of the container, you can change this with the "--dir" flag. `wandb docker` est une commande qui initie un conteneur docker, passe dans les variables d’environnement wandb, monte votre code, et s’assure que wandb est installé. Par défaut, cette commande utilise une image docker avec les installations de TensorFlow, PyTorch, Keras, et Jupyter. Vous pouvez utiliser la même commande pour initier votre propre image docker : `wandb docker my/image:latest` . Cette commande monte le répertoire courant dans le répertoire "/app" du conteneur, vous pouvez changer ceci avec le flag "--dir".

### Production

 La commande `wandb docker-run` est fournie pour les charges de travail de production. Elle est pensée comme un remplacement direct de `nvidia-docker`. C’est un simple wrapper pour la commande docker run qui ajoute vos références \(credentials\) et la variable d’environnement **WANDB\_DOCKER** à l’appel. Si vous ne passez pas le flag "--runtime" et que `nvidia-docker` est disponible sur votre machine, il s’assure aussi que le runtime est réglé sur nvidia.

### Kubernetes

Si vous faites vos entraînements de charge de travail dans Kubernetes et que l’API k8s est exposée à votre pod \(ce qui est le cas par défaut\), wandb fera une requête de l’API pour le résumé \(digest\) de l’image docker et réglera automatiquement la variable d’environnement **WANDB\_DOCKER**.

##  Restaurer

Si un essai a été instrumenté avec la variable d’environnement **WANDB\_DOCKER**, appeler`wandb restore username/project:run_id`passera une nouvelle branche pour restaurer votre code, puis lancera l’image docker exacte utilisée pour entraîner le pre-populated avec la commande originale.

