# Kubeflow

##  Integración de Kubeflow

Usar ciertas características requiere dependencias adicionales. Instala las dependencias de Kubeflow al ejecutar pip install`pip install wandb[kubeflow]`.

###  Trabajos de Entrenamiento

Actualmente, W&B lee automáticamente la variable de entorno **TF\_CONFIG** para agrupar ejecuciones distribuidas.

### Arena

La biblioteca wandb se integra con [arena](https://github.com/kubeflow/arena) al agregar automáticamente las credenciales en los entornos del contenedor. Si deseas usar localmente al wrapper de wandb, agrega lo siguiente a `tu .bashrc`

```text
alias arena="python -m wandb.kubeflow.arena"
```

Si no tienes instalado arena de forma local, el comando anterior utilizará la imagen de docker `wandb/arena`, e intentará montar tus configuraciones kubectl.

### Pipelines

wandb provee un `arena_launcher_op` que puede ser usado en [pipelines](https://github.com/kubeflow/pipelines).

Si deseas construir tu propio launcher op personalizado, también puedes usar este [código](https://github.com/wandb/client/blob/master/wandb/kubeflow/__init__.py) para agregar pipeline\_metadata. Para que wandb haga la autenticación, deberías agregar **WANDB\_API\_KEY** a la operación, entonces tu lanzador puede agregar la misma variable de entorno al contenedor de entrenamiento.

```python
import os
from kubernetes import client as k8s_client

op = dsl.ContainerOp( ... )
op.add_env_variable(k8s_client.V1EnvVar(
        name='WANDB_API_KEY',
        value=os.environ["WANDB_API_KEY"]))
```

