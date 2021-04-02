# TensorBoard

W&B支持给TensorBoard打补丁，以便从你的脚本中将全部指标自动记录到我们的本地图表。

```python
import wandb
wandb.init(sync_tensorboard=True)
```

我们支持通过所有版本的TensorFlow使用TensorBoard。如果你正通过其他框架使用TensorBoard， W&B支持通过PyTorch以及TensorBoardX使用TensorBoard 1.14以上版本。

### **自定义指标（Metric）**

如果你需要记录没有被记录到TensorBoard的额外自定义指标，你可以在你的代码中调用`wandb.log`，并使用与TensorBoard相同的step参数： 即`wandb.log({"custom": 0.8}, step=global_step)`

###  **高级配置**

如果你想更多地控制给TensorBoard打补丁的方式，你可以调用`wandb.tensorboard.patch`，而不是将`sync_tensorboard=True`传给init。 你可以向该方法传入tensorboardX=False ,以确保vanilla TensorBoard被打上补丁，如果在通过PyTorch使用TensorBoard 1.14以上版本，你可以传入`pytorch=True`，以确保它被打上补丁。这两个选项都有智能的默认值，默认值取决于这些库的导入版本。

默认情况下，我们还会同步tfevents文件和全部\*.pbtxt文件。这样我们就能替你启动一个TensorBoard实例。在运行页面你会看到一个[TensorBoard选项卡](https://www.wandb.com/articles/hosted-tensorboard)。可以通过将`save=False`传入`wandb.tensorboard.patch`来禁用该行为。

```python
import wandb
wandb.init()
wandb.tensorboard.patch(save=False, tensorboardX=True)
```

默认情况下，我们还会同步tfevents文件和全部\*.pbtxt文件。这样我们就能替你启动一个TensorBoard实例。在运行页面你会看到一个[TensorBoard选项卡](https://www.wandb.com/articles/hosted-tensorboard)。可以通过将`save=False`传入`wandb.tensorboard.patch`来禁用该行为。

###  **同步先前的TensorBoard运行**

如果你在本地存储有使用wandb库集成生成的tfevents文件，并且你想要将其导入wandb,你可以通过运行`wandb sync log_dir`，其中`log_dir`是包含tfevents文件的本地路径。 

你也可以运行`wandb sync directory_with_tf_event_file`。

```bash
"""This script will import a directory of tfevents files into a single W&B run.
You must install wandb from a special branch until this feature is merged
into the mainline:""" 
pip install --upgrade git+git://github.com/wandb/client.git@feature/import#egg=wandb
```

你可以用`python no_image_import.py dir_with_tf_event_file`.调用这个脚本。这将在wandb中创建一个运行，并从该目录中的even\_file文件中提取指标。如果你想要在许多目录上运行该脚本，你应该在每个运行上只执行该脚本一次，所以一个加载器可能如下所示：

```python
import glob
for run_dir in glob.glob("logdir-*"):
  subprocess.Popen(["python", "no_image_import.py", run_dir],
                   stderr=subprocess.PIPE, stdout=subprocess.PIPE)
```

```python
import glob
import os
import wandb
import sys
import time
import tensorflow as tf
from wandb.tensorboard.watcher import Consumer, Event
from six.moves import queue

if len(sys.argv) == 1:
    raise ValueError("Must pass a directory as the first argument")

paths = glob.glob(sys.argv[1]+"/*/.tfevents.*", recursive=True)
root = os.path.dirname(os.path.commonprefix(paths)).strip("/")
namespaces = {path: path.replace(root, "")\
              .replace(path.split("/")[-1], "").strip("/")
              for path in paths}
finished = {namespace: False for path, namespace in namespaces.items()}
readers = [(namespaces[path], tf.train.summary_iterator(path)) for path in paths] 
if len(readers) == 0: 
    raise ValueError("Couldn't find any event files in this directory")

directory = os.path.abspath(sys.argv[1])
print("Loading directory %s" % directory)
wandb.init(project="test-detection")

Q = queue.PriorityQueue()
print("Parsing %i event files" % len(readers))
con = Consumer(Q, delay=5)
con.start()
total_events = 0

while True:

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

print("Persisting %i events..." % total_events)
con.shutdown(); print("Import complete")
```

### **Google Colab和TensorBoard**

为了在Colab的命令行运行命令，必须运行`!wandb sync directoryname`。目前TensorBoard同步功能无法在Tensorflow 2.1+的笔记本环境使用。你可以在Colab使用老版本的TensorBoard，或者通过`!python your_script.py`从命令行运行一个脚本。

##  **W&B和TensorBoard有什么不同？**

我们受到启发要为大家改进实验跟踪工具。当合伙人开始构建W&B的工作时，他们受到启发要为OpenAI中受挫的TensorBoard 用户打造一个工具。以下是我们重点改进的几项内容：

1.   **重现模型**: Weights & Biases有利于实验、探索和以后重现模型。我们不仅可以捕获指标（Metric）,还可以捕获超参数和代码版本，我们还可以为你保存你的模型检查点，这样你的项目可以被重现。
2.   **自动组织**: 如果你将一个项目交给一个同事或者你要去休假。W&B使得很容易查看所有你试过的实验，这样就不用浪费时间重新运行旧实验。
3. **快速、灵活的集成**: 5分钟之内就可以将W&B添加到你的项目中。安装我们的免费开源Python包，并在你的代码中添加几行代码。然后每次你运行你的模型时，你的指标和记录都会被很好记录。 
4. **持久的集中式仪表盘**: 无论你在哪里训练模型，无论是在你的本地机器、你的实验室集群或云端的竞价实例（Spot Instance）中，我们都会为你提供相同的集中式仪表盘。你不需要花费时间从不同的机器上复制和组织TensorBoard文件。
5.  **强大的表格**: 搜索、过滤、排序和分组不同模型的结果。很容易查看成千上万的模型版本，并为不同的任务找到性能最好的模型。TensorBoard无法在大型项目上很好工作。
6. **协作的工具**: 使用W&B来组织复杂的机器学习项目。分享W&B链接非常容易，你可以使用私有（private）团队,让每个人将结果发送到共享项目。我们还支持通过报告进行协作——添加交互式可视化，并用markdown描述你的工作。这是保留工作日志，与你的上级分享发现，或向你的实验室展示发现的好方法。

开设一个[个人免费账号→](http://app.wandb.ai/)​​

