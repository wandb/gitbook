# Kubeflow

### Intégration Kubeflow

L’utilisation de certaines fonctionnalités exige des dépendances supplémentaires. Installez toutes les dépendances Kubeflow en exécutant `pip install wandb[kubeflow]`.

###  Traitement d’entraînement

W&B lit automatiquement la variable d’environnement **TF\_CONFIG** pour regrouper les essais distribués.

### Arena

La librairie wandb s’intègre avec [arena](https://github.com/kubeflow/arena) en ajoutant automatiquement des références \(credentials\) aux environnements containers. Si vous souhaitez utiliser le wrapper wandb de manière locale, ajoutez ce qui suit à votre`.bashrc`

```text
alias arena="python -m wandb.kubeflow.arena"
```

Si vous n’avez pas installé arena localement, la commande ci-dessus utilisera l’image docker `wandb/arena` et essayera de monter vos configs kubectl.

### Pipelines

 wandb fournit un `arena_launcher_op` qui peut être utilisé dans les [pipelines](https://github.com/kubeflow/pipelines).

Si vous voulez construire votre propre launcher op personnalisé, vous pouvez aussi utiliser ce [code](https://github.com/wandb/client/blob/master/wandb/kubeflow/__init__.py) pour ajouter pipeline\_metadata. Pour que wandb s’authentifie, vous devriez ajouter **WANDB\_API\_KEY** à l’opération, et ensuite, votre launcher pourra ajouter la même variable d’environnement au container d’entraînement.

```python
import os
from kubernetes import client as k8s_client

op = dsl.ContainerOp( ... )
op.add_env_variable(k8s_client.V1EnvVar(
        name='WANDB_API_KEY',
        value=os.environ["WANDB_API_KEY"]))
```

