---
description: 我们的公共API的最佳实践和常见用例——用于导出数据和更新现有的运行
---

# Import/Export API Guide

使用公共API导出或更新已保存到W＆B的数据。在使用API之前，您需要记录脚本中的数据——有关详情，请参见[快速入门](https://docs.wandb.ai/v/zh-hans/quickstart)。

 **公共API的用例**

* **导出数据：**下拉一个数据帧，在Jupyter Notebook中进行自定义分析。研究完数据之后，您可以通过创建一个新的分析运行和记录结果来同步您的发现，例如：`wandb.init(job_type="analysis")`
* **更新现有的运行：**您可以更新与W＆B运行相关联的记录数据。例如，您可能想要更新一组运行的配置，来纳入架构或最初时没有记录的超参数等额外信息。

有关可用函数的详细信息，请参阅[生成的参考文档](https://docs.wandb.ai/ref/public-api)。

### **验证**

 通过以下两种方式之一，使用[API密钥](https://wandb.ai/authorize)对计算机进行身份验证：

1. 在命令行上运行**wandb login**并贴入您的API密钥。
2.  设置**WANDB\_API\_KEY**环境变量为您的API密钥。

### **导出运行数据**

从已完成或活动的运行中下载数据。常见的用法包括下载数据帧，在jupiter笔记本中进行自定义分析，或者在自动化环境中使用自定义逻辑。

```python
import wandb
api = wandb.Api()
run = api.run("<entity>/<project>/<run_id>")
```

运行对象的最常用的属性是：

| 属性 | 定义 |
| :--- | :--- |
| run.config | 用于模型输入（例如超参数）的字典 |
| run.history\(\) | 字典列表，用于存储模型训练时发生变化的值，例如损失。wandb.log\(\)命令追加到该对象。 |
| run.summary | 输出内容的字典。可以是精度和损失等标量，也可以是大文件。默认情况下，wandb.log\(\)将汇总设置为已记录时间序列的最终值。这也可以直接设置。 |

您还可以修改或更新过去运行的数据。默认情况下，api对象的单个实例将缓存所有的网络请求。如果您的用例需要提供在运行脚本中的实时信息，则可以调用api.flush\(\)来获取更新的值。

### **采样**

默认的历史方法将指标抽样到固定数量的样本中（默认是500，您也可以使用样本参数来更改此值）。如果要一次大批量导出所有数据，则可以使用run.scan\_history\(\)方法。更多详情，请参见API参考。

### **查询多次运行**

{% tabs %}
{% tab title="数据帧和CSV" %}
该示例脚本查找一个项目，然后输出运行的CSV文件，其中包含了名称、配置和汇总统计信息。

```python
import wandb
api = wandb.Api()

# Change oreilly-class/cifar to <entity/project-name>
runs = api.runs("<entity>/<project>")
summary_list = [] 
config_list = [] 
name_list = [] 
for run in runs: 
    # run.summary are the output key/values like accuracy.  We call ._json_dict to omit large files 
    summary_list.append(run.summary._json_dict) 

    # run.config is the input metrics.  We remove special values that start with _.
    config_list.append({k:v for k,v in run.config.items() if not k.startswith('_')}) 

    # run.name is the name of the run.
    name_list.append(run.name)       

import pandas as pd 
summary_df = pd.DataFrame.from_records(summary_list) 
config_df = pd.DataFrame.from_records(config_list) 
name_df = pd.DataFrame({'name': name_list}) 
all_df = pd.concat([name_df, config_df,summary_df], axis=1)

all_df.to_csv("project.csv")
```
{% endtab %}

{% tab title="MongoDB Style" %}
W&B API还提供了一种在项目中使用api.runs\(\)进行跨运行查询的方法。最常见的用例是导出运行数据以进行自定义分析。查询界面与[MongoDB使用](https://docs.mongodb.com/manual/reference/operator/query)的界面相同。

```python
runs = api.runs("username/project", {"$or": [{"config.experiment_name": "foo"}, {"config.experiment_name": "bar"}]})
print("Found %i" % len(runs))
```
{% endtab %}
{% endtabs %}

 调用`api.runs（...）`返回一个可迭代的**运行**对象，其作用类似于列表。对象按需要每次依次加载50次运行，您也可以使用**per\_page**关键词参数来更改每页加载的次数。

`api.runs(...)`还接受**order（顺序）**关键词参数。默认顺序为`-created_at`，指定`+created_at`以按升序获取结果。您还可以按配置或汇总值（即`summary.val_acc`或`config.experiment_name`）进行排序。

### **错误处理**

 如果与W＆B服务器通信时发生错误，将引发`wandb.CommError`。此时可以通过**exc**属性对原始异常进行自省。

### **通过API获取最新的git commit**

在UI中，单击运行，然后单击运行页面上的“概述”选项卡以查看最新的git commit。它也在`wandb-metadata.json`文件中。使用公共API，您可以通过**run.commit**获得git哈希。

## **常见问题**

###  **导出数据，在matplotlib或seaborn中进行可视化**

查看我们的API示例，了解一些常见的导出模式。您还可以在自定义图或展开的运行表上单击下载按钮，从浏览器下载CSV。

**从脚本中获取随机的运行ID和运行名称**

在调用`wandb.init()`之后，您可以从脚本中读取随机的运行ID或人类可读的运行名称，如下所示：

*  唯一的运行ID（8个字符的哈希）: `wandb.run.id`
*  随机的运行名称（人类可读）: `wandb.run.name`

 如果您想为运行设置一些识别符，可以采用以下方法：

* **运行ID：**保留为生成的哈希。这在项目的各运行之间是独一无二的。
* **运行名称：**名称应该简短、易读，并且最好是唯一的，方便您分辨图表上不同线条之间的区别。
* **运行注解：**提供有关于您运行时做了什么的简单描述。可以使用`wandb.init(notes="your notes here")`进行设置。
* **运行标签：**在运行标签中对内容进行动态跟踪，并在UI中使用过滤器对表进行过滤，只保留您关心的运行。您可以从脚本中设置标签，然后在UI中，在运行表和运行页面的“概述”选项卡中对其进行编辑。

## **公共API的例子**

### **查找运行路径**

要使用公共API，通常需要使用**运行路径**`“<entity>/<project>/<run_id”`。在应用程序中，打开一个运行，然后单击**概述**选项卡，可以查看任何运行的运行路径。  


### **从运行中读取指标** 

本示例输出用`wandb.log({"accuracy": acc})`保存的时间戳和精度，该运行保存到//。

```python
import wandb
api = wandb.Api()

run = api.run("<entity>/<project>/<run_id>")
if run.state == "finished":
   for i, row in run.history().iterrows():
      print(row["_timestamp"], row["accuracy"])
```

### **从运行中读取特定的指标**

 要从运行中提取特定的指标，请使用`keys`参数。使用`run.history()`时，默认样本数为500。不包含特定指标的已记录步骤将在输出数据帧中显示为`NaN`。`keys`参数将使api更频繁地对那些包含所列出指标键的步骤进行采样。

```python
import wandb
api = wandb.Api()

run = api.run("<entity>/<project>/<run_id>")
if run.state == "finished":
   for i, row in run.history(keys=["accuracy"]).iterrows():
      print(row["_timestamp"], row["accuracy"])
```

###  **比较两次运行**

这将输出run1和run2之间不同的配置参数。

```python
import wandb
api = wandb.Api()

# replace with your <entity_name>/<project_name>/<run_id>
run1 = api.run("<entity>/<project>/<run_id>")
run2 = api.run("<entity>/<project>/<run_id>")

import pandas as pd
df = pd.DataFrame([run1.config, run2.config]).transpose()

df.columns = [run1.name, run2.name]
print(df[df[run1.name] != df[run2.name]])
```

输出：

```text
              c_10_sgd_0.025_0.01_long_switch base_adam_4_conv_2fc
batch_size                                 32                   16
n_conv_layers                               5                    4
optimizer                             rmsprop                 adam
```

###  **完成运行后，更新运行的指标**

本示例将前一次运行的精度设置为0.9。它还将前一次运行的精度直方图修改为numpy\_array的直方图。

```python
import wandb
api = wandb.Api()

run = api.run("<entity>/<project>/<run_id>")
run.summary["accuracy"] = 0.9
run.summary["accuracy_histogram"] = wandb.Histogram(numpy_array)
run.summary.update()
```

###  **更新现有运行的配置**

本示例将更新您的一个配置设置

```python
import wandb
api = wandb.Api()
run = api.run("<entity>/<project>/<run_id>")
run.config["key"] = updated_value
run.update()
```

###  **从单次运行中导出指标到CSV文件**

该脚本查找单次运行保存的所有指标，并将 其保存到CSV中。

```python
import wandb
api = wandb.Api()

# run is specified by <entity>/<project>/<run id>
run = api.run("<entity>/<project>/<run_id>")

# save the metrics for the run to a csv file
metrics_dataframe = run.history()
metrics_dataframe.to_csv("metrics.csv")
```

### **从大型单次运行中导出指标而无需采样**

默认的历史方法将指标抽样到固定数量的样本中（默认是500，您也可以使用样本参数来更改此值）。如果要一次大批量导出所有数据，则可以使用run.scan\_history\(\)方法。该脚本将所有的损失指标加载到可变损失中，以实现更长的运行时间。

```python
import wandb
api = wandb.Api()

run = api.run("<entity>/<project>/<run_id>")
history = run.scan_history()
losses = [row["Loss"] for row in history]
```

### **将项目中所有运行的指标导出到CSV文件**

该脚本查找一个项目，然后输出运行的CSV文件，其中包含了名称、配置和汇总统计信息。

```python
import wandb
api = wandb.Api()

runs = api.runs("<entity>/<project>")
summary_list = [] 
config_list = [] 
name_list = [] 
for run in runs: 
    # run.summary are the output key/values like accuracy.  We call ._json_dict to omit large files 
    summary_list.append(run.summary._json_dict) 

    # run.config is the input metrics.  We remove special values that start with _.
    config_list.append({k:v for k,v in run.config.items() if not k.startswith('_')}) 

    # run.name is the name of the run.
    name_list.append(run.name)       

import pandas as pd 
summary_df = pd.DataFrame.from_records(summary_list) 
config_df = pd.DataFrame.from_records(config_list) 
name_df = pd.DataFrame({'name': name_list}) 
all_df = pd.concat([name_df, config_df,summary_df], axis=1)

all_df.to_csv("project.csv")
```

###  **上传文件到已完成的运行**

下面的代码片段将所选的文件上载到一个已完成的运行中。

```python
import wandb
api = wandb.Api()
run = api.run("entity/project/run_id")
run.upload_file("file_name.extension")
```

### **从运行中下载文件**

此处在cifar项目中找到与运行ID uxte44z7关联的文件“model-best.h5”，并将其保存在本地

```python
import wandb
api = wandb.Api()
run = api.run("<entity>/<project>/<run_id>")
run.file("model-best.h5").download()
```

### **从运行中下载所有文件**

此处查找与运行ID uxte44z7关联的所有文件，并将其保存在本地。（注意：您也可以通过从命令行运行wandb restore 来完成此操作。）

```python
import wandb
api = wandb.Api()
run = api.run("<entity>/<project>/<run_id>")
for file in run.files():
    file.download()
```

### **下载最佳模型文件**

```python
import wandb
api = wandb.Api()
sweep = api.sweep("<entity>/<project>/<sweep_id>")
runs = sorted(sweep.runs, key=lambda run: run.summary.get("val_acc", 0), reverse=True)
val_acc = runs[0].summary.get("val_acc", 0)
print(f"Best run {runs[0].name} with {val_acc}% validation accuracy")
runs[0].file("model-best.h5").download(replace=True)
print("Best model saved to model-best.h5")
```

**从运行中删除所有具有给定扩展名的文件**

```python
import wandb

api = wandb.Api()
run = api.run("<entity>/<project>/<run_id>")

files = run.files()
for file in files:
	if file.name.endswith(".png"):
		file.delete()
```

###  **从特定扫描中获取运行**

```python
import wandb
api = wandb.Api()
sweep = api.sweep("<entity>/<project>/<sweep_id>")
print(sweep.runs)
```

### **下载系统指标数据**

这里为您提供了一个运行时包含所有系统指标的数据帧。

```python
import wandb
api = wandb.Api()
run = api.run("<entity>/<project>/<run_id>")
system_metrics = run.history(stream = 'events')
```

### **更新汇总指标**

您可以通过字典来更新汇总指标。

```python
summary.update({“key”: val})
```

### **获取运行run的命令**

每次运行都会在运行概述页面上捕获启动它的命令。要从API下拉此命令，您可以运行：

```python
api = wandb.Api()
run = api.run("username/project/run_id")
meta = json.load(run.file("wandb-metadata.json").download())
program = ["python"] + [meta["program"]] + meta["args"]
```

### **从历史中获取分页数据**

如果在我们的后端获取指标很慢，或者API请求超时，您可以尝试降低`scan_history`中的页面大小，以使单个请求不会超时。默认的页面大小为1000，因此您可以尝试不同的大小，看看哪种效果最好：

```python
api = wandb.Api()
run = api.run("username/project/run_id")
run.scan_history(keys=sorted(cols), page_size=100)
```

