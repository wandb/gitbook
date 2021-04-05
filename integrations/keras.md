# Keras

利用Keras的回调函数callback，自动保存所有指标和`model.fit`跟踪的损失值。

{% code title="example.py" %}
```python
import wandb
from wandb.keras import WandbCallback
wandb.init(config={"hyper": "parameter"})

# Magic

model.fit(X_train, y_train,  validation_data=(X_test, y_test),
          callbacks=[WandbCallback()])
```
{% endcode %}

在[colab笔记本](https://colab.research.google.com/github/wandb/examples/blob/master/colabs/keras/Simple_Keras_Integration.ipynb)中尝试我们的集成，并提供了完整的[视频教程](https://www.youtube.com/watch?v=Bsudo7jbMow&ab_channel=Weights%26Biases)，或查看我们的示例项目以获得完整的脚本示例。 

#### **选项**

 Keras的类`WandbCallback()`支持许多选项：

| 关键字参数 | 默认值 | 说明 |
| :--- | :--- | :--- |
| monitor | val\_loss | 用来评估性能的训练指标，以便于保存最佳模型，例如val\_loss。 |
| mode | auto | “min”，“max”或“auto”：如何在不同步（step）之间比较`monitor`指定的训练指标。 |
| save\_weights\_only | False | 只保存权重而不是整个模型 |
| save\_model | True | 如果每一步（step）都有改进，就保存该模型。 |
| log\_weights | False | 记录各层参数在每个周期（epoch）的参数值 |
| log\_gradients | False | 记录每个周期中各层的参数梯度。 |
| training\_data | None | 需要多元组\(x,y\)用于计算梯度。 |
| data\_type | None | 我们正保存的数据类型，目前仅支持图像“image”。 |
| labels | None | 只有指定了data\_type才会用到，如果你要做分类器，要把数字输出转化为标签列表。（支持二元分类器。 |
| predictions | 36 | 如果指定了data\_type，预测的次数。最大为100 |
| generator | None | 如果用数据扩增和data\_type，你可以指定一个生成器来做预测。 |

## **常见问题**

### **通过wandb使用Keras的multiprocessing**

如果你设置`use_multiprocessing=True`，然后收到错误`Error('You must call wandb.init() before wandb.config.batch_size')`，意思为错误`（‘你必须在wandb.config.batch_size前调用wandb.init()’）`，那么可以尝试下列方法：

1.  在Sequence类init中, 添加: `wandb.init(group='...')`
2. 在主程序中，确保使用`if __name__ == "__main__":`然后把你剩下的脚本逻辑的部分放进去。

## **示例**

我们已经为你创建了一些示例，以了解集成的工作原理：

* [Github上的示例](https://github.com/wandb/examples/blob/master/examples/keras/keras-cnn-fashion/train.py)：Python脚本中的Fashion MNIST示例
* 在Google Colab中运行: 一个简单的笔记本示例让你入门
* [Wandb仪表盘](https://wandb.ai/wandb/keras-fashion-mnist/runs/5z1d85qs)：在W&B上查看结果

