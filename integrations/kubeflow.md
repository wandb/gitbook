# Kubeflow

## **Kubeflow集成** <a id="kubeflow-integration"></a>

使用某些功能需要额外的依赖关系。通过运行`pip install wandb[kubeflow]`安装所有Kubeflow的依赖关系。

### **训练作业** <a id="training-jobs"></a>

目前W&B会自动读取**TF\_CONFIG** 环境变量，对分布式运行进行分组。

### Arena <a id="arena"></a>

wandb库通过自动将认证信息添加到容器环境与 [arena](https://github.com/kubeflow/arena) 集成。如果你想在本地使用wandb包装器，请在你的 `.bashrc` 中添加以下内容。

```text
alias arena="python -m wandb.kubeflow.arena"
```

I如果你没有在本地安装arena ，上面的命令将使用`wandb/arena` docker镜像，并尝试挂载你的kubectl 配置。

### Pipelines <a id="pipelines"></a>

wandb提供了一个可以在[pipelines](https://github.com/kubeflow/pipelines)中使用的`arena_launcher_op`

 如果你想要构建你自己的自定义启动器操作，你也可以使用[这段代码](https://github.com/wandb/client/blob/master/wandb/kubeflow/__init__.py)来添加pipeline\_metadata。为了wandb认证，你应该将**WANDB\_API\_KEY** 添加到该操作中，然后你的启动器可以将相同的环境变量添加到训练容器中。

```text
import osfrom kubernetes import client as k8s_client​op = dsl.ContainerOp( ... )op.add_env_variable(k8s_client.V1EnvVar(        name='WANDB_API_KEY',        value=os.environ["WANDB_API_KEY"]))
```

[  
](https://docs.wandb.ai/integrations/ignite)

