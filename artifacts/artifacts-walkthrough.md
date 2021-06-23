# Artifacts Walkthrough

使用W&B Artifacts进行数据集和模型版本控制。初始化一个运行，创建一个工件，然后在工作流的另一部分使用。您可以使用对象追踪和保存文件，或追踪外部URI。

0.9.0 及以上版本的`wandb`客户端支持此功能。

## 1. **初始化运行** <a id="1-initialize-a-run"></a>

若要追踪管道的一个步骤，请在脚本中初始化运行。为**job\_type**指定一个字符串，以区分不同的管道步骤预处理、训练、评估等。如果您从未使用W&B检测过运行，我们的[Python 库](https://docs.wandb.ai/v/zh-hans/library)文档中有更详细的实验追踪指导。

```text
run = wandb.init(job_type='train')
```

## 2. **创建工件** <a id="2-create-an-artifact"></a>

工件就像一个数据文件夹，其中的内容是存储在工件中的实际文件或对外部URI的引用。若要创建工件，请将其记录为运行的输出。为**type**指定一个字符串，以区分不同的工件——数据集、模型、结果等。为此工件指定一个**name**，如`bike-dataset`以帮助您记住工件内部的内容。在管道的后续步骤中，您可以使用此名称及版本（例如`bike-dataset:v1`）来下载此工件。

当调用**log\_artifact**时，我们会检查工件的内容是否发生了变化，如果发生了变化，我们会自动创建此工件的新版本：v0、v1、v2等。

**wandb.Artifact\(\)**

* **type \(str\)**: 区分工件的种类，用于组织目的。我们建议使用“数据集”、“模型”和“结果”。
* **name \(str\)**: 为您的工件指定一个唯一的名称，在其他地方引用工件时使用。名称中可以使用数字、字母、下划线、连字符和点。
* **description \(str, optional\)**: 在UI中显示在工件版本旁边的自由文本
* **metadata \(dict, optional\)**: 与工件相关的结构化数据，例如数据集的类分布。在我们构建Web界面时，您将能够使用这些数据来查询和绘图。

```text
artifact = wandb.Artifact('bike-dataset', type='dataset')​# Add a file to the artifact's contentsartifact.add_file('bicycle-data.h5')​# Save the artifact version to W&B and mark it as the output of this runrun.log_artifact(artifact)
```

**注意：**对于performant上传，将异步执行对`log_artifact`的调用。在循环中记录工件时，这可能会导致出人意料的结果。例如：

```text
for i in range(10):    a = wandb.Artifact('race', type='dataset', metadata={        "index": i,    })    # ... add files to artifact a ...    run.log_artifact(a)
```

工件版本**v0**不能保证在其元数据中具有索引0，因为工件可以以任意顺序记录。

## 3. **使用工件** <a id="3-use-an-artifact"></a>

您可以使用工件作为运行的输入。例如，我们可以使用`bike-dataset`的第一个版本`bike-dataset:v0`，并在我们管道中的下一个脚本中使用。当您调用**use\_artifact**时，您的脚本会查询W&B找出命名的工件，并将其标记为运行的输入。

```text
# Query W&B for an artifact and mark it as input to this runartifact = run.use_artifact('bike-dataset:v0')​# Download the artifact's contentsartifact_dir = artifact.download()
```

**使用来自不同项目的工件**您可以通过使用其项目名称限定工件的名称来自由地引用您有权访问的任何项目的工件。您还可以通过使用其实体名称进一步限定工件的名称来跨实体引用工件。

```text
# Query W&B for an artifact from another project and mark it# as an input to this run.artifact = run.use_artifact('my-project/bike-model:v0')​# Use an artifact from another entity and mark it as an input# to this run.artifact = run.use_artifact('my-entity/my-project/bike-model:v0')
```

**使用尚未记录的工件**您还可以构造一个工件对象并将其传递给**use\_artifact**。我们检查工件是否已经存在于W&B中，如果不存在，我们会创建一个新的工件。这是幂等的——您可以把一个工件传递给use\_artifact多次，只要内容保持不变，我们就会对它进行重复数据消除。

```text
artifact = wandb.Artifact('bike-model', type='model')artifact.add_file('model.h5')run.use_artifact(artifact)
```

## **版本和别名** <a id="versions-and-aliases"></a>

当您第一次记录工件时，我们会创建版本**v0**。当您再次登录到相同的工件时，我们校验内容，如果工件发生变化，我们将保存一个新版本**v1**。

您可以使用别名作为指向特定版本的指针。默认情况下，run.log\_artifact会将**最新的**别名添加到记录的版本中。

您可以使用别名获取工件。例如，如果您希望您的训练脚本始终获取数据集的**最新**版本，请在使用该工件时指定**最新**版本。

```text
artifact = run.use_artifact('bike-dataset:latest')
```

您也可以将自定义别名应用于工件版本。例如，如果要标记哪个模型检查点是度量AP-50上最好的，则可以在记录模型工件时添加字符串**best-ap50**作为别名。

```text
artifact = wandb.Artifact('run-3nq3ctyy-bike-model', type='model')artifact.add_file('model.h5')run.log_artifact(artifact, aliases=['latest','best-ap50'])
```

## **构造工件** <a id="constructing-artifacts"></a>

工件就像一个数据文件夹。每个条目都是存储在工件中的实际文件，或者对外部URI的引用。您可以在工件中嵌套文件夹，就像常规文件系统一样。通过初始化`wandb.Artifact()`类来构造新的工件。

您可以将以下字段传递给`Artifact()`构造函数，或者直接在工件对象上设置**:**

* **type:** 自由形式的字符串，如“dataset”、“model”或“result”
* **description**: 将在UI中显示的自由形式文本。
* **metadata**: 一个可以包含任何结构化数据的目录。您将能够使用这些数据进行查询和绘图。例如，您可以选择将数据集工件的类分布存储为元数据。

```text
artifact = wandb.Artifact('bike-dataset', type='dataset')
```

使用**name**指定可选的文件名，或使用文件路径前缀添加目录。

```text
# Add a single fileartifact.add_file(path, name='optional-name')​# Recursively add a directoryartifact.add_dir(path, name='optional-prefix')​# Return a writeable file-like object, stored as <name> in the artifactwith artifact.new_file(name) as f:    ...  # Write contents into the file ​# Add a URI referenceartifact.add_reference(uri, name='optional-name')
```

### **添加文件和目录** <a id="adding-files-and-directories"></a>

对于以下示例，假设我们有一个包含这些文件的项目目录：

```text
project-directory|-- images|   |-- cat.png|   +-- dog.png|-- checkpoints|   +-- model.h5+-- model.h5
```

<table>
  <thead>
    <tr>
      <th style="text-align:left">API &#x8C03;&#x7528;</th>
      <th style="text-align:left">&#x751F;&#x6210;&#x7684;&#x5DE5;&#x4EF6;&#x5185;&#x5BB9;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">artifact.add_file(&apos;model.h5&apos;)</td>
      <td style="text-align:left">model.h5</td>
    </tr>
    <tr>
      <td style="text-align:left">artifact.add_file(&apos;checkpoints/model.h5&apos;)</td>
      <td style="text-align:left">model.h5</td>
    </tr>
    <tr>
      <td style="text-align:left">artifact.add_file(&apos;model.h5&apos;, name=&apos;models/mymodel.h5&apos;)</td>
      <td
      style="text-align:left">models/mymodel.h5</td>
    </tr>
    <tr>
      <td style="text-align:left">artifact.add_dir(&apos;images&apos;)</td>
      <td style="text-align:left">
        <p>cat.png</p>
        <p>dog.png</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">artifact.add_dir(&apos;images&apos;, name=&apos;images&apos;)</td>
      <td
      style="text-align:left">
        <p>images/cat.png</p>
        <p>images/dog.png</p>
        </td>
    </tr>
    <tr>
      <td style="text-align:left">artifact.new_file(&apos;hello.txt&apos;)</td>
      <td style="text-align:left">hello.txt</td>
    </tr>
  </tbody>
</table>

### **添加引用** <a id="adding-references"></a>

```text
artifact.add_reference(uri, name=None, checksum=True)
```

* **uri \(string\):** 要追踪的引用URI。
* **name \(string\):** 可选名称覆盖。如果未提供，则从URL推断名称。
* **checksum \(bool\):** 如果为true，则引用从**URL**收集校验和信息和元数据以进行验证。

您可以将对外部URI的引用添加到工件，而不是实际的文件。如果URI有一个wandb知道如何处理的方案，工件将追踪校验和和其他可再现性的信息。Artifacts当前支持以下URI方案：

* `http(s)://`:  可通过HTTP访问的文件路径。如果HTTP服务器支持`ETag`和`Content-Length`响应头，则工件将追踪etag和size元数据形式的校验和。
* `s3://`: 在S3中指向对象或对象前缀的路径。工件将追踪被引用对象的校验和和与版本信息（如果存储桶启用了工件版本控制）。对象前缀扩展为包括前缀下的对象，最多可包含10,000个对象。
* `gs://`: 工件将追踪被引用对象的校验和和与版本信息（如果存储桶启用了工件版本控制）。对象前缀扩展为包括前缀下的对象，最多可包含10,000个对象。

对于以下示例，假设我们有一个包含这些文件的S3存储桶：

```text
s3://my-bucket|-- images|   |-- cat.png|   +-- dog.png|-- checkpoints|   +-- model.h5+-- model.h5
```

<table>
  <thead>
    <tr>
      <th style="text-align:left">API &#x8C03;&#x7528;</th>
      <th style="text-align:left">&#x751F;&#x6210;&#x7684;&#x5DE5;&#x4EF6;&#x5185;&#x5BB9;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">artifact.add_reference(&apos;s3://my-bucket/model.h5&apos;)</td>
      <td style="text-align:left">model.h5</td>
    </tr>
    <tr>
      <td style="text-align:left">artifact.add_reference(&apos;s3://my-bucket/checkpoints/model.h5&apos;)</td>
      <td
      style="text-align:left">model.h5</td>
    </tr>
    <tr>
      <td style="text-align:left">artifact.add_reference(&apos;s3://my-bucket/model.h5&apos;, name=&apos;models/mymodel.h5&apos;)</td>
      <td
      style="text-align:left">models/mymodel.h5</td>
    </tr>
    <tr>
      <td style="text-align:left">artifact.add_reference(&apos;s3://my-bucket/images&apos;)</td>
      <td style="text-align:left">
        <p>cat.png</p>
        <p>dog.png</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">artifact.add_reference(&apos;s3://my-bucket/images&apos;, name=&apos;images&apos;)</td>
      <td
      style="text-align:left">
        <p>images/cat.png</p>
        <p>images/dog.png</p>
        </td>
    </tr>
  </tbody>
</table>

###  **从并行运行添加文件** <a id="adding-files-from-parallel-runs"></a>

对于大型数据集或分布式训练，可能需要多个并行运行来支持单个工件。您可以使用以下模式来构造这样的并行工件：

```text
import wandbimport time​# We will use ray to launch our runs in parallel# for demonstration purposes. You can orchestrate# your parallel runs however you want.import ray​ray.init()​artifact_type = "dataset"artifact_name = "parallel-artifact"table_name = "distributed_table"parts_path = "parts"num_parallel = 5​# Each batch of parallel writers should have its own# unique group name.group_name = "writer-group-{}".format(round(time.time()))​@ray.remotedef train(i):  """  Our writer job. Each writer will add one image to the artifact.  """  with wandb.init(group=group_name) as run:    artifact = wandb.Artifact(name=artifact_name, type=artifact_type)        # Add data to a wandb table. In this case we use example data    table = wandb.Table(columns=["a", "b", "c"], data=[[i, i*2, 2**i]])        # Add the table to folder in the artifact    artifact.add(table, "{}/table_{}".format(parts_path, i))        # Upserting the artifact creates or appends data to the artifact    run.upsert_artifact(artifact)  # Launch your runs in parallelresult_ids = [train.remote(i) for i in range(num_parallel)]​# Join on all the writers to make sure their files have# been added before finishing the artifact. ray.get(result_ids)​# Once all the writers arefinished, finish the artifact# to mark it ready.with wandb.init(group=group_name) as run:  artifact = wandb.Artifact(artifact_name, type=artifact_type)    # Create a "PartitionTable" pointing to the folder of tables  # and add it to the artifact.  artifact.add(wandb.data_types.PartitionedTable(parts_path), table_name)    # Finish artifact finalizes the artifact, disallowing future "upserts"  # to this version.  run.finish_artifact(artifact)
```

## **使用和下载工件** <a id="using-and-downloading-artifacts"></a>

```text
run.use_artifact(artifact=None)
```

* 将工件标记为运行的输入。

有两种工件使用模式。您可以使用显式存储在W&B中的工件名称，或者您可以构造一个工件对象，并在必要时将其传递给客户以进行重复数据消除。

### **使用存储在W&B中的工件** <a id="use-an-artifact-stored-in-w-and-b"></a>

```text
artifact = run.use_artifact('bike-dataset:latest')
```

您可以对返回的对象调用以下方法：

```text
datadir = artifact.download(root=None)
```

* 下载当前不存在的工件的所有内容。这将返回包含工件内容目录的路径。您可以通过设置**root**来显式指定下载目标。

```text
path = artifact.get_path(name)
```

* 仅获取路径`name`处的文件。使用以下方法返回`Entry`对象：
  * **Entry.download\(\)**: 从对象的路径`name`下载文件
  * **Entry.ref\(\)**: 如果使用`add_reference`将条目存储为引用，则返回URI

拥有W&B知道如何处理的方案的引用可以像工件文件一样下载。消费者API也是一样的。

### **构造并使用工件** <a id="construct-and-use-an-artifact"></a>

您还可以构造一个工件对象并将其传递给**use\_artifact**。这将在W&B中创建工件（如果还不存在）。这个是幂等的，所以您可以做多少次。只要`model.h5`的内容保持不变，工件只会创建一次。

```text
artifact = wandb.Artifact('reference model')artifact.add_file('model.h5')run.use_artifact(artifact)
```

### **在运行之外下载工件** <a id="download-an-artifact-outside-of-a-run"></a>

```text
api = wandb.Api()artifact = api.artifact('entity/project/artifact:alias')artifact.download()
```

##  **更新工件** <a id="updating-artifacts"></a>

您可以更新工件的描述、元数据和别名，只需将它们设置为所需的值，然后调用save\(\)。

```text
api = wandb.Api()artifact = api.artifact('bike-dataset:latest')​# Update the descriptionartifact.description = "My new description"​# Selectively update metadata keysartifact.metadata["oldKey"] = "new value"​# Replace the metadata entirelyartifact.metadata = {"newKey": "new value"}​# Add an aliasartifact.aliases.append('best')​# Remove an aliasartifact.aliases.remove('latest')​# Completely replace the aliasesartifact.aliases = ['replaced']​# Persist all artifact modificationsartifact.save()
```

## **遍历工件图** <a id="traversing-the-artifact-graph"></a>

W&B会自动追踪给定运行记录的工件以及给定运行使用的工件。您可以使用以下API浏览此图表：

```text
api = wandb.Api()​artifact = api.artifact('data:v0')​# Walk up and down the graph from an artifact:producer_run = artifact.logged_by()consumer_runs = artifact.used_by()​# Walk up and down the graph from a run:logged_artifacts = run.logged_artifacts()used_artifacts = run.used_artifacts()
```

## **清理未使用的版本** <a id="cleaning-up-unused-versions"></a>

随着工件随时间的发展，您最终可能有一大堆版本挤在UI里。尤其是如果您将工件用于模型检查点时，其中只有工件的最新版本（标记为`latest`的版本）才是有用的。W&B让您可以轻松清理这些不需要的版本：

```text
api = wandb.Api()​artifact_type, artifact_name = ... # fill in the desired type + namefor version in api.artifact_versions(artifact_type, artifact_name):    # Clean up all versions that don't have an alias such as 'latest'.    if len(version.aliases) == 0:        version.delete()
```

##  **数据隐私** <a id="data-privacy"></a>

工件使用安全的API级访问控制。静止和传输中的文件都经过加密。工件还可以追踪对私有存储桶的引用，而无需将文件内容发送到W&B。有关替代方案，请通过contact@wandb.com与我们联系，进一步讨论私有云和本地安装。

## **浏览图形** <a id="explore-the-graph"></a>

若要从工件的“图形”选项卡导航，请单击“分解”以查看每个作业类型和工件类型的所有单独实例。然后单击节点以在新选项卡中打开该运行或工件。自己在[示例图形页面](https://wandb.ai/shawn/detectron2-11/artifacts/dataset/furniture-small-val/06d5ddd4deeb2a6ebdd5/graph)试试。

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-MT1W9MeA1zoGySC16Tk%2F-MT1WHhRw1HiMpwgMC-h%2F2021-02-08%2008.40.34.gif?alt=media&token=b4312634-469a-4e50-b3fa-54377871adef)

