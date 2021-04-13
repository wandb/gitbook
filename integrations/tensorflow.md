# Tensorflow

如果你已经在用TensorBoard，与wandb集成很容易。

```python
import tensorflow as tf
import wandb
wandb.init(config=tf.flags.FLAGS, sync_tensorboard=True)
```

请参阅我们的[示例项目](https://docs.wandb.ai/v/zh-hans/examples)以获得完整的脚本示例。

## **自定义指标**

如果你需要额外记录一些没有被记录到TensorBoard的指标，可以在代码中调用`wandb.log`，并使用与TensorBoard一样的step参数：

即`wandb.log({"custom": 0.8}, step=global_step)`

##  **TensorFlow钩子**

如果你想对被记录的内容有更多的控制，wandb还为TensorFlow估算器提供了一个钩子。它将在图中记录所有的`tf.summary`值。

```python
import tensorflow as tf
import wandb

wandb.init(config=tf.FLAGS)

estimator.train(hooks=[wandb.tensorflow.WandbHook(steps_per_log=1000)])
```

## **手动记录**

 在TensorFlow中记录指标的最简单方法就是用TensorFlow记录器记录`tf.summary`。

```python
import wandb

with tf.Session() as sess:
    # ...
    wandb.tensorflow.log(tf.summary.merge_all())
```

 对于TensorFlow2，如果你要用自定义循环来训练模型，我们建议使用`tf.GradientTape`。 你可以在[这里](https://www.tensorflow.org/tutorials/customization/custom_training_walkthrough)阅读更多关于它的信息。如果你想在你的自定义TensorFlow训练循环中加入`wandb`来记录指标，你可以参考这段代码——

```python
    with tf.GradientTape() as tape:
        # Get the probabilities
        predictions = model(features)
        # Calculate the loss
        loss = loss_func(labels, predictions)

    # Log your metrics
    wandb.log("loss": loss.numpy())
    # Get the gradients
    gradients = tape.gradient(loss, model.trainable_variables)
    # Update the weights
    optimizer.apply_gradients(zip(gradients, model.trainable_variables))
```

[这里](https://www.wandb.com/articles/wandb-customizing-training-loops-in-tensorflow-2)有个完整示例。

## **W&B和TensorBoard有什么不同？**

我们受到启发要为大家改进实验跟踪工具。当合伙人开始构建W&B的工作时，他们受到启发要为OpenAI中受挫的TensorBoard 用户打造一个工具。以下是我们重点改进的几项内容：

1.  **重现模型**: Weights & Biases有利于实验、探索和以后重现模型。我们不仅可以捕获指标（Metric）,还可以捕获超参数和代码版本，我们还可以为你保存你的模型检查点，这样你的项目可以被重现。
2. **自动组织**: 如果你将一个项目交给一个同事或者你要去休假。W&B使得很容易查看所有你试过的实验，这样就不用浪费时间重新运行旧实验。
3.    **快速、灵活的集成**: 5分钟之内就可以将W&B添加到你的项目中。安装我们的免费开源Python包，并在你的代码中添加几行代码。然后每次你运行你的模型时，你的指标和记录都会被很好记录。 
4.  **持久的集中式仪表盘**: 无论你在哪里训练模型，无论是在你的本地机器、你的实验室集群或云端的竞价实例（Spot Instance）中，我们都会为你提供相同的集中式仪表盘。你不需要花费时间从不同的机器上复制和组织TensorBoard文件。
5.  **强大的表格**: 搜索、过滤、排序和分组不同模型的结果。很容易查看成千上万的模型版本，并为不同的任务找到性能最好的模型。TensorBoard无法在大型项目上很好工作。
6.   **协作的工具**: 使用W&B来组织复杂的机器学习项目。分享W&B链接非常容易，你可以使用私有（private）团队,让每个人将结果发送到共享项目。我们还支持通过报告进行协作——添加交互式可视化，并用markdown描述你的工作。这是保留工作日志，与你的上级分享发现，或向你的实验室展示发现的好方法。

 开设一个[个人免费账号→](http://app.wandb.ai/)​​

## **示例**

我们已经为你创建了一些示例，以了解集成的工作原理：

*   [Github上的示例](https://github.com/wandb/examples/blob/master/examples/tensorflow/tf-estimator-mnist/mnist.py)：使用TensorFlow估算器的MNIST示例
*   [Github](https://github.com/wandb/examples/blob/master/examples/tensorflow/tf-cnn-fashion/train.py)上的示例: 使用Raw TensorFlow的Fashion MNIST示例
*   [Wandb仪](https://app.wandb.ai/l2k2/examples-tf-estimator-mnist/runs/p0ifowcb)表盘: 在W&B上查看结果
* 自定义TensorFlow 2 中的训练循环 - 文章\| [Colab 笔记本](https://colab.research.google.com/drive/1JCpAbjkCFhYMT7LCQ399y35TS3jlMpvM)\| 仪表盘​​

