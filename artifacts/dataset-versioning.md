---
description: 使用Artifacts可进行数据集版本控制的指南
---

# Dataset Versioning

W&B Artifacts可帮助您在项目的整个生命周期中保存和组织机器学习数据集。

### **常见使用案例**

1. **​**[**无缝版本数据**](https://docs.wandb.ai/artifacts/dataset-versioning#25c79f05-174e-4d35-abda-e5c238b8d6d6)，不中断您的工作流
2. [**预打包数据拆分**](https://docs.wandb.ai/artifacts/dataset-versioning#7ccfb650-1f87-458c-a4e2-538138660292)，如训练、验证和测试集
3. **​**[**迭代优化数据集**](https://docs.wandb.ai/artifacts/dataset-versioning#cee1428d-3b7a-4e1b-956b-e83170e7038f)，不会让团队失去同步
4. **​**[**处理多个数据集**](https://docs.wandb.ai/artifacts/dataset-versioning#4ba93c33-dd39-468b-8b3e-96c938bbd024)，如微调和域内自适应
5. **​**[**可视化和共享您的数据工作流**](https://docs.wandb.ai/artifacts/dataset-versioning#57023a52-2c00-4b24-8e17-b193b40e216b)，将所有工作集中在一个位置

### **灵活的追踪和托管**

除了这些常见的场景之外，您还可以使用核心Artifact功能来上传、版本控制、指定别名、比较和下载数据，通过S3、GCP或https支持本地或远程文件系统上的任何自定义数据集工作流。

## **核心Artifacts功能** <a id="403224a6-95d9-4095-9161-076362f8fc5f"></a>

W&B Artifacts通过以下基本功能为数据集提供支持：

1. **上传**: 使用`run.log_artifact()`开始追踪和版本控制所有数据（文件或目录）。您也可以[通过引用](https://docs.wandb.ai/v/zh-hans/artifacts)追踪远程文件系统（例如S3或GCP中的云存储）中的数据集，使用链接或URI而不是原始内容。
2. **版本控制**:  定义工件，为其指定类型 \(`"raw_data"`、`"preprocessed_data"`、`"balanced_data"`\) 和名称\(`"imagenet_cats_10K"`\)。当您再次记录相同的名称时，W&B将自动创建包含最新内容的工件的新版本。
3. **指定别名**:  选择任意两个版本并排浏览内容。我们也在研究一个数据集可视化的工具，[在这里了解更多→](https://docs.wandb.ai/datasets-and-predictions)​
4. **对比**: 选择任意两个版本并排浏览内容。我们也在研究一个数据集可视化的工具，[在这里了解更多→](https://docs.wandb.ai/datasets-and-predictions)​
5. **下载:** 获取工件的本地副本或通过引用验证内容。

有关这些特性的更多详细信息，请查看[Artifacts Core Concepts](https://docs.wandb.ai/artifacts/artifacts-core-concepts)。

##  **无缝版本数据** <a id="25c79f05-174e-4d35-abda-e5c238b8d6d6"></a>

W&B工件的直接价值是自动对数据进行版本控制：追踪您可能在其中添加、删除、替换或编辑项目的单个文件和目录的内容。所有这些操作都是可追溯和可逆的，减少了正确处理数据的认知负担。一旦您创建并上传一个工件，添加和记录新文件将创建该工件的新版本。在后台，我们将比较内容（通过校验和进行深入的比较），以便只上传更改部分，就像 [git](https://www.atlassian.com/git/tutorials/what-is-git)一样。您可以在浏览器中查看所有版本和各个文件、所有版本中的不同内容，并按索引或别名下载任何特定版本（默认情况下，`"latest"`是最新版本的别名）。 为保持数据传输精简和快速，wandb会缓存文件。

示例代码：

```python
run = wandb.init(project="my_project")
my_data = wandb.Artifact("new_dataset", type="raw_data")
my_data.add_dir("path/to/my/data")
run.log_artifact(my_data)
```

 在[这个例子](https://wandb.ai/stacey/mendeleev/artifacts/balanced_data/inat_80-10-10_5K/ab79f01e007113280018)中，我有三个包含1K、5K和10K项的数据集，我可以查看和比较子文件夹中的文件名（按数据拆分或按类标签）。

![](../.gitbook/assets/screen_shot_2021-02-23_at_3.18.03_pm%20%281%29%20%281%29.png)

## **预打包数据拆分** <a id="7ccfb650-1f87-458c-a4e2-538138660292"></a>

在迭代模型和训练方案时，您可能需要不同的数据切片，从而改变

* **项目数量**: 作为概念证明/快速迭代开始的一个较小数据集，或者用于查看模型从更多数据中受益程度的几个不断增加的数据集
* **训练/val/试验分配及比例**: 项目比例不同的训练/试验拆分或训练/val/试验拆分
* **每类平衡**: 均衡标签表示（每个K个类中有N个图像）或遵循数据的现有不平衡分布

  或您任务的其他特定因素。

您可以将所有这些作为工件进行保存和独立版本控制，并在不同的计算机、环境、团队成员等中按名称可靠地下载它们，而不必记下或记住您上次在什么位置保存了哪个版本。

Artifacts系统尽可能避免重复文件，因此跨多个版本使用的任何文件都只存储一次！

示例代码：

```python
run = wandb.init(project="my_project")
my_data = wandb.Artifact("new_dataset", type="raw_data")

for dir in ["train", "val", "test"]:
	my_data.add_dir(dir)`

run.log_artifact(my_data)
```

查看[文件内容 →](https://wandb.ai/stacey/mendeleev/artifacts/balanced_data/inat_80-10-10_5K/ab79f01e007113280018/files)​

![](../.gitbook/assets/screen-shot-2021-03-03-at-12.55.55-pm.png)

##  **迭代优化数据** <a id="cee1428d-3b7a-4e1b-956b-e83170e7038f"></a>

当您浏览训练数据或添加新批次示例时，您可能会注意到以下问题

* 不正确的地面实况标签
* 难负例或通常错误分类的例子
* 有问题的类失衡

若要清理和优化数据，您可以修改不正确的标签，添加或删除文件以解决不平衡问头，或者将难负例分组到特殊的测试拆分中。使用Artifacts，一旦完成了一批更改，就可以调用`run.log_artifact()`将新版本推送到云。 这将自动使用新版本更新工件以反映您的更改，同时保留以前更改的沿袭和历史。

您可以使用自定义别名标记工件版本、记录更改的内容、将元数据与每个版本一起存储以及查看哪些使用了特定版本的实验运行。这样，您的整个团队就可以确保他们正在使用`latest`或`stable`版本的数据。

我们也在努力让这个精细的过程更简单、更直观——[在此→](https://docs.wandb.ai/datasets-and-predictions)​查看我们的测试版数据集＆预测

### **版本控制是自动的** <a id="4d22c630-6fed-4fab-a802-1c7ae0f2d8db"></a>

如果工件发生变化，只需要重新运行相同的工件创建脚本。在这种情况下，假设`nature-data`目录包含两个照片id列表，`animal-ids.txt`和`plant-ids.txt`。我们编辑`animals-ids.txt`以删除错误标记的示例。这个脚本将整齐地捕获新版本——我们将校验工件、识别变化并追踪新版本。如果没有任何更改，我们不会重新上传任何数据（即，在这种情况下，我们不会重新上传`plant-ids.txt`）或创建新版本。

```python
run = wandb.init(job_type="dataset-creation")
artifact = wandb.Artifact('nature-dataset', type='dataset')
artifact.add_dir("nature-data")

# Edit the list of ids in one of the file to remove the mislabeled examples
# Let's say nature-photos contains "animal-ids.txt", which changes
# and "plant-ids", which does not

# Log that artifact, and we identify the changed file
run.log_artifact(artifact)
# Now you have a new version of the artifact, tracked in W&B
```

为数据集指定自定义名称并使用注释或键值对元数据对其进行注释

![](../.gitbook/assets/image%20%2821%29.png)

## **处理多个数据集** <a id="4ba93c33-dd39-468b-8b3e-96c938bbd024"></a>

您的任务可能需要一个更复杂的课程：也许是对[ImageNet](http://www.image-net.org/)中的一部分类进行预训练，并对自定义数据集进行微调，比如[iNaturalist](https://github.com/visipedia/inat_comp/tree/master/2021)或您自己的照片集。在域适应、迁移学习、元学习和相关任务中，您可以为每个数据类型或源保存不同的工件，以保持您的实验的组织性和可复制性。

 ​[互动式探索图形→ ](https://wandb.ai/stacey/mendeleev/artifacts/balanced_data/inat_80-10-10_5K/ab79f01e007113280018/graph)

![](../.gitbook/assets/image%20%2818%29.png)

多个版本的不同大小的平衡数据集：1K、5K和10K以及相应的工件图，显示了该数据上运行的训练和推断。

![](../.gitbook/assets/image%20%287%29.png)

原始数据的多个版本，共有50和500项，data\_split作业从中为“train”和“val”数据创建两个单独的对象。

### **可视化并轻松共享数据工作流**

Artifacts让您可以查看并可视化通过模型开发脚本的数据流，无论是预处理、培训、测试、分析还是任何其他工作类型：

* 为您的工件和作业选择**有意义的组织类型**：对于数据，可以是train、val、或 test；对于脚本，这可以是preprocess、train、evaluate等。您可能还希望将其他数据记录为工件：模型对固定验证数据的预测、生成输出的样本、评估度量等。
* **探索工件图形**：与代码和数据之间的所有连接进行交互（输入工件→脚本或作业→输出工件）。单击计算图上的“分解”以查看每个工件的所有版本或按作业类型查看每个脚本的所有运行。单击各个节点以查看更多详细信息（文件内容或代码、注释/元数据、配置、时间戳、父/子节点等）。
* **轻松迭代**：所有实验脚本运行，数据将自动保存和版本控制，因此您可以专注于核心的建模任务，而不必担心何时何地保存了哪个版本的数据集或代码
* **轻松共享和复制：**工件集成后，您和您的队友可以顺利地重新运行相同的工作流并从相同的数据集（默认为最新/最佳版本），甚至可以在不同的环境/不同的硬件上进行训练

 ​[互动示例→](https://wandb.ai/wandb/arttest/artifacts/model/iv3_trained/5334ab69740f9dda4fed/graph)​

简单计算机图形示例

![](../.gitbook/assets/image%20%2813%29.png)

更复杂的计算图，将预测和评估结果记录为工件

![](../.gitbook/assets/image%20%2814%29.png)

计算图的简化版本，节点按工件/作业类型分组（“分解”关闭）

![](../.gitbook/assets/image%20%286%29.png)

每个节点的完整详细信息：按对象类型列出的版本和按作业类型运行的脚本（“分解”开启）

![](../.gitbook/assets/image%20%288%29.png)

resnet18的特定版本的细节：哪个训练运行产生了它，哪个进一步运行为进行推断加载了它。以上在每个项目中都有深度链接，因此您可以浏览完整的图表。

![](../.gitbook/assets/image%20%285%29.png)



