---
description: 每当开始一个新的运行项，就要调用wandb.init()，然后才能进入训练循环并记录模型指标。
---

# wandb.init\(\)

**概述**

调用`wandb.init()` 返回一个[**运行**](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MKaPhwzNIegNuInaekR/library/reference/wandb_api#run)对象。调用`wandb.run`同样可以获取运行对象。

一般而言，你应当在训练脚本开头调用一次`wandb.init()`。这会新建一个运行项并启动一个后台进程，后台进程将把数据同步到我们的云端。如果你想让机器离线运行并在以后上传数据，则用[离线模式](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MKaPhwzNIegNuInaekR/library/technical-faq#does-your-tool-track-or-store-training-data)。

`wandb.init()` 接受几个关键字参数：

* **name** — 运行项的显示名称，在界面中显示并且可编辑，不唯一。
* **notes** — 与运行项有关的多行字符串描述信息。
* **config** —一个字典类对象，用以设置初始配置。
* **project** — 本次运行所在项目的名称。
* **tags** — 标签，与运行项有关的一系列字符串。
* **dir** — 将把制品写入该路径.（默认值：./wandb\)
* **entity** — 发布该运行项的团队。（默认值：你的用户名或默认团队）
* **job\_type** — 所记录的任务的类型，如eval、worker、ps。（默认值：training）
* **save\_code** — 把主Python文件或笔记本文件保存至wandb，以启动diffing算法。（默认值：可在[设置](https://wandb.ai/settings)页编辑）
* **group** —  根据某一字符串把其它运行项分组；见于[分组](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MKaPhwzNIegNuInaekR/library/advanced/grouping)。
* **reinit** — 在同一个进程中是否允许对wandb.init进行多次调用。（默认值：False）
* **id** — 运行项的唯一编号，主要用于[断点续训](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MKaPhwzNIegNuInaekR/library/advanced/resuming)。必须全局唯一，并且，如果删除了某一运行项，就不能再用该id。用**name**字段给运行项取一个描述性的名称。id不能包含特殊字符。
* **resume** —  断点续训）——如果设置为True，运行项会自动断点续训；还可以是一个唯一的字符串，这样就手动断点续训；见于[断点续训](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MKaPhwzNIegNuInaekR/library/advanced/resuming)。（默认值：False）
* **anonymous** — 其值为“allow”“never”或“must”。用来打开或明确关闭匿名记录。（默认值：never）
* **force** — 运行脚本时是否强制用户登录wandb。（默认值：False）
* **magic** — （数据类型可选bool、dict或str）：把配置转化为布尔型、字典型、json字符串型、yaml文件名。若设置为True，将尝试自动执行脚本。（默认值：None）
* **sync\_tensorboard** — 布尔型，表示是否把全部TensorBoard日志复制到wandb；见于[Tensorboard](https://docs.wandb.com/library/integrations/tensorboard)（默认值：False）
* **monitor\_gym** — 布尔型，表示是否记录OpenAI Gym生成的视频；见于[Ray Tune](https://docs.wandb.com/library/integrations/ray-tune)（默认值：False）
* **allow\_val\_change** —  是否允许wandb.config改变值，默认情况下，如果config值被重写，我们就抛出异常。（默认值：False）

这些设置中的大多数还可以通过[环境变量](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MKaPhwzNIegNuInaekR/library/environment-variables)来控制。如果在集群上运行任务的话，这种方法就很有用。

运行wandb.init\(\)的脚本，我们会自动保存一份脚本副本。可在[代码比较器](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MKaPhwzNIegNuInaekR/app/features/panels/code)详细了解代码比较功能。如果要关闭该功能，就设置环境变量WANDB\_DISABLE\_CODE=True

## **常见问题**

**如何在一个脚本中启动多个运行项？**

If you're trying to start multiple runs from one script, add two things to your code:

1. run = wandb.init\(**reinit=True**\):这样设置就可以重新初始化运行项；
2. **run.finish\(\)**: 放到运行结束的地方，以结束本次运行的日志记录。

```python
import wandb
for x in range(10):
    run = wandb.init(project="runs-from-for-loop", reinit=True)
    for y in range (100):
        wandb.log({"metric": x+y})
    run.finish()
```

还有个方法就是，使用Python上下文管理器，将自动结束记录日志：

```python
import wandb
for x in range(10):
    run = wandb.init(reinit=True)
    with run:
        for y in range(100):
            run.log({"metric": x+y})
```

## LaunchError: Permission denied **拒绝访问）**

当你正要把运行项发送到某个项目，如果你遇到这样的错误**LaunchError: Launch exception: Permission denied**，说明你没有权限记录到该项目。应该有多方面的原因。

1. 你没有登录到该机器。在命令行运行`wandb login`
2. 你设置的归属单位（entity）不存在。“entity”应当为你的用户名或已存在的团队的名称。如果你要创建一个团队，请打开我们的[订购页](https://wandb.ai/billing)。
3. 你没有项目权限。请找项目创建者把隐私权限设置为**Open**（开放），这样你就能把运行项记录到该项目

**给运行项取一个易辨别的名称**

给运行项取一个好看、易辨别的名称。

```python
import wandb

wandb.init()
run_name = wandb.run.name
```

**把运行项名称设置为生成的运行项id**

如果你想把运行项名称（如snowy-owl-10）重写为运行项id（如qvlp96vk），则用下面这段代码：

```python
import wandb
wandb.init()
wandb.run.name = wandb.run.id
wandb.run.save()
```

**保存git提交**

当在脚本中调用`wandb.init()`，我们自动查找git信息，以保存一个指向repo的链接，即最近提交的SHA值。git信息应该在你的[运行页](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MKaPhwzNIegNuInaekR/app/pages/run-page#overview-tab)，如果没有的话，请确保你调用wandb.init\(\)的脚本所在的文件夹有git信息。

git提交以及运行实验的命令对你可见、对别的用户隐藏，所以，你的项目是公开的，其详细信息仍然是保密的。

**离线保存日志**

默认情况下，wandb.init\(\)会启动一个进程，把指标实时同步到我们的云托管应用程序。如果你的机器离线，或者你不能上网，下面讲述如何用离线模式运行wandb并在以后同步。

设置两个环境变量：

1. **WANDB\_API\_KEY**: 设置为账号的API密钥，该密钥在你的[设置页](https://wandb.ai/login)；
2. **WANDB\_MODE**: dryrun

下面举例说明这段脚本：

```python
import wandb
import os

os.environ["WANDB_API_KEY"] = YOUR_KEY_HERE
os.environ["WANDB_MODE"] = "dryrun"

config = {
  "dataset": "CIFAR10",
  "machine": "offline cluster",
  "model": "CNN",
  "learning_rate": 0.01,
  "batch_size": 128,
}

wandb.init(project="offline-demo")

for i in range(100):
  wandb.log({"accuracy": i})
```

下面是终端输出的示例：

![](../../.gitbook/assets/image%20%2881%29.png)

连上网以后，运行一条同步命令即可把该文件夹发送到云端。

`wandb sync wandb/dryrun-folder-name`

![](../../.gitbook/assets/image%20%2836%29.png)

