---
description: 使用Docker在自己的机器上运行Weights & Biases
---

# Local

## **启动服务器**

要在本地运行W＆B服务器，您需要安装[Docker](https://www.docker.com/products/docker-desktop)。 然后只需运行：

```text
wandb local
```

在后台，wandb客户端库正在运行[wandb/local](https://hub.docker.com/repository/docker/wandb/local) 的docker映像，将端口8080转发到主机，并配置您的计算机将度量发送到本地实例而不是我们的托管云。 如果要手动本地运行，可以运行以下docker命令：

```text
docker run --rm -d -v wandb:/vol -p 8080:8080 --name wandb-local wandb/local
```

### **中央托管**

在localhost上运行wandb非常适合进行初始测试，但是要利用wandb / local的协作功能，您应该将服务托管在中央服务器上。在“[设置](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MKaPhwzNIegNuInaekR/self-hosted/setup)”中可以找到有关在各个平台上设置中央服务器的说明。

### **基本配置**

运行`wandb local`配置您的本地计算机——将度量推送到[http://localhost:8080](http://localhost:8080/).。如果要在其他端口上托管本地，则可以将`--port`参数传递给`wandb local`。如果已使用本地实例配置DNS，则可以在任何计算机上运行：`wandb login --host=http://wandb.myhost.com`来报告度量标准。您还可以将**WANDB\_BASE\_URL**环境变量设置为任何计算机上的主机或IP，让它们报告给本地实例。在自动化环境中，您还需要从设置页面的api密钥中设置WANDB\_API\_KEY环境变量。如需将计算机还原为向我们的云托管报告度量，请运行`wandb login --host = https：//api.wandb.ai`。

###  **认证方式**

wandb/local的基本安装以默认用户local@wandb.com开始。默认密码是**perceptron**。前端将尝试自动使用该用户登录，并提示您重设密码。未购买许可证的wandb版本将允许您最多创建4个用户。您可以在位于`http://localhost:8080/admin/users 的wandb / local`的“用户管理”页面中配置用户。

###  **持久性和可扩展性**

 发送给W＆B的所有元数据和文件都存储在`/vol`目录中。如果您未在此位置安装持久化卷，则所有数据将在Docker进程终止时丢失。如果您购买了wandb/local的许可证，则可以将元数据存储在外部MySQL数据库中，并将文件存储在外部存储桶中，可免去状态容器。

**持久性和可扩展性**发送给W＆B的所有元数据和文件都存储在/vol目录中。如果您未在此位置安装持久化卷，则所有数据将在Docker进程终止时丢失。如果您购买了wandb/local的许可证，则可以将元数据存储在外部MySQL数据库中，并将文件存储在外部存储桶中，可免去状态容器。 

虽然可以通过利用如上所述方法装载到/vol持久化卷来使用W&B，但此解决方案不适用于生产工作负载。如果您决定以这种方式使用W&B，建议提前分配足够的空间来存储当前和未来的度量需求，并强烈建议可以根据需要调整下面的文件存储的大小。

此外，应设置警报，以便在超出最小存储阈值时通知您调整下层文件系统的大小。对于非图像/视频/音频的繁重工作负载，我们建议下层卷中至少有100GB的可用空间。如果使用大文件测试W&B，下层存储需要有足够的空间来满足这些需求。在任何情况下，分配的空间都需要体现工作流的度量和输出。

**升级**

我们会定期将新版本的wandb/local推送到dockerhub。如需升级，您可以运行：

```text
$ wandb local --upgrade
```

要手动升级实例，可以运行以下命令

```text
$ docker pull wandb/local
$ docker stop wandb-local
$ docker run --rm -d -v wandb:/vol -p 8080:8080 --name wandb-local wandb/local
```

### **获得许可证**

如果您有兴趣配置团队、使用外部存储或将wandb/local部署到Kubernetes集群，请发送电子邮件至[contact@wandb.com](mailto:contact@wandb.com)

