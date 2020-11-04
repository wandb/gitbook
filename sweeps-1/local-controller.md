---
description: 在本地运行搜索和停止算法，而不是使用我们的云托管服务
---

# Local Controller

默认情况下，超参数控制器由W＆B托管为云服务。 W＆B代理与控制器通信，以确定用于训练的下一组参数。控制器还负责运行早期停止算法，以确定哪些运行可以停止。

本地控制器功能允许用户在本地运行搜索和停止算法。本地控制器使用户能够检查和检测代码，以便调试问题以及开发可集成到云服务中的新功能。

{% hint style="info" %}
当前，本地控制器仅限于运行单个代理。
{% endhint %}

**本地控制器配置**

要启用本地控制器，请将以下内容添加到扫描配置文件中：

```text
controller:
  type: local
```

**运行本地控制器**

以下命令将启动扫描控制器：

```text
wandb controller SWEEP_ID
```

或者，您可以在初始化扫描时启动控制器：

```text
wandb sweep --controller sweep.yaml
```

