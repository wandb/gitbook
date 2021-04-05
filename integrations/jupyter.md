# Jupyter

 在你的Jupyter笔记本中使用Weights & Biases来获得交互式可视化，并对训练运行进行自定义分析。

## **Jupyter笔记本的W&B使用案例**

1. **迭代实验**: 运行和重新运行实验，调整参数，并将所有运行结果保存到W&B，而无需手动记录。
2. **代码保存**: 当重现一个模型时，很难知道笔记本上的哪些单元格在运行，以及运行的顺序。在设置页面开启代码保存，可以保存每个实验的单元格执行记录。
3.  **自定义分析**: 一旦运行被记录到W&B, 就可以很容易地从API中获取数据框（dataframe）,并进行自定义分析，然后将这些结果记录到W&B,以便在报告中保存和分享。

## **配置笔记本**

用以下代码启动你的笔记本，安装W&B并链接你的账户：

```python
!pip install wandb -qqq
import wandb
wandb.login()
```

接下来，设置你的实验并保存超参数：

```python
wandb.init(project="jupyter-projo",
           config={
               "batch_size": 128,
               "learning_rate": 0.01,
               "dataset": "CIFAR-100",
           })
```

运行`wandb.init()` 后, 通过 `%%wandb` 启动一个新单元格以便在笔记本中查看实时图形。如果你多次运行该单元格，数据将被附加到运行中。

```python
%%wandb

# Your training loop here
```

你可以自己尝试，在这个[快速示例脚本中→](https://bit.ly/wandb-jupyter-widgets-colab)​

![](../.gitbook/assets/jupyter-widget.png)

 作为 `%%wandb` 装饰器的替代方法，在运行`wandb.init()` 后，你可以使用`wandb.run`结束任何单元格以显示内嵌图形。

```python
# Initialize wandb.run first
wandb.init()

# If cell outputs wandb.run, you'll see live graphs
wandb.run
```

## **W&B中的其他Jupyter功能**

1. **Colab**: 当你在Colab中第一次调用`wandb.init()` 时，如果你当前在浏览器中登录了W&B,我们会自动认证你的运行时。在你运行页面的概览选项卡上，你会看到一个指向Colab的链接。如果你在[设置](https://app.wandb.ai/settings) 中开启了代码保存，你还可以看到运行实验时执行的单元格，从而可以更好地重现。
2.   **启动Docker Jupyter**: 调用`wandb docker --jupyter` 来启动一个docker容器，在其中挂载你的代码，确保Jupyter已经安装,并在端口8888启动。
3. **run.finish\(\)**: 默认情况下，我们要等待下一次调用wandb.init\(\)时才会将运行标记为完成。这允许你运行单个单元格，并将它们都记录到同一运行中。要在Jupyter笔记本中手动标记一个运行为完成,请使用**run.finish\(\)** 功能。

```python
import wandb
run = wandb.init()
# Training script and logging goes here
run.finish()
```

### **静音W&B消息**

要禁用消息，请在笔记本中运行以下内容：

```python
import logging
logger = logging.getLogger("wandb")
logger.setLevel(logging.ERROR)
```

## **常见问题**

### **笔记本名称**

如果你看到错误信息"Failed to query for notebook name, you can set it manually with the WANDB\_NOTEBOOK\_NAME environment variable（查询笔记本名称失败，你可以用WANDB\_NOTEBOOK\_NAME环境变量来手动设置它）," 你可以通过从你的脚本中设置该环境变量来解决，像这样：`os.environ['WANDB_NOTEBOOK_NAME'] = 'some text here'`

