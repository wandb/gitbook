# Keras

利用Keras的回调函数callback，自动保存`model.fit`记录的全部指标和损失值。

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

 [范例项目](https://app.gitbook.com/@weights-and-biases/s/docs/examples)中有一个完整的脚本范例，可前往查看。

####  **选项**

Keras的类`WandbCallback()`支持许多选项：

| 关键字参数 | 默认值 | 说明 |
| :--- | :--- | :--- |
| monitor | val\_loss |  用来评估性能的训练指标，以便于保存最佳模型，例如val\_loss。 |
| mode | auto | '“max”或“auto”：如何在不同时间步之间比较`monitor`指定的训练指标。 |
| save\_weights\_only | False |  仅保存权值，不保存整个模型。 |
| save\_model | True |  如果模型在每一步都被批准了，就保存该模型。 |
| log\_weights | False | 记录每个历元中每一层的参数值。 |
| log\_gradients | False |  记录每个历元中每一层的参数梯度。 |
| training\_data | None |  需要元组\(x,y\)用于计算梯度。 |
| data\_type | None | 我们正保存的数据类型，目前仅支持图像“image”。 |
| labels | None | 只有指定了data\_type才会用到，如果你正在做分类器，会把数据型输出转化为一系列标签。（支持二元分类。） |
| predictions | 36 | 如果指定了data\_type，要做多少次预测。最大为100 |
| generator | None | 如果用了数据扩增和data\_type，你可以指定一个生成器来做预测。 |

##  **常见问题**

###  **通过wandb使用Keras多进程**

如果你设置`use_multiprocessing=True`，然后收到错误`Error('You must call wandb.init() before wandb.config.batch_size')`，意思为错误`（‘你必须在wandb.config.batch_size前面调用wandb.init()’）`，那就尝试下列方法：

1. In the Sequence class init, add: `wandb.init(group='...')`  在Sequence类的初始化中，加入：`wandb.init(group='...')`
2.  在主程序中，确保用的是`if name == "main"`:然后把你的脚本逻辑的剩余部分放进去。

##  **范例**

 我们准备了几个例子，让大家看看集成的效果：

* [Github范例](https://github.com/wandb/examples/blob/master/examples/keras/keras-cnn-fashion/train.py)：Fashion MNIST范例
* Run in Google Colab: A simple notebook example to get you started
*  [wandb指示板](https://wandb.ai/wandb/keras-fashion-mnist/runs/5z1d85qs)：在权阈中查看结果

