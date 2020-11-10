# Sweeps Quickstart

从任何机器学习模型开始，在几分钟内运行超参数扫描。想看一个例子吗？这是[示例代码](https://github.com/wandb/examples/tree/master/examples/pytorch/pytorch-cnn-fashion)和示例[仪表板。](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MKaPhwzNIegNuInaekR/sweeps-1/sweeps-quickstart)

![](../../.gitbook/assets/image%20%2850%29.png)

{% hint style="info" %}
已经有一个权阈项目？[跳到我们的下一个sweep教程→](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MKaPhwzNIegNuInaekR/sweeps/add-to-existing)
{% endhint %}

### **1.添加wandb**

设置您的帐户

1. 从W＆B帐户开始。[现在创建一个→](https://wandb.ai)
2. 转到终端中的项目文件夹并安装我们的库：`pip install wandb`
3. 在您的项目文件夹中，登录到W＆B：`wandb login`

**设置您的Python训练脚本**

1. 导入我们的库`wandb`
2. 确保您的超参数可以通过sweep正确设置。在脚本顶部的字典中定义它们，并将它们传递到wandb.init中。
3. 记录指标以在实时仪表板中查看它们

```python
import wandb

# Set up your default hyperparameters before wandb.init
# so they get properly set in the sweep
hyperparameter_defaults = dict(
    dropout = 0.5,
    channels_one = 16,
    channels_two = 32,
    batch_size = 100,
    learning_rate = 0.001,
    epochs = 2,
    )

# Pass your defaults to wandb.init
wandb.init(config=hyperparameter_defaults)
config = wandb.config

# Your model here ...

# Log metrics inside your training loop
metrics = {'accuracy': accuracy, 'loss': loss}
wandb.log(metrics)
```

[See a full code example →](https://github.com/wandb/examples/tree/master/examples/pytorch/pytorch-cnn-fashion)

## 2. **.扫描配置**

设置一个YAML文件以指定您的训练脚本，参数范围，搜索策略和停止条件。 W＆B会将这些参数及其值作为命令行参数传递给您的训练脚本，我们将使用在[步骤1](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MKaPhwzNIegNuInaekR/sweeps/quickstart#set-up-your-python-training-script)中设置的config对象自动解析它们。

Here are some config resources:

1. [ 示例YAML](https://github.com/wandb/examples/blob/master/examples/pytorch/pytorch-cnn-fashion/sweep-grid-hyperband.yaml)：执行扫描的脚本和YAML文件的代码示例
2. [置](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MKaPhwzNIegNuInaekR/sweeps/configuration)：设置扫描配置的完整规格
3. [Jupyter Notebook](../../sweeps/python-api.md): 使用Python字典而不是YAML文件设置扫描配置
4. [从UI生成配置](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MKaPhwzNIegNuInaekR/sweeps/add-to-existing)：接受现有的W＆B项目并生成配置文件
5. [输入先前的运行](https://docs.wandb.com/sweeps/add-to-existing#seed-a-new-sweep-with-existing-runs)：进行先前的运行并将其添加到新扫描中

这是一个名为**sweep.yam**l的扫描配置YAML文件示例：

```text
program: train.py
method: bayes
metric:
  name: val_loss
  goal: minimize
parameters:
  learning_rate:
    min: 0.001
    max: 0.1
  optimizer:
    values: ["adam", "sgd"]
```

{% hint style="warning" %}
如果您指定要优化的指标，请确保您正在记录它。在此示例中，我的配置文件中有**val\_loss**，因此我必须在脚本中确切地记录该指标名称：

`wandb.log({"val_loss": validation_loss})`
{% endhint %}

在幕后，此示例配置将使用贝叶斯优化方法来选择用于调用程序的超参数值集。它将使用以下语法启动实验：

```text
python train.py --learning_rate=0.005 --optimizer=adam
python train.py --learning_rate=0.03 --optimizer=sgd
```

{% hint style="info" %}
如果您在脚本中使用argparse，建议您在变量名中使用下划线而不是连字符。
{% endhint %}

## 3. **初始化扫描**

我们的中央服务器在执行扫描的所有代理之间进行协调。设置扫描配置文件并运行以下命令以开始使用：

```text
wandb sweep sweep.yaml
```

该命令将打印出扫描ID。复制该文件以用于下一步！

## 4. **启动代理**

在要执行扫描的每台计算机上，启动具有扫描ID的代理。您将对执行相同扫描的所有代理使用相同的扫描ID。

在您自己的计算机上的壳层\(shell\)中，运行wandb 代理命令，该命令将询问服务器要运行的命令：

```text
wandb agent your-sweep-id
```

您可以在多台计算机上或在同一台计算机上的多个进程中运行wandb代理，并且每个代理都会轮询中央W＆B扫描服务器以查找下一组要运行的超参数。

## 5. **.可视化结果**

打开项目以在扫描仪表板中查看实时结果。

[Example dashboard →](https://app.wandb.ai/carey/pytorch-cnn-fashion)

![](../../.gitbook/assets/image%20%2880%29.png)

![](../../.gitbook/assets/image%20%2850%29.png)

{% hint style="info" %}
Already have a Weights & Biases project? [Skip to our next Sweeps tutorial →](../../sweeps/add-to-existing.md)
{% endhint %}

