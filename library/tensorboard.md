# TensorBoard

权阈能够修补TensorBoard，从脚本把全部指标自动记录到本地图表。

```python
import wandb
wandb.init(sync_tensorboard=True)
```

我们支持各版本TensorFlow的TensorBoard。如果你将TensorBoard搭配其它框架使用，权阈支持TensorBoard &gt; 1.14搭配PyTorch以及TensorBoardX。.

### Custom Metrics

TensorBoard并不记录全部指标，如果你需要额外记录自定义指标，可以在代码中调用`wandb.log`，并且与TensorBoard用同样的时间步参数：即`wandb.log({"custom": 0.8}, step=global_step)`

### Advanced Configuration

 如果你想更多地控制如何修补TensorBoard，那么，就在初始化时调用`wandb.tensorboard.patch`，而不要传入`sync_tensorboard=True`。为保证vanilla TensorBoard被修补上，你可以向该方法传入tensorboardX=False，如果你用的是TensorBoard &gt; 1.14搭配PyTorch，就可以传入`pytorch=True`，即可保证被修补上。这两个选项都有智能的默认值，默认值要看导入的是哪个版本的库。

 默认情况下，我们还会同步tfevents文件和全部\*.pbtxt文件。这样我们就能替你启用一个[TensorBoard实例](https://www.wandb.com/articles/hosted-tensorboard)。在运行项页面你会看到一个TensorBoard选项卡。可以禁用该行为，方法就是向`wandb.tensorboard.patch`传入`save=False`。

```python
import wandb
wandb.init()
wandb.tensorboard.patch(save=False, tensorboardX=True)
```

###  **同步先前的TensorBoard运行项**

如果你想把以前的实验导入权阈，可以运行`wandb sync log_dir`，其中`log_dir`是tfevents文件所在的本地路径。还可以运行`wandb sync directory_with_tf_event_file`。

You can also run `wandb sync directory_with_tf_event_file`

```python
"""This script will import a directory of tfevents files into a single W&B run.
You must install wandb from a special branch until this feature is merged into the mainline: 
```bash
pip install --upgrade git+git://github.com/wandb/client.git@feature/import#egg=wandb
```



```python
import glob
for run_dir in glob.glob("logdir-*"):
  subprocess.Popen(["python", "no_image_import.py", run_dir], stderr=subprocess.PIPE, stdout=subprocess.PIPE)
```



```text
# Consume 500 events at a time from all readers and push them to the queue
for namespace, reader in readers:
    if not finished[namespace]:
        for i in range(500):
            try:
                event = next(reader)
                kind = event.value.WhichOneof("value")
                if kind != "image":
                    Q.put(Event(event, namespace=namespace))
                    total_events += 1
            except StopIteration:
                finished[namespace] = True
                print("Finished parsing %s event file" % namespace)
                break
if all(finished.values()):
    break
```

print\("Persisting %i events..." % total\_events\) con.shutdown\(\) print\("Import complete"\)

\`\`\`

### Google Colab and TensorBoard

 为了在Colab的命令行运行命令，必须运行`!wandb sync directoryname`。目前TensorBoard同步功能无法在Tensorflow 2.1+的笔记本环境使用。你可以在Colab使用老版本的TensorBoard，或者在命令行运行脚本时加上`!python your_script.py`。

## **权阈和TensorBoard有什么不同？**

我们决心要为每个人改进实验跟踪工具。当联合创始人们开始创立权阈，他们就很想在OpenAI为垂头丧气的TensorBoard用户开发一个工具。我们重点改进以下几方面：

1. **重新制作模型**：权阈有利于实验、探索、以后重新制作模型。我们不仅捕捉指标，还捕捉超参数和代码版本，并且，我们会保存你的模型检查点，因而你的项目是可再生的。
2.  **自动组织**：如果你把项目交给同事或者要去度假，权阈可以让你便捷地查看你制作的所有模型，你就不必花费大量时间来重新运行旧实验。
3.  **快速、灵活的集成**：只需5分钟即可把权阈加到自己的项目。下载我们免费的开源Python包，然后在代码中插入几行，以后你每次运行模型都会得到记录完备的指标和记录。
4.  **持续、集中式指示板**：不管你在哪里训练模型，不管是在本地机器、实验室集群还是在云端竞价实例，我们为您提供同样的集中式指示板。你不必花时间从别的机器上复制、组织TensorBoard文件。
5.  **强大的表格**：对不同模型的结果进行搜索、筛选、分类和分组。可以轻而易举地察看成千上万个模型版本，并找到不同任务的最佳模型。TensorBoard本身就不适合大型项目。
6. **协作工具**：用权阈组织复杂的机器学习项目。发送权阈的链接非常简便，你可以使用非公开团队，让每个人把结果发送至共享项目。我们还支持通过报告协作——加入交互式的可视化，在富文本编辑器描述自己的工作成果。这非常便于保存工作日志、与上司分享成果以及向自己的实验室展示成果。

新手就先注册一[个免费个人账号→](https://wandb.ai/)

