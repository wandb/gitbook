# Kubeflow

### Intégration Kubeflow

L’utilisation de certaines fonctionnalités requiert des dépendances supplémentaires. Installez toutes les dépendances Kubeflow en exécutant `pip install wandb[kubeflow]`.

###  Traitement d’entraînement

W&B lit automatiquement la variable d’environnement **TF\_CONFIG** pour regrouper les essais distribués.

### Arena

La bibliothèque wandb s’intègre avec [arena](https://github.com/kubeflow/arena) en ajoutant automatiquement des références \(credentials\) aux environnements de conteneurs. Si vous souhaitez utiliser le wrapper wandb de manière locale, ajoutez ce qui suit à votre `.bashrc`

```text
alias arena="python -m wandb.kubeflow.arena"
```

Si vous n’avez pas installé arena localement, la commande susmentionnée utilisera l’image Docker `wandb/arenaet` essaiera de monter vos configurations kubectl.

### Pipelines

 wandb fournit un `arena_launcher_op` qui peut être utilisé dans les [pipelines](https://github.com/kubeflow/pipelines).

Si vous voulez construire votre propre lanceur de processus \(launcher op\) personnalisé, vous pouvez aussi utiliser ce [code](https://github.com/wandb/client/blob/master/wandb/kubeflow/__init__.py) pour ajouter pipeline\_metadata. Pour que wandb s’authentifie, vous devez ajouter **WANDB\_API\_KEY** à l’opération, et ensuite, votre lanceur pourra ajouter la même variable d’environnement au conteneur d’entraînement.

```python
import os
from kubernetes import client as k8s_client

op = dsl.ContainerOp( ... )
op.add_env_variable(k8s_client.V1EnvVar(
        name='WANDB_API_KEY',
        value=os.environ["WANDB_API_KEY"]))
```

