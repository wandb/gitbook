---
description: 使用单个运行、协作使用分布式编写器或作为先前版本的补丁来创建新的工件版本。
---

# Artifact Creation Modes

可以通过以下三种方式中的任意一种创建工件的新版本：

* **简单：**可提供新版本所有数据的单个运行。这是最常见的情况，最适合在运行完全重新创建所需数据时使用。例如：输出表中保存的模型或模型预测以进行分析。
* **协作：**可协作提供新版本所有数据的一组运行。最适合具有多次运行生成数据的分布式作业，通常是并行的。例如：以分布式方式评估模型并输出预测。
* **补丁：**（即将推出）提供了要应用的差异的补丁的单次运行。当运行希望向工件添加数据而不需要重新创建所有已有的数据时，这是最合适的。例如：您有一个通过运行每日Web Scraper创建的黄金数据集——在这种情况下，您希望运行将新数据附加到数据集。

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-MWjc5J2TbLRNA1e_SG8%2F-MWjeGQp-vT_qprRLrbT%2FArtifact%20Version%20Types%20%282%29.png?alt=media&token=0e6b55e7-2c7a-4554-b1df-d3a9f238f12f)

### **简单模式** <a id="simple-mode"></a>

若要使用生成工件中所有文件的单次运行来记录工件的新版本，请使用简单模式：

```text
with wandb.init() as run:	artifact = wandb.Artifact("artifact_name", "artifact_type")	# Add Files and Assets to the artifact using 	# `.add`, `.add_file`, `.add_dir`, and `.add_reference`	artifact.add_file("image1.png")	run.log_artifact(artifact)
```

此模式非常常见，因此我们提供了一种创建版本的方法，无需使用`Artifact.save()`显式启动运行：

```text
artifact = wandb.Artifact("artifact_name", "artifact_type")# Add Files and Assets to the artifact using # `.add`, `.add_file`, `.add_dir`, and `.add_reference`artifact.add_file("image1.png")artifact.save()
```

### **协作模式** <a id="collaborative-mode"></a>

若要允许运行集合在提交版本之前对版本进行协作，请使用协作模式。在使用协作模式时，需要记住两个关键：

1. 集合中的每个运行都需要知道相同的唯一ID（称为`distributed_id`）才能在同一版本上进行协作。作为默认设置，如果存在，我们将使用由`wandb.init(group=GROUP)`设置的`group`作为`distributed_id`。
2. 必须有一个最终运行来“提交”版本，并永久锁定其状态。

请考虑以下示例。请注意，我们不使用`log_artifact`而是使用`upsert_artifact`添加协作工件，使用`finish_artifact`来完成提交。

 **运行1：**

```text
with wandb.init() as run:	artifact = wandb.Artifact("artifact_name", "artifact_type")	# Add Files and Assets to the artifact using 	# `.add`, `.add_file`, `.add_dir`, and `.add_reference`	artifact.add_file("image1.png")	run.upsert_artifact(artifact, distributed_id="my_dist_artifact")
```

 **运行2**

```text
with wandb.init() as run:	artifact = wandb.Artifact("artifact_name", "artifact_type")	# Add Files and Assets to the artifact using 	# `.add`, `.add_file`, `.add_dir`, and `.add_reference`	artifact.add_file("image2.png")	run.upsert_artifact(artifact, distributed_id="my_dist_artifact")
```

**运行3：**必须在运行1和运行2完成后运行。这个会调用`finish_artifact`的运行在工件中很受欢迎，但其实并不需要。

```text
with wandb.init() as run:	artifact = wandb.Artifact("artifact_name", "artifact_type")	# Add Files and Assets to the artifact using 	# `.add`, `.add_file`, `.add_dir`, and `.add_reference`	artifact.add_file("image3.png")	run.finish_artifact(artifact, distributed_id="my_dist_artifact")
```

###  **补丁模式（即将推出）** <a id="patch-mode-coming-soon"></a>

若要通过修改以前的版本来创建工件的新版本，请使用补丁模式。一旦可用，将提供补丁模式的代码片段。

## **疑难解答** <a id="faq"></a>

### **如何在协作模式下记录表格？** <a id="how-do-i-log-a-table-in-collaborative-mode"></a>

对于大型数据集可能需要多个并行运行来支持单个表格。您可以使用以下模式来构造这样的并行工件。关键是每个工作者都要将他们自己的表格放在工件的目录中。然后，最后一个工作人员将PartitionTable添加到指向“parts”文件夹的工件。

```text
import wandbimport time​# We will use ray to launch our runs in parallel# for demonstration purposes. You can orchestrate# your parallel runs however you want.import ray​ray.init()​artifact_type = "dataset"artifact_name = "parallel-artifact"table_name = "distributed_table"parts_path = "parts"num_parallel = 5​# Each batch of parallel writers should have its own# unique group name.group_name = "writer-group-{}".format(round(time.time()))​@ray.remotedef train(i):  """  Our writer job. Each writer will add one image to the artifact.  """  with wandb.init(group=group_name) as run:    artifact = wandb.Artifact(name=artifact_name, type=artifact_type)        # Add data to a wandb table. In this case we use example data    table = wandb.Table(columns=["a", "b", "c"], data=[[i, i*2, 2**i]])        # Add the table to folder in the artifact    artifact.add(table, "{}/table_{}".format(parts_path, i))        # Upserting the artifact creates or appends data to the artifact    run.upsert_artifact(artifact)  # Launch your runs in parallelresult_ids = [train.remote(i) for i in range(num_parallel)]​# Join on all the writers to make sure their files have# been added before finishing the artifact. ray.get(result_ids)​# Once all the writers are done writing, finish the artifact# to mark it ready.with wandb.init(group=group_name) as run:  artifact = wandb.Artifact(artifact_name, type=artifact_type)    # Create a "PartitionTable" pointing to the folder of tables  # and add it to the artifact.  artifact.add(wandb.data_types.PartitionedTable(parts_path), table_name)    # Finish artifact finalizes the artifact, disallowing future "upserts"  # to this version.  run.finish_artifact(artifact)
```

[  
](https://docs.wandb.ai/artifacts/model-versioning)

