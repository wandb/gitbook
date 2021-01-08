---
description: 在你的脚本顶部调用 wandb.init() 以启动一个新运行
---

# wandb.init\(\)

在你的脚本开始调用一次wandb.init\(\) 以初始化一个新作业，这将在W&B 中创建一个新运行并启动一个后台进程来同步数据。

* **On Prem:** 如果你需要W&B 的私有云或本地实例，请参见我们的[自托管](https://docs.wandb.com/self-hosted)产品。
*  **自动化环境**: 大多数这些设置也可以通过[环境变量](https://docs.wandb.com/library/environment-variables)进行控制。当你在集群上运行作业时，这非常有用。

### 参考文档 <a id="reference-docs"></a>

从参考文档中查看参数.[Init/ref/init](https://docs.wandb.ai/ref/init)

##  常见问题 <a id="common-questions"></a>

### **如何从一个脚本中启动多个运行？** <a id="how-do-i-launch-multiple-runs-from-one-script"></a>

如果你想从一个脚本启动多个运行，需要在你的代码中添加如下两步:

1. run = wandb.init\(**reinit=True**\): 使用这个设置以允许重新初始化运行。
2. **run.finish\(\)**: 在你的运行结束时使用此功能来完成该运行的日志记录。

```text
import wandbfor x in range(10):    run = wandb.init(project="runs-from-for-loop", reinit=True)    for y in range (100):        wandb.log({"metric": x+y})    run.finish()
```

或者你也可以使用Python上下文管理器，它将自动完成记录日志：

```text
import wandbfor x in range(10):    run = wandb.init(reinit=True)    with run:        for y in range(100):            run.log({"metric": x+y})
```

### LaunchError: Permission denied <a id="launcherror-permission-denied"></a>

如果遇到**错误** **LaunchError: Launch exception: Permission denied ，说明你没有权限登录到你正发送运行的目标项目。这可能有如下原因。**

1. 你没有登录到该机器。在命令行运行`wandb login`
2. 你设置的实体（Entity）不存在。“实体（Entity）”应当为你的用户名或已存在的团队的名称。如果你要创建一个团队，请打开我们的[订阅页](https://wandb.ai/billing)面。
3. 你没有项目权限。请找项目创建者把隐私权限设置为开放（**Open**），这样你就能把运行记录到该项目。

### **给运行取一个易读的名称** <a id="get-the-readable-run-name"></a>

给运行取一个易读的名称。

```text
import wandb​wandb.init()run_name = wandb.run.name
```

### **把运行项名称设置为生成的运行id** <a id="set-the-run-name-to-the-generated-run-id"></a>

如果你想把运行名称（如snowy-owl-10）重写为运行id（如qvlp96vk），可以用这个代码片段：

```text
import wandbwandb.init()wandb.run.name = wandb.run.idwandb.run.save()
```

###  **保存git commit** <a id="save-the-git-commit"></a>

当`wandb.init()` 在你的脚本中被调用时，我们会自动查找git信息，以保存一个指向你仓库的链接，即最新提交的SHA值。git信息应该显示在你的[运行页](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MKaPhwzNIegNuInaekR/app/pages/run-page#overview-tab)面，如果没有的话，请确保你调用wandb.init\(\)的脚本位于一个有git信息的文件夹中。

 git commit以及运行实验的命令对你可见但对外部用户隐藏，所以，即使你的项目是公开的，这些详细信息将仍然保持私有。

### **离线保存日志** <a id="save-logs-offline"></a>

 默认情况下，wandb.init\(\)会启动一个进程，把指标（Metric）实时同步到我们的云托管应用程序。如果你的机器离线，或者你无法访问互联网，下面讲述如何以离线模式运行wandb并在稍后同步。

 设置两个环境变量：

1. **WANDB\_API\_KEY**: Set this to your account's API key, on your [settings page](https://app.wandb.ai/settings)​
2. **WANDB\_MODE**: dryrun

 下面是一个脚本示例：

```text
import wandbimport os​os.environ["WANDB_API_KEY"] = YOUR_KEY_HEREos.environ["WANDB_MODE"] = "dryrun"​config = {  "dataset": "CIFAR10",  "machine": "offline cluster",  "model": "CNN",  "learning_rate": 0.01,  "batch_size": 128,}​wandb.init(project="offline-demo")​for i in range(100):  wandb.log({"accuracy": i})
```

下面是一个终端输出示例:![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-M4ZqIaDYRFSEiZrYTaI%2F-M4Zx9NGlicWWRF-Zcgh%2Fimage.png?alt=media&token=6f32064c-d58e-412e-8344-ed43baee721e)

一旦可以访问互联网，运行一条同步命令即可把该文件夹发送到云端。

`wandb sync wandb/dryrun-folder-name`![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-M4ZqIaDYRFSEiZrYTaI%2F-M4ZxQU2WrG9S0MZzqDI%2Fimage.png?alt=media&token=0295541a-90bf-464f-8899-2f9a53c45e1c)[PreviousLibrary](https://docs.wandb.ai/library)[Nextwandb.config](https://docs.wandb.ai/library/config)4![](https://lh5.googleusercontent.com/-ohnhj3YAM9Y/AAAAAAAAAAI/AAAAAAAAAAA/ACHi3rdb3Cq-FZ97LHoDhbnKVi0teESbRg/photo.jpg)![](https://avatars2.githubusercontent.com/u/29?v=4)![](https://gblobscdn.gitbook.com/users%2FtXkcUGlNaYSjmMWkzQQLlJUqbmB2%2Favatar.png?alt=media)CONTENTS[Reference Docs](https://docs.wandb.ai/library/init#reference-docs)[Common Questions](https://docs.wandb.ai/library/init#common-questions)[How do I launch multiple runs from one script?](https://docs.wandb.ai/library/init#how-do-i-launch-multiple-runs-from-one-script)[LaunchError: Permission denied](https://docs.wandb.ai/library/init#launcherror-permission-denied)[Get the readable run name](https://docs.wandb.ai/library/init#get-the-readable-run-name)[Set the run name to the generated run ID](https://docs.wandb.ai/library/init#set-the-run-name-to-the-generated-run-id)[Save the git commit](https://docs.wandb.ai/library/init#save-the-git-commit)[Save logs offline](https://docs.wandb.ai/library/init#save-logs-offline)

