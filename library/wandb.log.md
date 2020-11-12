---
description: 记录指标、视频、自定义图表等等。
---

# wandb.log\(\)

  调用`wandb.log(dict)`可以把指标或自定义对象组成的字典类型数据保存到一个时间步。默认情况下，每次我们会递增步长，因为模型的输出结果是随时间变化的，这样你就能看到输出结果的动态图表和丰富的可视化。

**关键字参数：**

*  **step**——记录要关联到哪个时间步（详见[递增记录](https://app.gitbook.com/@weights-and-biases/s/docs/library/log#incremental-logging)）
* **commit**——默认commit=True，就是每当你调用一次wandb.log，我们就递增步长。若设置commit=False，就是用多条wandb.log\(\)命令序列把数据保存至同一个时间步。

**用例：**

```python
wandb.log({'accuracy': 0.9, 'epoch': 5})
```

## **记录对象**

我们支持图像、视频、音频、自定义图表等等。记录多元媒体，便于查看结果以及可视化不同运行项之间的对比结果。

[ ](https://colab.research.google.com/drive/15MJ9nLDIXRvy_lCwAou6C2XN3nppIeEK)[用Colab试试](https://colab.research.google.com/drive/15MJ9nLDIXRvy_lCwAou6C2XN3nppIeEK)

### **直方图**

```python
wandb.log({"gradients": wandb.Histogram(numpy_array_or_sequence)})
wandb.run.summary.update({"gradients": wandb.Histogram(np_histogram=np.histogram(data))})
```

 如果提供的第一个参数是序列，我们会自动分箱到直方图。你还可以自己做分箱，就是把np.histogram返回的值赋给np\_histogram关键字参数。所支持的最大箱子数为512.你可以用可选的num\_bins关键字参数，这时候就会传入一个序列来重写默认的64个箱子。

如果直方图表示的是汇总，就会在单独的运行页以走势图的形式出现。如果直方图表示的是历史结果，我们会绘制一个随时间变化的箱子热力图。

### **图像和叠加**

{% tabs %}
{% tab title="图像" %}
`wandb.log({"examples": [wandb.Image(numpy_array_or_pil, caption="Label")]})`

如果提交的是一个numpy数组，我们就这样处理，如果最后一维是1，就是灰度；如果是3，就是RGB；如果是4，就是RGBA。如果数组含有浮点数，我们就转化为0-255的整数。你可以手动指定一个模式，要么就仅提交一个`PIL.Image`。建议每个时间步记录的图像数少于50.
{% endtab %}

{% tab title="分割掩码" %}
If you have images with masks for semantic segmentation, you can log the masks and toggle them on and off in the UI. To log multiple masks, log a mask dictionary with multiple keys. Here's an example:

* **mask\_data**: a 2D numpy array containing an integer class label for each pixel
* **class\_labels**: a dictionary mapping the numbers from **mask\_data** to readable labels

```python
mask_data = np.array([[1, 2, 2, ... , 2, 2, 1], ...])

class_labels = {
  1: "tree",
  2: "car",
  3: "road"
}

mask_img = wandb.Image(image, masks={
  "predictions": {
    "mask_data": mask_data,
    "class_labels": class_labels
  },
  "groud_truth": {
    ...
  },
  ...
})
```

[See a live example →](https://app.wandb.ai/stacey/deep-drive/reports/Image-Masks-for-Semantic-Segmentation--Vmlldzo4MTUwMw)

[Sample code →](https://colab.research.google.com/drive/1SOVl3EvW82Q4QKJXX6JtHye4wFix_P4J)

![](../.gitbook/assets/semantic-segmentation.gif)
{% endtab %}

{% tab title="边界框" %}
Log bounding boxes with images, and use filters and toggles to dynamically visualize different sets of boxes in the UI.

```python
class_id_to_label = {
    1: "car",
    2: "road",
    3: "building",
    ....
}

img = wandb.Image(image, boxes={
    "predictions": {
        "box_data": [{
            "position": {
                "minX": 0.1,
                "maxX": 0.2,
                "minY": 0.3,
                "maxY": 0.4,
            },
            "class_id" : 2,
            "box_caption": "minMax(pixel)",
            "scores" : {
                "acc": 0.1,
                "loss": 1.2
            },
        }, 
        # Log as many boxes as needed
        ...
        ],
        "class_labels": class_id_to_label
    },
    "ground_truth": {
    # Log each group of boxes with a unique key name
    ...
    }
})

wandb.log({"driving_scene": img})
```

Optional Parameters

`class_labels` An optional argument mapping your class\_ids to string values. By default we will generate class\_labels `class_0`, `class_1`, etc...

Boxes - Each box passed into box\_data can be defined with different coordinate systems.

`position`

* Option 1: `{minX, maxX, minY, maxY}` Provide a set of coordinates defining the upper and lower bounds of each box dimension.
* Option 2: `{middle, width, height}`  Provide a set of coordinates specifying the middle coordinates as `[x,y]`, and `width`, and `height` as scalars 

`domain` Change the domain of your position values based on your data representation

* `percentage` \(Default\) A relative value representing the percent of the image as distance
* `pixel`An absolute pixel value

[See a live example →](https://app.wandb.ai/stacey/yolo-drive/reports/Bounding-Boxes-for-Object-Detection--Vmlldzo4Nzg4MQ)

![](../.gitbook/assets/bb-docs.jpeg)
{% endtab %}
{% endtabs %}

### **媒体**

{% tabs %}
{% tab title="音频" %}
```python
wandb.log({"examples": [wandb.Audio(numpy_array, caption="Nice", sample_rate=32)]})
```

 每时间步记录的音频片段数最大数量为100.
{% endtab %}

{% tab title="视频" %}
```python
wandb.log({"video": wandb.Video(numpy_array_or_path_to_video, fps=4, format="gif")})
```

If a numpy array is supplied we assume the dimensions are: time, channels, width, height. By default we create a 4 fps gif image \(ffmpeg and the moviepy python library is required when passing numpy objects\). Supported formats are "gif", "mp4", "webm", and "ogg". If you pass a string to `wandb.Video` we assert the file exists and is a supported format before uploading to wandb. Passing a BytesIO object will create a tempfile with the specified format as the extension.

On the W&B runs page, you will see your videos in the Media section.
{% endtab %}

{% tab title="文本表格" %}
Use wandb.Table\(\) to log text in tables to show up in the UI. By default, the column headers are `["Input", "Output", "Expected"]`. The maximum number of rows is 10,000.

```python
# Method 1
data = [["I love my phone", "1", "1"],["My phone sucks", "0", "-1"]]
wandb.log({"examples": wandb.Table(data=data, columns=["Text", "Predicted Label", "True Label"])})

# Method 2
table = wandb.Table(columns=["Text", "Predicted Label", "True Label"])
table.add_data("I love my phone", "1", "1")
table.add_data("My phone sucks", "0", "-1")
wandb.log({"examples": table})
```

You can also pass a pandas `DataFrame` object.

```python
table = wandb.Table(dataframe=my_dataframe)
```
{% endtab %}

{% tab title="HTML" %}
```python
wandb.log({"custom_file": wandb.Html(open("some.html"))})
wandb.log({"custom_string": wandb.Html('<a href="https://mysite">Link</a>')})
```

Custom html can be logged at any key, this exposes an HTML panel on the run page. By default we inject default styles, you can disable default styles by passing `inject=False`.

```python
wandb.log({"custom_file": wandb.Html(open("some.html"), inject=False)})
```
{% endtab %}

{% tab title="Molecule" %}
```python
wandb.log({"protein": wandb.Molecule(open("6lu7.pdb"))}
```

Log molecular data in any of 10 file types:

`'pdb', 'pqr', 'mmcif', 'mcif', 'cif', 'sdf', 'sd', 'gro', 'mol2', 'mmtf'`

When your run finishes, you'll be able to interact with 3D visualizations of your molecules in the UI.

[See a live example →](https://app.wandb.ai/nbaryd/Corona-Virus/reports/Visualizing-Molecular-Structure-with-Weights-%26-Biases--Vmlldzo2ODA0Mw)

![](../.gitbook/assets/docs-molecule.png)
{% endtab %}
{% endtabs %}

### **自定义图表**

 这些预设有内置的`wandb.plot`方法，能够直接从脚本快速记录图表，在网页界面快速地看到想要的可视化结果。

{% tabs %}
{% tab title="线条图" %}
`wandb.plot.line()`

 记录一个自定义线条图——在x和y任意轴上有序连接的一系列点\(x,y\)

```python
data = [[x, y] for (x, y) in zip(x_values, y_values)]
table = wandb.Table(data=data, columns = ["x", "y"])
wandb.log({"my_custom_plot_id" : wandb.plot.line(table, "x", "y", title="Custom Y vs X Line Plot")})
```

可以用这记录任意二维空间上的曲线。注意，如果你要记录两列值，列表中值的数量必须正好匹配（即每个点必须有一个x和一个y）.

![](../.gitbook/assets/line-plot.png)

 [在应用程序中查看→](https://wandb.ai/wandb/plots/reports/Custom-Line-Plots--VmlldzoyNjk5NTA)

 [运行代码→](https://colab.research.google.com/drive/1uXLKDmsYg7QMRVFyjUAlg-eZH2MW8yWH?usp=sharing)
{% endtab %}

{% tab title="散点图" %}
`wandb.plot.scatter()`

Log a custom scatter plot—a list of points \(x, y\) on a pair of arbitrary axes x and y.

```python
data = [[x, y] for (x, y) in zip(class_x_prediction_scores, class_y_prediction_scores)]
table = wandb.Table(data=data, columns = ["class_x", "class_y"])
wandb.log({"my_custom_id" : wandb.plot.scatter(table, "class_x", "class_y")})
```

You can use this to log scatter points on any two dimensions. Note that if you're plotting two lists of values against each other, the number of values in the lists must match exactly \(i.e. each point must have an x and a y\).

![](../.gitbook/assets/demo-scatter-plot.png)

[See in the app →](https://wandb.ai/wandb/plots/reports/Custom-Scatter-Plots--VmlldzoyNjk5NDQ)

[Run the code →](https://tiny.cc/custom-charts)
{% endtab %}

{% tab title="条形图" %}
`wandb.plot.bar()`

Log a custom bar chart—a list of labeled values as bars—natively in a few lines:

```python
data = [[label, val] for (label, val) in zip(labels, values)]
table = wandb.Table(data=data, columns = ["label", "value"])
wandb.log({"my_bar_chart_id" : wandb.plot.bar(table, "label", "value", title="Custom Bar Chart")
```

You can use this to log arbitrary bar charts. Note that the number of labels and values in the lists must match exactly \(i.e. each data point must have both\).

![](../.gitbook/assets/image%20%2896%29.png)

[See in the app →](https://wandb.ai/wandb/plots/reports/Custom-Bar-Charts--VmlldzoyNzExNzk)

[Run the code →](https://tiny.cc/custom-charts)
{% endtab %}

{% tab title="直方图" %}
`wandb.plot.histogram()`

Log a custom histogram—sort list of values into bins by count/frequency of occurrence—natively in a few lines. Let's say I have a list of prediction confidence scores \(`scores`\) and want to visualize their distribution:

```python
data = [[s] for s in scores]
table = wandb.Table(data=data, columns=["scores"])
wandb.log({'my_histogram': wandb.plot.histogram(table, "scores", title=None)})
```

You can use this to log arbitrary histograms. Note that `data` is a list of lists, intended to support a 2D array of rows and columns.

![](../.gitbook/assets/demo-custom-chart-histogram.png)

[See in the app →](https://wandb.ai/wandb/plots/reports/Custom-Histograms--VmlldzoyNzE0NzM)

[Run the code →](https://tiny.cc/custom-charts)
{% endtab %}

{% tab title="曲线" %}
`wandb.plot.pr_curve()`

Log a [Precision-Recall curve](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_recall_curve.html#sklearn.metrics.precision_recall_curve) in one line:

```python
wandb.log({"pr" : wandb.plot.pr_curve(ground_truth, predictions,
                     labels=None, classes_to_plot=None)})
```

You can log this whenever your code has access to:

* a model's predicted scores \(`predictions`\) on a set of examples
* the corresponding ground truth labels \(`ground_truth`\) for those examples
* \(optionally\) a list of the labels/class names \(`labels=["cat", "dog", "bird"...]` if label index 0 means cat, 1 = dog, 2 = bird, etc.\)
* \(optionally\) a subset \(still in list format\) of the labels to visualize in the plot

![](../.gitbook/assets/demo-precision-recall.png)

[See in the app →](https://wandb.ai/wandb/plots/reports/Plot-Precision-Recall-Curves--VmlldzoyNjk1ODY)

[Run the code →](https://colab.research.google.com/drive/1mS8ogA3LcZWOXchfJoMrboW3opY1A8BY?usp=sharing)
{% endtab %}

{% tab title="ROC曲线" %}
`wandb.plot.roc_curve()`

Log an [ROC curve](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_curve.html#sklearn.metrics.roc_curve) in one line:

```text
wandb.log({"roc" : wandb.plot.roc_curve( ground_truth, predictions, \
                        labels=None, classes_to_plot=None)})
```

You can log this whenever your code has access to:

* a model's predicted scores \(`predictions`\) on a set of examples
* the corresponding ground truth labels \(`ground_truth`\) for those examples
* \(optionally\) a list of the labels/ class names \(`labels=["cat", "dog", "bird"...]` if label index 0 means cat, 1 = dog, 2 = bird, etc.\)
* \(optionally\) a subset \(still in list format\) of these labels to visualize on the plot

![](../.gitbook/assets/demo-custom-chart-roc-curve.png)

[See in the app →](https://wandb.ai/wandb/plots/reports/Plot-ROC-Curves--VmlldzoyNjk3MDE)

[Run the code →](https://colab.research.google.com/drive/1_RMppCqsA8XInV_jhJz32NCZG6Z5t1RO?usp=sharing)
{% endtab %}
{% endtabs %}

### **Custom presets 自定义预设**

改进内置的自定义图表预设，或者创建一个新的预设，然后保存图表。直接从脚本用图表id把数据记录到该自定义预设。

```python
# Create a table with the columns to plot
table = wandb.Table(data=data, columns=["step", "height"])

# Map from the table's columns to the chart's fields
fields = {"x": "step",
          "value": "height"}

# Use the table to populate the new custom chart preset
# To use your own saved chart preset, change the vega_spec_name
my_custom_chart = wandb.plot_table(vega_spec_name="carey/new_chart",
              data_table=table,
              fields=fields,
              )
```

 [运行代码→](https://colab.research.google.com/drive/1uXLKDmsYg7QMRVFyjUAlg-eZH2MW8yWH?usp=sharing)

### Matplotlib

```python
import matplotlib.pyplot as plt
plt.plot([1, 2, 3, 4])
plt.ylabel('some interesting numbers')
wandb.log({"chart": plt})
```

你可以把`matplotlib.pyplot`对象或图形对象赋给`wandb.log()`。默认情况下，我们会把该图表转化为[Plotly](https://plot.ly/)图表。如果你明确要以图像形式记录该图表，则可以把该图表赋给`wandb.Image`。我们还可以直接记录Plotly图表。

### **三维可视化**

{% tabs %}
{% tab title="三维对象" %}
记录文件并存为`obj`、`gltf`或`glb`格式，你的运行项结束后，我们就在用户界面渲染这些文件

```python
wandb.log({"generated_samples":
           [wandb.Object3D(open("sample.obj")),
            wandb.Object3D(open("sample.gltf")),
            wandb.Object3D(open("sample.glb"))]})
```

![Ground truth and prediction of a headphones point cloud](../.gitbook/assets/ground-truth-prediction-of-3d-point-clouds.png)

[See a live example →](https://app.wandb.ai/nbaryd/SparseConvNet-examples_3d_segmentation/reports/Point-Clouds--Vmlldzo4ODcyMA)
{% endtab %}

{% tab title="点云" %}
Log 3D point clouds and Lidar scenes with bounding boxes. Pass in a numpy array containing coordinates and colors for the points to render.

```python
point_cloud = np.array([[0, 0, 0, COLOR...], ...])

wandb.log({"point_cloud": wandb.Object3D(point_cloud)})
```

Three different shapes of numpy arrays are supported for flexible color schemes.

* `[[x, y, z], ...]` `nx3`
* `[[x, y, z, c], ...]` `nx4` `| c is a category` in the range `[1, 14]` \(Useful for segmentation\)
* `[[x, y, z, r, g, b], ...]` `nx6 | r,g,b` are values in the range `[0,255]`for red, green, and blue color channels.

Here's an example of logging code below:

* `points`is a numpy array with the same format as the simple point cloud renderer shown above.
* `boxes` is a numpy array of python dictionaries with three attributes:
  * `corners`- a list of eight corners
  * `label`- a string representing the label to be rendered on the box \(Optional\)
  * `color`- rgb values representing the color of the box 
* `type` is a string representing the scene type to render. Currently the only supported value is `lidar/beta`

```python
# Log points and boxes in W&B
wandb.log(
        {
            "point_scene": wandb.Object3D(
                {
                    "type": "lidar/beta",
                    "points": np.array(
                        [
                            [0.4, 1, 1.3], 
                            [1, 1, 1], 
                            [1.2, 1, 1.2]
                        ]
                    ),
                    "boxes": np.array(
                        [
                            {
                                "corners": [
                                    [0,0,0],
                                    [0,1,0],
                                    [0,0,1],
                                    [1,0,0],
                                    [1,1,0],
                                    [0,1,1],
                                    [1,0,1],
                                    [1,1,1]
                                ],
                                "label": "Box",
                                "color": [123,321,111],
                            },
                            {
                                "corners": [
                                    [0,0,0],
                                    [0,2,0],
                                    [0,0,2],
                                    [2,0,0],
                                    [2,2,0],
                                    [0,2,2],
                                    [2,0,2],
                                    [2,2,2]
                                ],
                                "label": "Box-2",
                                "color": [111,321,0],
                            }
                        ]
                    ),
                    "vectors": [
                        {"start": [0,0,0], "end": [0.1,0.2,0.5]}
                    ]
                }
            )
        }
    )
```
{% endtab %}
{% endtabs %}

## **递增式记录**

如果你想从代码的很多地方记录到一个历史时间步，就可以把一个时间步索引赋值给`wandb.log()`，如下所示：

```python
wandb.log({'loss': 0.2}, step=step)
```

 只要你一直赋给`step`同样的值，权阈就会在同一个字典中收集每次调用的键值对。调用`wandb.log()`时，一旦你赋给`step`的值变化了、和上次不一样了，权阈会把收集的全部键值对写入历史，然后再重新开始收集。注意，你使用该方法时，只能赋给`step`连续的值：0、1、2……该功能绝不允许你随意写入任意一个历史时间步，只能写入“当前”的和“下一个”。

你还可以在`wandb.log`设置**commit=False**，就会累积指标，只需确保在没有**提交**标记的前提下调用`wandb.log`即可保留指标。

```python
wandb.log({'loss': 0.2}, commit=False)
# Somewhere else when I'm ready to report this step:
wandb.log({'accuracy': 0.8})
```

## **汇总指标**

 汇总统计，用来按模型记录单一指标。如果汇总指标更改了，只有最新的状态会被保存。我们会将summary自动设置为最后添加的历史行，除非你手动修改它。如果你修改了某个汇总指标，我们仅保留设置的最后一个值。

```python
wandb.init(config=args)

best_accuracy = 0
for epoch in range(1, args.epochs + 1):
  test_loss, test_accuracy = test()
  if (test_accuracy > best_accuracy):
    wandb.run.summary["best_accuracy"] = test_accuracy
    best_accuracy = test_accuracy
```

训练结束后，你或许想保存运行汇总里的评估指标。`summary`可以处理`numpy`数组、`pytorch`张量、`tensorflow`张量。当某个值属于这些类的其中之一，我们会把整个张量保存到一个二进制文件，并把高级指标保存至`summary`对象，例如最小值、平均值、方差、95%百分位数等等。

```python
api = wandb.Api()
run = api.run("username/project/run_id")
run.summary["tensor"] = np.random.random(1000)
run.summary.update()
```

###  **直接获取日志**

对象用来跟踪`wandb.log`记录的指标。你可以用`run.history.row`获取指标的可变字典。每当调用`run.history.add`或wandb.log，该行就被保存，新的一行被创建。

####  **Tensorflow例子**

```python
wandb.init(config=flags.FLAGS)

# Start tensorflow training
with tf.Session() as sess:
  sess.run(init)

  for step in range(1, run.config.num_steps+1):
      batch_x, batch_y = mnist.train.next_batch(run.config.batch_size)
      # Run optimization op (backprop)
      sess.run(train_op, feed_dict={X: batch_x, Y: batch_y})
      # Calculate batch loss and accuracy
      loss, acc = sess.run([loss_op, accuracy], feed_dict={X: batch_x, Y: batch_y})

      wandb.log({'acc': acc, 'loss':loss}) # log accuracy and loss
```

####  **PyTorch例子**

```python
# Start pytorch training
wandb.init(config=args)

for epoch in range(1, args.epochs + 1):
  train_loss = train(epoch)
  test_loss, test_accuracy = test()

  torch.save(model.state_dict(), 'model')

  wandb.log({"loss": train_loss, "val_loss": test_loss})
```

##  **常见问题**

### **比较不同代（epoch）的图像**

每当你记录一个时间步的图像，我们就保存起来并展示于用户界面。按住图像面板，然后移动**时间步滑块**即可查看不同时间步的图像。这便于比较训练过程中模型的输出如何变化。

```python
wandb.log({'epoch': epoch, 'val_acc': 0.94})
```

###  **批记录**

如果你想记录每批（batch）的某些指标并使图表标准化，则可以记录x轴的值，你要用那些指标绘制x轴。然后再自定义图表中，点击编辑并选择自定义x轴。

```python
wandb.log({'batch': 1, 'loss': 0.3})
```

### **记录png图像**

 默认情况下，wandb.Image把numpy数组或PILImage实例转化为png图像。

```python
wandb.log({"example": wandb.Image(...)})
# Or multiple images
wandb.log({"example": [wandb.Image(...) for img in images]})
```

### **记录jpeg图像**

 若要保存为jpeg图像，你可以给文件传入一个路径：

```python
im = PIL.fromarray(...)
rgb_im = im.convert('RGB')
rgb_im.save('myimage.jpg')
wandb.log({"example": wandb.Image("myimage.jpg")})
```

### **记录视频**

```python
wandb.log({"example": wandb.Video("myvideo.mp4")})
```

 现在你就可以用媒体播放器看视频了。进入你的项目工作区、运行工作区，或者进入报告，然后点击“添加可视化”，从而添加一个富媒体面板。

###  **自定义x轴**

默认情况下，每当你调用wandb.log，我们就递增全局步长。如果你想，你就可以自行记录单调递增步长，然后选择它作为图表上的自定义x轴。

 例如，如果你想要匹配训练和验证步长，那就传给我们你的步长计数器：`wandb.log({“acc”:1, “global_step”:1})`。然后在图表中选择“global\_step”作为x轴。除了默认的时间步轴，`wandb.log({“acc”:1,”batch”:10}, step=epoch)`能让你选择“批”（batch）作为一个x轴。

###  **浏览、缩放点云**

 在点云中，你可以按住“Ctrl”键并使用鼠标移动。

###  **图表中什么都没有**

如果你看到“No visualization data logged yet”（尚未记录可视化数据），说明我们还没有收到你的脚本对wandb.log的调用。这可能因为你的运行项要用很长时间才能完成一个时间步。如果你正在一代（epoch）的结尾记录数据，每一代你都可以记录多次，以便于快速查看数据流。

### **复制指标名称**

如果你在同一键下记录不同类型的数据，我们必须在数据集中把它们分离开。这就意味着，在用户界面中，你将看到下拉框中同一个指标名称有多个入口。我们按照以下类型分类：数字、字符串、布尔及其它（主要为数组），以及wandb类型（直方图、图像等等）。为了避免这种情况，请给每个键只发送一种类型。

### **表现和限制**

**采样**

你给我们发送的点越多，在用户界面中加载图表所用的时间就越长。如果你一条线上有超过1000个点，那么我们在向你的浏览器发送数据之前，在后台采样其中的1000个点。这种采样具有不确定性，所以，如果你刷新页面，就会看到一套不同的采样点。

 如果你想要全部原始数据，可以用我们的数[据API提](https://docs.wandb.com/ref/export-api)取未采样的数据。

####  **使用指南**

 我们建议你尽量每个指标记录少于10,000个点。如果你有超过500列配置指标和汇总指标，我们在表格中仅展示500列。如果你一条线记录了超过1百万个点，我们要花很长时间才能载入页面。

 我们保存指标时不分大小写，所以要确保指标没有相同的名称，例如“My-Metric”和“my-metric”。

###  **控制图像上传**

"我想把权阈集成到我的项目，但我不想上传图像"

我们的集成不会自动上传图像——由你明确指定要上传哪些文件。下面是一个简短示例，用的是PyTorch，我指明要记录图像：[http://bit.ly/pytorch-mnist-colab](http://bit.ly/pytorch-mnist-colab)

```python
wandb.log({
        "Examples": example_images,
        "Test Accuracy": 100. * correct / len(test_loader.dataset),
        "Test Loss": test_loss})
```

