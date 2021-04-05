# Technical FAQ

### **这对我的的训练进程有什么影响？**

 当`wandb.init()` 从你的训练脚本中被调用时，一个API调用将在我们的服务器上创建一个运行（run）对象。一个新的进程被启动，以流式传输和收集指标（metric）,从而使所有线程和逻辑不在你的主进程中。你的脚本正常运行并写入到本地文件，而新启动的独立进程会将他们它们和系统指标一起流式传输到我们的服务器上。你可以通过从你的训练目录中运行`wandb off`，或者将**WANDB\_MODE** 环境变量设置为"dryrun"来关闭流式传输。

#### **如果wandb崩溃, 它是否有可能使我的训练运行崩溃？**

绝不干扰你的训练运行，对我们而言非常重要。我们在一个独立进程中运行wandb以确保如果wandb由于某些原因崩溃，你的训练将可以继续运行。如果网络中断，wandb将继续尝试向wandb.com发送数据。

### **wandb会拖慢我的训练吗？**

如果你正常使用wandb,它对你的训练性能的影响应该可以忽略不计。正常使用wandb意味着每秒记录不到一次，每一步（step）记录的数据少于几兆。Wandb 运行在一个独立的进程中，方法调用不会阻塞，所有如果网络出现短暂的故障或者磁盘出现间歇性读写问题，应该不会影响你的性能。可以快速记录大量数据，如果你这样做可能会造成磁盘I/O问题。如果有任何问题，请不要犹豫联系我们。

### **我可以离线运行wandb吗？**

 如果你是在一个离线机器上进行训练，并想在训练结束后将训练结果上传到我们的服务器，我们有一个功能可以满足你的需求！

1. 设置环境变量`WANDB_MODE=dryrun` 以将指标（metric）保存在本地，不需要网络。
2. 当你准备就绪，在你的目录中运行`wandb init` 来设置项目名称。
3. 运行`wandb sync YOUR_RUN_DIRECTORY` 将指标（metric）推送到我们的云服务，并在我们托管的web应用程序中查看结果。

### **你的工具是否跟踪和存储训练数据？**

你可以向`wandb.config.update(...)` 传递一个SHA或其他唯一标识符，以将数据集与训练运行相关联。除非用本地文件名调用 `wandb.save` ，否则W&B不会存储任何数据。 

### **多久收集一次系统指标（Metric）？**

默认，每2秒收集一次指标,并取30秒内平均值。如果你需要更高分辨率的指标，请发邮件给我们 [contact@wandb.com](mailto:contact@wandb.com).

###  **这是否仅适用于Python？**

目前该库只适用于Python 2.7+ & 3.6+ 项目。上面提到的架构应该可以让我们很容易与其他语言集成。如果你有监控其他语言的需求，请发邮件给我们 [contact@wandb.com](mailto:contact@wandb.com).

###  **我可以只记录指标，而不记录代码或数据集实例吗？**

**数据集实例**

 默认情况下，我们不会记录你的任何数据集实例。你可以显式打开该功能，以便在我们的Web界面上查看实例预测。

 **代码记录**

 有两种方法可以关闭代码记录：

1. 将**WANDB\_DISABLE\_CODE** 设置为**true** ，以关闭所有代码跟踪。我们将不提取git SHA或diff补丁。
2.  将**WANDB\_IGNORE\_GLOBS** 设置为 **\*.patch** ，以关闭将diff补丁同步到我们的服务器。你将仍然在本地拥有它，并且能够用命令[wandb restore](https://docs.wandb.ai/library/cli#restore-the-state-of-your-code)来应用它。

### **记录会阻塞我的训练吗？**

"记录功能是不是很懒？我不想依懒网络把结果发到你的服务器，然后继续我的本地操作。"调用**wandb.log** 向本地文件写入一行; 它不会阻塞任何网络调用。

当你调用wandb.init时，我们会在同一台机器上启动一个新进程，监听文件系统的变化，并从你的训练进程异步地与我们的网络服务对话。 

### **你的平滑算法用的是什么公式?**

我们使用与TensorBoard相同的指数移动平均公式。你可以在这里找到解释：[https://stackoverflow.com/questions/42281844/what-is-the-mathematics-behind-the-smoothing-parameter-in-tensorboards-scalar](https://stackoverflow.com/questions/42281844/what-is-the-mathematics-behind-the-smoothing-parameter-in-tensorboards-scalar).

###  **W&B与TensorBoard有何不同?**

我们热爱TensorBoard的人们, 而且我们有一个[TensorBoard集](https://docs.wandb.ai/integrations/tensorboard)成! 我们受到启发要为大家改进实验跟踪工具。当合伙人开始构建W&B的工作时，他们受到启发要为OpenAI中受挫的TensorBoard 用户打造一个工具。以下是我们重点改进的几项内容：

1. **重现模型**: Weights & Biases有利于实验、探索和以后重现模型。我们不仅可以捕获指标（Metric）,还可以捕获超参数和代码版本，我们还可以为你保存你的模型检查点，这样你的项目可以被重现。
2. **自动组织**: 如果你将一个项目交给一个同事或者你要去休假。W&B使得很容易查看所有你试过的实验，这样就不用浪费时间重新运行旧实验。
3.  **快速、灵活的集成**: 5分钟之内就可以将W&B添加到你的项目中。安装我们的免费开源Python包，并在你的代码中添加几行代码。然后每次你运行你的模型时，你的指标和记录都会被很好记录。 
4. **持久的集中式仪表盘**: 无论你在哪里训练模型，无论是在你的本地机器、你的实验室集群或云端的竞价实例（Spot Instance）中，我们都会为你提供相同的集中式仪表盘。你不需要花费时间从不同的机器上复制和组织TensorBoard文件。
5.  **强大的表格**: 搜索、过滤、排序和分组不同模型的结果。很容易查看成千上万的模型版本，并为不同的任务找到性能最好的模型。TensorBoard无法在大型项目上很好工作。
6.  **协作的工具**: 使用W&B来组织复杂的机器学习项目。分享W&B链接非常容易，你可以使用私有（private）团队,让每个人将结果发送到共享项目。我们还支持通过报告进行协作——添加交互式可视化，并用markdown描述你的工作。这是保留工作日志，与你的上级分享发现，或向你的实验室展示发现的好方法。

开设一个[个人免费账号→](http://app.wandb.ai/)​

###  **如何在我的训练代码中配置运行名称？**

在你的训练脚本顶部，当你调用wandb.init时，传入一个实验名称，像这样: `wandb.init(name="my awesome run")`

### **如何在我的脚本中获取随机运行名称？**

 调用`wandb.run.save()` ，然后用`wandb.run.name` 获取名称。

### **有没有anaconda包?**

我们没有anaconda包，但你应该可以安装wandb，使用:

```text
conda activate myenv
pip install wandb
```

如果你在安装过程中遇到问题，请告诉我们。这个Anaconda [关于管理软件包的文档](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-pkgs.html) 有一些有用指导。

###  **如何阻止wandb写到我的终端或jupyter笔记本输出?**

设置环境变量 [WANDB\_SILENT](https://docs.wandb.ai/library/environment-variables)。

在一个笔记本中:

```text
%env WANDB_SILENT true
```

在一个python脚本中:

```text
os.environ["WANDB_SILENT"] = "true"
```

###  **如何终止带有wandb的作业？**

按键盘上的ctrl+D 停止使用wandb监控的脚本。

### How do I deal with network issues? **如何解决网络问题？**

如果你看到SSL或网络错误:`wandb: Network error (ConnectionError), entering retry loop.` 你可以尝试如下几种不同方式来解决该问题:

1. 升级你的SSL证书。如果你在Ubuntu服务器上运行脚本，请运行`update-ca-certificates` ，如果没有有效的SSL证书，我们无法同步训练记录，因为存在安全隐患。
2. 如果你的网络存在故障，请以[离线模式](https://docs.wandb.com/resources/technical-faq#can-i-run-wandb-offline)运行训练，然后从一个可以访问互联网的机器将文件同步给我们。
3. 尝试运行[W&B本](https://docs.wandb.ai/self-hosted/local)地, 它将在你的机器上操作，而不会将文件同步到我们的云服务器。

**SSL CERTIFICATE\_VERIFY\_FAILED:** 这个错误可能是由于你公司的防火墙引起的。你可以设置本地CAs然后使用：

`export REQUESTS_CA_BUNDLE=/etc/ssl/certs/ca-certificates.crt`

### **如果我在训练模型时网络连接中断了会发生什么?**

如果我们的程序库无法连接到互联网，它将进入一个重试循环，并不断尝试流式传输指标（Metric）直到网络恢复。在此期间，你的程序可以继续运行。

如果需要在没有互联网的机器上运行，你可以设置WANDB\_MODE=dryrun ，将指标（Metric）只存储在本地硬盘。之后你可以调用 `wandb sync DIRECTORY` 来将数据流式传输到我们的服务器。

### **我可以记录两个不同时间尺度上的指标（Metric）吗? \(例如，我想记录每个批次的训练准确率和每个周期的验证准确率\)**

是的，你可以通过记录多个指标，然后给他们设置一个x轴值来实现。所你可以在一个步（step）中调用 `wandb.log({'train_accuracy': 0.9, 'batch': 200})` ，在另一个步（step）中调用 `wandb.log({'val_acuracy': 0.8, 'epoch': 4})`

### **如何记录一个不随时间变化的指标，比如最终评估准确率？**

使用 wandb.log\({'final\_accuracy': 0.9} 可以很好解决这个问题。默认 wandb.log\({'final\_accuracy'}\)会更新wandb.settings\['final\_accuracy'\]，也就是运行表中显示的值。

### **如何在运行完成后记录其他指标？**

有几种方式可以做到这一点。

对于复杂的工作流程，我们建议使用多个运行，并在组成单个实验的所有进程中为[wandb.init](https://docs.wandb.ai/library/init)将组参数设置为一个唯一的值。[运行表](https://docs.wandb.ai/app/pages/run-page)将根据组ID自动分组并按照预期可视化。这将允许你运行多个实验并以独立进程记录所有结果到一个地方。

对于比较简单的工作流程，你可以使用参数resume=True 和id=UNIQUE\_ID调用wandb.init，然后以同样的参数id=UNIQUE\_ID调用wandb.init。然后你就可以使用[wandb.log](https://docs.wandb.ai/library/log) 或wandb.summary正常记录，运行值会更新。

在任何时候，你都可以使用[API](https://docs.wandb.ai/ref/export-api) 来添加额外的评估指标（Metric）。

###   **.log\(\) 和.summary的区别是什么？** 

summary 是显示在表格中的值，而log将保存所有的值以便稍后绘制。

 例如，你可能想在每次准确率变化时调用`wandb.log`，通常你只需要使用.log。 `wandb.log()` 默认也会更新总结值（Summary）,除非你为该指标手动设置了总结（Summary）。

 散点图和平行坐标图也会使用总结值（Summary）,而线图则会绘制所有由.log设置的值。

我们之所以提供了这样两个方法，是因为有些人喜欢手动设置总结（Summary）,因为他们希望总结（Summary）能够反应出例如最佳准确率，而不是最后记录的准确率。

### **如何在没有gcc的环境中安装wandb Python 库？**

 如果你尝试安装`wandb` 并看到如下错误:

```text
unable to execute 'gcc': No such file or directory
error: command 'gcc' failed with exit status 1
```

你可以直接从一个已做好的轮子安装psutil。在这里找到你的Python版本和OS: [https://pywharf.github.io/pywharf-pkg-repo/psutil](https://pywharf.github.io/pywharf-pkg-repo/psutil)

例如，在Linux中的python 3.8 上安装psutil:

```text
pip install https://github.com/pywharf/pywharf-pkg-repo/releases/download/psutil-5.7.0-cp38-cp38-manylinux2010_x86_64.whl/psutil-5.7.0-cp38-cp38-manylinux2010_x86_64.whl#sha256=adc36dabdff0b9a4c84821ef5ce45848f30b8a01a1d5806316e068b5fd669c6d
```

安装完psutil后，可以用 `pip install wandb`来安装wandb

### **wandb流如何记录和写入磁盘？**

 W&B在内存中排队，但也会将[事件异步写入磁盘](https://github.com/wandb/client/blob/7cc4dd311f3cdba8a740be0dc8903075250a914e/wandb/sdk/internal/datastore.py)以处理故障，对于`WANDB_MODE=offline` 的情况，在记录结束后，你可以同步数据。

在终端中，你可以看到本地运行目录的路径。该目录包含一个“.wandb”文件，就是上边的数据存储位置。如果你也记录图片，在将它们上传到云存储之前，我们会将其写到该目录中的“media/images”下。

## **如何通过选择不同运行获得多个图表？**

使用wandb报告的过程如下：

* 有多个面板网格
* 添加过滤器以过滤每个面板网格的运行集。这将有助于选择你想在各个面板上绘制的运行。
* 在面板网格中创建你想要的图表。

