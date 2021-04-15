# Artifacts FAQs

###  **如何以编程的方式更新工件？**

 您可以直接从您的脚本更新多个工件属性（如描述、元数据和别名），只需简单地将它们设置为需要的值，然后调用`.save()`：

```python
api = wandb.Api()
artifact = api.artifact('bike-dataset:latest')

# Update the description
artifact.description = "My new description"

# Selectively update metadata keys
artifact.metadata["oldKey"] = "new value"

# Replace the metadata entirely
artifact.metadata = {"newKey": "new value"}

# Add an alias
artifact.aliases.append('best')

# Remove an alias
artifact.aliases.remove('latest')

# Completely replace the aliases
artifact.aliases = ['replaced']

# Persist all artifact modifications
artifact.save()
```

##  **如何将工件记录到现有运行？**

有时，您可能希望将工件标记为以前记录的运行的输出。在这种情况下，您可以重新初始化旧运行并将新工件记录到其中，如下所示：

```python
with wandb.init(id="existing_run_id", resume="allow") as run:
    artifact = wandb.Artifact("artifact_name", "artifact_type")
    artifact.add_file("my_data/file.txt")
    run.log_artifact(artifact)
```

## **如何在不启动运行的情况下记录一个工件？**

您可以使用方便的wandb artifact put命令记录工件，无需编写脚本处理上传：

```bash
$ wandb artifact put -n project/artifact -t dataset mnist/
```

同样，您可以使用以下命令将工件下载到目录：

```bash
$ wandb artifact get project/artifact:alias --root mnist/
```

## **如何在运行之外下载工件？**

您可以使用方便的`wandb artifact get`命令下载工件：

```bash
$ wandb artifact get project/artifact:alias --root mnist/
```

或者，您可以编写一个使用公共API的脚本：

```python
api = wandb.Api()

artifact = api.artifact('data:v0')
artifact_dir = artifact.checkout()
```

## **如何清理未使用的工件版本？**

 随着工件随时间的发展，您最终可能有一大堆版本挤在UI里，占用大量的存储空间。尤其是如果您将工件用于模型检查点时，其中只有工件的最新版本（标记为latest的版本）才是有用的。下面介绍如何删除没有任何别名的工件的所有版本：

```python
api = wandb.Api()

artifact_type, artifact_name = ... # fill in the desired type + name
for version in api.artifact_versions(artifact_type, artifact_name):
    # Clean up all versions that don't have an alias such as 'latest'.
		#
		# NOTE: You can put whatever deletion logic you want here.
    if len(version.aliases) == 0:
        version.delete()
```

## **如何遍历工件图？**

W&B会自动追踪给定运行记录的工件以及给定运行使用的工件。您可以以编程的方式通过API查看图像：

```python
api = wandb.Api()

artifact = api.artifact('data:v0')

# Walk up and down the graph from an artifact:
producer_run = artifact.logged_by()
consumer_runs = artifact.used_by()

# Walk up and down the graph from a run:
logged_artifacts = run.logged_artifacts()
used_artifacts = run.used_artifacts()
```

## **如何清理我的本地工件缓存？**

为提高下载共享许多相同文件的各个版本的速度，W&B会缓存工件文件。但是，随着时间的推移，这个缓存目录可能会变得很大。您可以运行以下命令清理缓存，清理最近未使用的任何文件：

```text
$ wandb artifact cache cleanup 1GB
```

运行上述命令将缓存大小限制为1GB，并根据上次访问文件的时间来确定要删除的文件的优先级。

## **如何用分布式流程记录一个工件？**

对于大型数据集或分布式训练，可能需要多个并行运行来支持单个工件。您可以使用以下模式来构造这样的并行工件：

```python
import wandb
import time

# We will use ray to launch our runs in parallel
# for demonstration purposes. You can orchestrate
# your parallel runs however you want.
import ray

ray.init()

artifact_type = "dataset"
artifact_name = "parallel-artifact"
table_name = "distributed_table"
parts_path = "parts"
num_parallel = 5

# Each batch of parallel writers should have its own
# unique group name.
group_name = "writer-group-{}".format(round(time.time()))

@ray.remote
def train(i):
  """
  Our writer job. Each writer will add one image to the artifact.
  """
  with wandb.init(group=group_name) as run:
    artifact = wandb.Artifact(name=artifact_name, type=artifact_type)
    
    # Add data to a wandb table. In this case we use example data
    table = wandb.Table(columns=["a", "b", "c"], data=[[i, i*2, 2**i]])
    
    # Add the table to folder in the artifact
    artifact.add(table, "{}/table_{}".format(parts_path, i))
    
    # Upserting the artifact creates or appends data to the artifact
    run.upsert_artifact(artifact)
  
# Launch your runs in parallel
result_ids = [train.remote(i) for i in range(num_parallel)]

# Join on all the writers to make sure their files have
# been added before finishing the artifact. 
ray.get(result_ids)

# Once all the writers are done writing, finish the artifact
# to mark it ready.
with wandb.init(group=group_name) as run:
  artifact = wandb.Artifact(artifact_name, type=artifact_type)
  
  # Create a "PartitionTable" pointing to the folder of tables
  # and add it to the artifact.
  artifact.add(wandb.data_types.PartitionedTable(parts_path), table_name)
  
  # Finish artifact finalizes the artifact, disallowing future "upserts"
  # to this version.
  run.finish_artifact(artifact)
```

## **如何在扫描中找出最佳运行的工件？**

在扫描中，您可以让各个运行各自发出自己的工件，而不是让所有运行生成同一工件的版本。使用此模式，您可以使用以下代码来检索与扫描中性能最佳的运行相关联的工件：

```python
import wandb
api = wandb.Api()
sweep = api.sweep("<entity>/<project>/<sweep_id>")
runs = sorted(sweep.runs, key=lambda run: run.summary.get("val_acc", 0), reverse=True)
best_run = runs[0]
for artifact in best_run.logged_artifacts():
  artifact_path = artifact.download()
  print(artifact_path)
```

## **如何保存代码？‌**‌

 在`wandb.init`中使用`save_code=True`可保存要启动运行的主脚本或笔记本。若要将所有代码保存到运行，请使 Artifacts 件对代码进行版本控制。以下为示例

```python
code_artifact = wandb.Artifact(type='code')
code_artifact.add_dir('.')
wandb.log_artifact(code_artifact)
```

## **工件文件存储在哪里？**

 默认情况下，W&B将工件文件存储在位于美国的私人Google云存储桶中。静止和传输中的全部文件都经过加密。

对于敏感文件，我们建议使用私有W&B安装或使用引用工件。

## **工件文件什么时候会被删掉？**

W&B存储工件文件的方式最大限度见笑了连续工件版本之间的重复内容。这意味着，如果您记录一个包含1,000个图像的数据集，然后记录一个仅添加100个图像的后续版本，则第二个版本的大小仅等于引入的文件大小。

删除工件版本时，W&B会检查哪些文件可以完全安全地删除。换句话说，它可以保证文件没有被先前或后续的工件版本使用。如果删除是安全的，文件将立即删除，不会在我们的服务器上留下任何痕迹。

## **谁有权访问我的工件？**

工件会继承其父项目的访问权限：

* 如果项目是私有的，那么只有项目团队的成员才能访问其工件。
* 对于公共项目，所有用户都有工件的读取权限，但只有项目团队的成员才能进行创建或修改。
* 对于开放项目，所有用户都有工件的读写权限。

