---
description: 使用Docker在自己的机器上运行权阈(W&B)
---

# Local

## **启动服务器**

要在本地运行W＆B服务器，您需要安装[Docker](https://www.docker.com/products/docker-desktop)。 然后只需运行：

```text
wandb local
```

在后台，wandb客户端库正在运行[_wandb/local_](https://hub.docker.com/repository/docker/wandb/local) 的docker映像，将端口8080转发到主机，并将您的计算机配置为将指标发送到本地实例而不是我们的托管云。 如果要手动本地运行，可以运行以下docker命令：

```text
docker run --rm -d -v wandb:/vol -p 8080:8080 --name wandb-local wandb/local
```

### **集中托管**

在localhost上运行wandb非常适合进行初始测试，但是要利用wandb / local的协作功能，您应该将服务托管在中央服务器上。在“[设置](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MKaPhwzNIegNuInaekR/self-hosted/setup)”部分中可以找到有关在各种平台上设置集中式服务器的说明。

## 基本配置

运行`wandb local`会进行您的本地计算机配置——将指标推送到[http://localhost:8080。如果要在其他端口上托管本地，则可以将\`--port\`参数传递给wandb](http://localhost:8080。如果要在其他端口上托管本地，则可以将`--port`参数传递给wandb) local。如果已使用本地实例配置DNS，则可以在任何计算机上运行：`wandb login --host=http://wandb.myhost.co`\`来报告度量标准。您还可以将WANDB\_BASE\_URL环境变量设置为任何计算机上的主机或IP，他们可以报告给本地实例。在自动化环境中，您还需要从设置页面的api密钥中设置`WANDB_API_KEY`环境变量。要将计算机还原为向我们的云托管报告指标，请运行`wandb login --host = https：//api.wandb.ai`。

### 认证方式

wandb/local的基本安装以默认用户local@wandb.com开始。默认密码是**perceptron**。前端将尝试自动使用该用户登录，并提示您重设密码。未经许可的wandb版本将允许您最多创建4个用户。您可以在位于`http://localhost:8080/admin/users` 的wandb / local的“用户管理”页面中配置用户。

## **持久性**

发送给W＆B的所有元数据和文件都存储在`/vol`目录中。如果您未在此位置安装持久卷，则当Docker进程终止时，所有数据将丢失。如果您购买了wandb/local的许可证，则可以将元数据存储在外部MySQL数据库中，并将文件存储在外部存储桶中，从而无需有状态容器。

**升级**

我们会定期将新版本的wandb/local推送到dockerhub。要升级，您可以运行：

```text
$ wandb local --upgrade
```

要手动升级实例，可以运行以下命令

```text
$ docker pull wandb/local
$ docker stop wandb-local
$ docker run --rm -d -v wandb:/vol -p 8080:8080 --name wandb-local wandb/local
```

**取得许可证**

如果您有兴趣配置团队，使用外部存储或将wandb/local部署到Kubernetes集群，请发送电子邮件至[contact@wandb.com](mailto:contact@wandb.com)

