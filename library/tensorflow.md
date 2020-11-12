# Tensorflow

 如果你已经在用TensorBoard，轻而易举就可以集成到wandb。

```python
import tensorflow as tf
import wandb
wandb.init(config=tf.flags.FLAGS, sync_tensorboard=True)
```

 [范例项目](https://app.gitbook.com/@weights-and-biases/s/docs/examples)中有一个完整的脚本范例，可前往查看。

##  **自定义指标**

如果你需要额外记录一些没有被记录到TensorBoard的指标，可以在代码中调用`wandb.log`，并使用与TensorBoard一样的步长参数：即`wandb.log({"custom": 0.8}, step=global_step)`

## **TensorFlow钩子**

如果你想更多地控制记录哪些东西，权阈还为TensorFlow估算器提供了钩子。这个钩子会把`tf.summary`的全部值记录到图表中

```python
import tensorflow as tf
import wandb

wandb.init(config=tf.FLAGS)

estimator.train(hooks=[wandb.tensorflow.WandbHook(steps_per_log=1000)])
```

## **手动记录**

 在TensorFlow记录指标的最简单方法就是用TensorFlow记录器记录`tf.summary`。

```python
import wandb

with tf.Session() as sess:
    # ...
    wandb.tensorflow.log(tf.summary.merge_all())
```

对于TensorFlow2，如果你要用自定义循环来训练模型，我们建议使用`tf.GradientTape`。可在这里深入了解。在你的自定义TensorFlow训练循环中，如果你想把`wandb`合并进来，以便于记录指标，就可以参考这段代码——

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

 [这里](https://www.wandb.com/articles/wandb-customizing-training-loops-in-tensorflow-2)有个完整范例。

##  **权阈和TensorBoard有什么不同？**

我们决心要为每个人改进实验跟踪工具。当联合创始人们开始创立权阈，他们就很想在OpenAI为垂头丧气的TensorBoard用户开发一个工具。我们重点改进以下几方面：

1. **重新制作模型**：权阈有利于实验、探索、以后重新制作模型。我们不仅捕捉指标，还捕捉超参数和代码版本，并且，我们会保存你的模型检查点，因而你的项目是可再生的。
2. **自动组织**：如果你把项目交给同事或者要去度假，权阈可以让你便捷地查看你制作的所有模型，你就不必花费大量时间来重新运行旧实验。
3. **快速、灵活的集成**：只需5分钟即可把权阈加到自己的项目。下载我们免费的开源Python包，然后在代码中插入几行，以后你每次运行模型都会得到记录完备的指标和记录。
4.  **持续、集中式指示板**：不管你在哪里训练模型，不管是在本地机器、实验室集群还是在云端竞价实例，我们为您提供同样的集中式指示板。你不必花时间从别的机器上复制、组织TensorBoard文件。
5. . **强大的表格**：对不同模型的结果进行搜索、筛选、分类和分组。可以轻而易举地察看成千上万个模型版本，并找到不同任务的最佳模型。TensorBoard本身就不适合大型项目。
6. **协作工具**：用权阈组织复杂的机器学习项目。发送权阈的链接非常简便，你可以使用非公开团队，让每个人把结果发送至共享项目。我们还支持通过报告协作——加入交互式的可视化，在富文本编辑器描述自己的工作成果。这非常便于保存工作日志、与上司分享成果以及向自己的实验室展示成果

 [新手就先注册一个免费个人账号→。](https://wandb.ai/)

## Example **示例**

我们准备了几个例子，让大家看看集成的效果：

*  [Github范例](https://github.com/wandb/examples/blob/master/examples/tensorflow/tf-estimator-mnist/mnist.py)：使用TensorFlow估算器的MNIST范例
* [Example on Github](https://github.com/wandb/examples/blob/master/examples/tensorflow/tf-cnn-fashion/train.py): Fashion MNIST example Using Raw TensorFlow
* [Wandb Dashboard](https://app.wandb.ai/l2k2/examples-tf-estimator-mnist/runs/p0ifowcb): View result on W&B
* Customizing Training Loops in TensorFlow 2 - [Article](https://www.wandb.com/articles/wandb-customizing-training-loops-in-tensorflow-2) \| [Colab Notebook](https://colab.research.google.com/drive/1JCpAbjkCFhYMT7LCQ399y35TS3jlMpvM) \| [Dashboard](https://app.wandb.ai/sayakpaul/custom_training_loops_tf)

