# Docker

### **Docker集成**

W&B可以存储一个指针，它指向运行你代码的Docker镜像 ，让你能够将之前的实验恢复到它曾经运行的环境中。wandb库寻找**WANDB\_DOCKER** 环境变量来持久化这个状态。我们提供了一些帮助程序来自动设置这个状态。

###  **本地开发** <a id="local-development"></a>

`wandb docker` 是一个启动docker容器的命令，传递wandb环境变量，挂载你的代码，并确保安装了wandb。默认情况下，该命令使用安装了TensorFlow、PyTorch、Keras和Jupyter的docker镜像。你可以使用相同的命令来启动你自己的docker镜像：`wandb docker my/image:latest` 。该命令将当前目录挂载到容器的 "/app" 目录下，你可以使用"--dir" 标志来更改。

### **生产环境** <a id="production"></a>

`wandb docker-run` 命令是为生产环境工作负载提供的。它的目的是作为 `nvidia-docker`的dropin替代品。它是`docker run` 命令的一个简单包装器，在调用中添加了你的认证信息和**WANDB\_DOCKER** 环境变量。如果你没有传递 "--runtime" 标志，而机器上有 `nvidia-docker` ,这也会确保runtime是设置为nvidia。

### Kubernetes <a id="kubernetes"></a>

如果你在Kubernetes中运行你的训练工作负载，并且k8s API暴露给你的pod（默认情况下是这样的）。wandb将查询该API以获得镜像摘要并自动设置**WANDB\_DOCKER**环境变量。

##  **恢复** <a id="restoring"></a>

如果一个运行被设置了**WANDB\_DOCKER** 环境变量，调用`wandb restore username/project:run_id` 将检出一个新分支，恢复你的代码，并启动用于训练的docker镜像，该镜像使用原始命令进行了预填充。 [  
](https://docs.wandb.ai/integrations/kubeflow)

