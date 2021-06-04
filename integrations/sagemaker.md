# SageMaker

## Intégration SageMaker

W&B s’intègre avec [Amazon SageMaker](https://aws.amazon.com/sagemaker/), en lisant automatiquement les hyperparamètres, en regroupant les essais distribués et en reprenant les essais à partir des checkpoints.  


### Authentification

W&B cherche un fichier nommé `secrets.env` relatif au script d’entraînement et le charge dans l’environnement lorsque `wandb.init()` est appelé. Vous pouvez générer un fichier secrets.env en appelant `wandb.sagemaker_auth(path="source_dir")` dans le script que vous utilisez pour lancer vos expériences. Assurez-vous d’ajouter ce fichier à votre `.gitignore !`

###  Estimateurs existants

 Si vous utilisez un des estimateurs préconfigurés de SageMaker, vous devez ajouter un `requirements.txt` dans votre répertoire source qui inclut wandb.

```text
wandb
```

 Si vous utilisez un estimateur qui utilise Python 2, il faudra que vous installiez psutil directement à partir d’une [roue](https://pythonwheels.com/) \(wheel\) avant d’installer wandb :

```text
https://wheels.galaxyproject.org/packages/psutil-5.4.8-cp27-cp27mu-manylinux1_x86_64.whl
wandb
```

Un exemple complet est disponible sur [GitHub](https://github.com/wandb/examples/tree/master/examples/pytorch/pytorch-cifar10-sagemaker), et vous pouvez en lire davantage sur le sujet sur notre [blog](https://www.wandb.com/blog/running-sweeps-with-sagemaker).

