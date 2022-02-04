# Kubeflow

## Kubeflow Integration

### Training Jobs

Currently W\&B automatically reads the **TF\_CONFIG** environment variable to group distributed runs.

### Pipelines

wandb provides an `arena_launcher_op` that can be used in [pipelines](https://github.com/kubeflow/pipelines).

If you want to build your own custom launcher op, you can also use this [code](https://github.com/wandb/client/blob/master/wandb/kubeflow/\_\_init\_\_.py) to add pipeline\_metadata. For wandb to authenticate you should add the **WANDB\_API\_KEY** to the operation, then your launcher can add the same environment variable to the training container.

```python
import os
from kubernetes import client as k8s_client

op = dsl.ContainerOp( ... )
op.add_env_variable(k8s_client.V1EnvVar(
        name='WANDB_API_KEY',
        value=os.environ["WANDB_API_KEY"]))
```
