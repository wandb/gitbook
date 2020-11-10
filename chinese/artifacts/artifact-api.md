# Artifact API

使用W＆B Artifacts进行数据集跟踪和模型版本控制。 初始化运行，创建artifact，然后在工作流程的另一部分中使用它。 您可以使用artifact来跟踪和保存文件，或者跟踪外部URI。

从`wandb` 0.9.0版开始，此功能在客户端中可用。

## 1. **初始化运行**

要跟踪pipeline的步骤，请在脚本中初始化运行。 为job\_type指定一个字符串以区分不同的pipeline步骤——预处理，训练，评估等。如果您从未使用W＆B进行运行测试，我们的[Python库文档](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MKaPhwzNIegNuInaekR/chinese/untitled)中提供了关于实验跟踪的更详细的指导。

```python
run = wandb.init(job_type='train')
```

## 2. **创建一个artifact**

artifact就像数据文件夹，其内容是存储在artifact中的实际文件或对外部URI的引用。要创建artifact，请将其记录为运行的输出。为**type**指定一个字符串，以区分不同的artifact（数据集，模型，结果等）。为该artifact指定一个**name**，例如`bike-dataset`，以帮助您记住artifact内部的内容。在pipeline的后续步骤中，您可以使用此名称以及类似`bike-dataset`：v1的版本来下载此artifact。

当您调用log\_artifact时，我们检查artifact的内容是否已更改，如果已更改，我们将自动创建artifact的新版本：v0，v1，v2等。

**wandb.Artifact\(\)**

* **type \(str\)**: 用于不同目的的不同artifact。我们建议仅使用于“数据集”，“模型”和“结果”。
* **name \(str\)**: 给artifact一个唯一的名称，在其他地方引用该artifact时使用。您可以在名称中使用数字，字母，下划线，连字符和点。
* **description \(str,** 选**\)**: UI中artifact版本旁边显示的自由文本
* **metadata \(**字典，可选**\)**: 与artifact关联的结构化数据，例如数据集的类分布。当我们构建Web界面时，您将能够使用此数据来查询和绘制图表。

```python
artifact = wandb.Artifact('bike-dataset', type='dataset')

# Add a file to the artifact's contents
artifact.add_file('bicycle-data.h5')

# Save the artifact version to W&B and mark it as the output of this run
run.log_artifact(artifact)
```

## 3. **Artifact的使用**

您可以将artifact用作运行的输入。 例如，我们可以获取`bike-dataset`：v0，即`bike-dataset`的第一个版本，并在pipeline的下一个脚本中使用它。 调用use\_artifact时，脚本将查询W＆B以查找该命名的artifact并将其标记为运行的输入。

```python
# Query W&B for an artifact and mark it as input to this run
artifact = run.use_artifact('bike-dataset:v0')

# Download the artifact's contents
artifact_dir = artifact.download()
```

**使用来自其他项目的artifact**

通过使用项目加上artifact名称，你可以自由引用您有权访问的任何项目中的artifact。 您还可以通过使用artifact的实体名称进一步限定artifact的名称来跨实体引用artifact。

```python
# Query W&B for an artifact from another project and mark it
# as an input to this run.
artifact = run.use_artifact('my-project/bike-model:v0')

# Use an artifact from another entity and mark it as an input
# to this run.
artifact = run.use_artifact('my-entity/my-project/bike-model:v0')
```

**使用尚未记录的artifact**  
您还可以构造artifact对象并将其传递给use\_artifact。 我们检查artifact是否在W＆B中存在，如果不存在，则创建一个新artifact。 这是幂等的——您可以根据需要多次将artifact传递给use\_artifact，只要内容保持不变，我们就会对其进行重复数据删除。

```python
artifact = wandb.Artifact('bike-model', type='model')
artifact.add_file('model.h5')
run.use_artifact(artifact)
```

### **版本和别名**

首次记录artifact时，我们将创建版本**v0**。 当您再次登录同一artifact时，我们将对内容进行校验，如果artifact已更改，我们将保存新版本**v1**。

您可以使用别名作为指向特定版本的指针。 默认情况下，run.log\_artifact将**最新**的别名添加到记录的版本中。

您可以使用别名来获取artifact。 例如，如果您希望训练脚本始终提取数据集的**最新**版本，则在使用该artifact时指定最新。

```python
artifact = run.use_artifact('bike-dataset:latest')
```

您还可以将自定义别名应用于artifact版本。 例如，如果要标记哪个模型检查点在度量标准AP-50上最好，则可以在记录模型artifact时添加字符串best-ap50作为别名

```python
artifact = wandb.Artifact('run-3nq3ctyy-bike-model', type='model')
artifact.add_file('model.h5')
run.log_artifact(artifact, aliases=['latest','best-ap50'])
```

### **构建artifact**

一个artifact就像数据文件夹。 每个条目要么是存储在artifact中的实际文件，要么是对外部URI的引用。 您可以将文件夹嵌套在artifact中，就像常规文件系统一样。 通过初始化`wandb.Artifact（）`类来构造新的artifact。

您可以将以下字段传递给`Artifact（）`构造函数，或直接在artifact对象上设置它们

* **type:** 应为“数据集”，“模型”或“结果
* **description**: 将在用户界面中显示的自由格式文本。
* **metadata**: 可以包含任何结构化数据的字典。 您将可以使用此数据查询和绘图。 例如。 您可以选择将数据集artifact的类分布存储为元数据。

```python
artifact = wandb.Artifact('bike-dataset', type='dataset')
```

使用**name**指定可选的文件名，如果要添加目录，则使用文件路径前缀。

```python
# Add a single file
artifact.add_file(path, name='optional-name')

# Recursively add a directory
artifact.add_dir(path, name='optional-prefix')

# Return a writeable file-like object, stored as <name> in the artifact
with artifact.new_file(name) as f:
    ...  # Write contents into the file 

# Add a URI reference
artifact.add_reference(uri, name='optional-name')
```

#### **添加文件和目录**

对于以下示例，假设我们有一个包含这些文件的项目目录：

```text
project-directory
|-- images
|   |-- cat.png
|   +-- dog.png
|-- checkpoints
|   +-- model.h5
+-- model.h5
```

<table>
  <thead>
    <tr>
      <th style="text-align:left">API call</th>
      <th style="text-align:left">Resulting artifact contents</th>
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

**添加参考**

```python
artifact.add_reference(uri, name=None, checksum=True)
```

* **uri（字符串）：**要跟踪的参考URI。
* **name（字符串）**：可选的名称覆盖。如果未提供，则从uri推断名称。
* **checksum（布尔）**：如果为true，则引用从uri收集校验和\(checksum\)信息和元数据以进行验证。

您可以将对外部URI的引用添加到artifact中，而不是实际文件中。如果URI具有wandb知道如何处理的方案，则artifact将跟踪校验和和其他信息以确保可重复性。当前artifact支持以下URI方案：

* `http(s)://`: 可通过HTTP访问的文件的路径。如果HTTP服务器支持`ETag`和`Content-Length`响应标头，则artifact将以`etags`和大小元数据的形式跟踪校验和
* `s3://`: S3中对象或对象前缀的路径。该artifact将跟踪所引用对象的校验和和版本信息（如果存储桶启用了对象版本控制）。对象前缀被扩展为包括该前缀下的对象，最多10,000个对象。
* `gs://`: A path to an object or object prefix in GCS. The artifact will track checksums and versioning information \(if the bucket has object versioning enabled\) for the referenced objects. Object prefixes are expanded to include the objects under the prefix, up to a maximum of 10,000 objects.

对于以下示例，假设我们有一个包含这些文件的S3存储桶：

```text
s3://my-bucket
|-- images
|   |-- cat.png
|   +-- dog.png
|-- checkpoints
|   +-- model.h5
+-- model.h5
```

<table>
  <thead>
    <tr>
      <th style="text-align:left">API call</th>
      <th style="text-align:left">Resulting artifact contents</th>
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

### **使用和下载artifact**

```python
run.use_artifact(artifact=None)
```

* 将artifact为运行的输入。

有两种使用artifact的模式。您可以使用显式存储在W＆B中的artifact名称，也可以构造一个artifact对象并将其传递以根据需要进行重复数据删除。

#### **使用存储在W＆B中的artifact**

```python
artifact = run.use_artifact('bike-dataset:latest')
```

您可以在返回的artifact上调用以下方法：

```python
datadir = artifact.download(root=None)
```

* 下载当前不存在的所有artifact内容。这将返回包含artifact内容的目录的路径。您可以通过设置root显式指定下载目标。

```python
path = artifact.get_path(name)
```

* 仅获取路径名下的文件。使用以下方法返回`Entry`对象：
  * **Entry.download\(\)**:从artifact上的路径名下载文件
  * **Entry.ref\(\)**: 如果使用`add_reference`将条目存储为引用，则返回URI

W＆B能够处理的参考可以像artifact文件一样下载。Consumer API是相同的。

**构造和使用artifact**

您还可以构造artifact对象并将其传递给use\_artifact。如果尚不存在，这将在W＆B中创建artifact。这是幂等的，因此您可以随意进行多次。只要`model.h5`的内容保持不变，artifact就只能创建一次。

```python
artifact = wandb.Artifact('reference model')
artifact.add_file('model.h5')
run.use_artifact(artifact)
```

**在运行外下载artifact**

```python
api = wandb.Api()
artifact = api.artifact('entity/project/artifact:alias')
artifact.download()
```

### **更新artifact**

您可以设置artifact的`description`，`metadata`和`aliases`为所需的值，然后调用`save()`来更新它们。

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

### **数据私隐**

Artifact使用安全的API级别的访问控制。 文件在静止和传输过程中都被加密。 artifact也可以跟踪对专用存储桶的引用，而无需将文件内容发送到W＆B。 有关替代方案，请通过contact@wandb.com与我们联系，以讨论私有云和本地安装。

