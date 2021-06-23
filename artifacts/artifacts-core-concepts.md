# Artifacts Core Concepts

在本指南中，您将学习开始使用W&BArtifacts所需的一切。开始吧！

## **重点** <a id="the-big-picture"></a>

W&B Artifacts旨在轻松版本控制您的数据集和模型，无论您是否想将文件存储在我们这里，还是您已经有一个想要我们追踪的存储桶。一旦您追踪了您的数据集或模型文件，W&B将自动记录每一个修改，并为您提供完整且可审查的文件修改历史记录。从而让您专注于进化数据集和训练模型的有趣和重要的部分，而W&B则处理追踪细节的所有繁琐过程。

## **术语** <a id="terminology"></a>

我们先从一些定义开始。首先，我们所说的“工件”（artifact）到底是什么意思？

从概念上讲，一个**工件**只是一个目录，您可以在其中存储任何东西，无论是图像、HTML、代码、音频还是原始二进制数据。您可以像使用S3或谷歌云存储一样使用它。每次更改此目录的内容时，W&B将创建工件的新**版本**，而不是简单地覆盖以前的内容。

假设我们有以下目录结构：

```text
images|-- cat.png (2MB)|-- dog.png (1MB)
```

我们把它记录为一个新工件的第一个版本`animals`：

```text
#!/usr/bin/env python#log_artifact.pyimport wandb​run = wandb.init()artifact = wandb.Artifact('animals', type='dataset')artifact.add_dir('images')run.log_artifact(artifact) # Creates `animals:v0`
```

用W&B的说法，这个版本的索引就是`v0`。工件的每个新版本都会增加一个索引。您可以想象，一旦您有数百个版本，通过索引引用特定版本就会变得混乱不堪且容易出错。这就是**别名**派上用场的地方。**别名**让您可以为特定版本指定人类可读的名称。

更具体一点，假设我们想用新图像更新我们的数据集，并将新版本标记为我们的最新图像。以下是我们新目录结构：

To make this more concrete, let's say we want to update our dataset with a new image and mark the new version as our `latest` image. Here's our new directory structure:

```text
images|-- cat.png (2MB)|-- dog.png (1MB)|-- rat.png (3MB)
```

现在，我们可以简单地重新运行`log_artifact.py`就可以生产`animals:v1`。W&B将自动为最新版本分配别名latest，因此我们还可以用animals:latest.对其引用，而不用版本索引。通过将`aliases=['my-cool-alias']`传递给`log_artifact`，可以自定义为版本应用的别名。

引用工件很简单。在我们的训练脚本中，我们只需要这么做就可以拉入您的数据集的当前最新版本：

```text
import wandb​run = wandb.init()animals = run.use_artifact('animals:latest')directory = animals.download()​# Train on our image dataset...
```

就这样！现在，每当我们想对数据集进行更改时，我们可以再次运行`log_artifact.py`，其余的则由W&B来处理。然后，训练脚本就会自动拉入`latest`版本。

## **存储布局** <a id="storage-layout"></a>

随着工件随时间的发展，版本开始积累，您可能会开始担心存储数据集所有迭代的空间需求。幸运的是，W&B存储工件文件时，只有在两个版本之间出现变化的文件才会产生存储成本。

让我们再参考一下`animals:v0`和`animals:v1`，它们追踪了以下内容：

```text
images|-- cat.png (2MB) # Added in `v0`|-- dog.png (1MB) # Added in `v0`|-- rat.png (3MB) # Added in `v1`
```

虽然`v1` 总共跟踪6MB的文件，但它只占用了3MB的空间，因为它与`v0`共享剩余的3MB。如果您要删除`v1`，您将回收与rat.png关联的3MB。另一方面，如果要删除`v0`，那么`v1`将需要继承cat.png和dog.png的存储成本使其大小达到6MB。

## **数据隐私与合规性** <a id="data-privacy-and-compliance"></a>

在记录工件时，文件将被上传到由W&B管理的Google云存储桶。静止和传输中的存储桶内容都经过加密。而且，工件文件只对有权访问相应项目的用户可见。

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-MUtth0QHsFU8uW3xGHU%2F-MUtuFRyuOIF0Kzofe5f%2Fimage.png?alt=media&token=4a4d710c-4ee6-4c92-8b08-1b3c79074cf8)

当您删除工件的一个版本时，所有可以安全删除的文件（意味着这些文件不在以前或以后的版本中使用）都会立即从我们的存储桶中删除。同样，当您删除整个工件时，其所有内容都将从我们的存储中删除。

对于不能驻留在多租户环境中的敏感数据集，可以使用连接到您的云桶的私有W&B服务器或[**引用工件**](https://docs.wandb.ai/artifacts/references)。引用工件维护指向存储桶或服务器上的文件的链接，这意味着W&B仅追踪与文件相关联的元数据，而不是文件本身

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-MUtth0QHsFU8uW3xGHU%2F-MUtuIUyqweoWl7ACTWA%2Fimage.png?alt=media&token=7a26ccc4-f86c-456e-9603-c77da642fff1)

构建**引用工件**的原理与常规工件相同：

```text
import wandb​run = wandb.init()artifact = wandb.Artifact('animals', type='dataset')artifact.add_reference('s3://my-bucket/animals')
```

您保有对存储桶及其文件的控制权，而W&B仅代表您追踪所有元数据。[  
](https://docs.wandb.ai/artifacts)

