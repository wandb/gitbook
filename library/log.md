---
description: 指標、ビデオ、カスタムプロットなどを追跡します
---

# wandb.log\(\)

**`wandb.log（dict）`を呼び出して、メトリックまたはカスタムオブジェクトのディクショナリをステップに記録します。デフォルトでは、ステップを毎回インクリメントするため、モデルの出力がグラフと豊富な視覚化で時間の経過とともに表示されます。wandb.log（dict）を呼び出して、メトリックまたはカスタムオブジェクトのディクショナリをステップに記録します。デフォルトでは、ステップを毎回インクリメントするため、モデルの出力がグラフと豊富な視覚化で時間の経過とともに表示されます。**

**キーワード引数**

*    **step**—**ログを関連付けるタイムステップ（**[インクリメンタルログ](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MNT_hJubmcvhzLDyMFV/v/japanese/library/log#incremental-logging)**を参照）**
*  **commit**—**デフォルトではcommit=trueです。これは、wandb.logを呼び出すたびにステップをインクリメントすることを意味します。commit=falseを設定して、複数の連続したwandb.log（）コマンドでデータを同じステップに保存します。**

**使用例**

```python
wandb.log({'accuracy': 0.9, 'epoch': 5})
```

## ロギングオブジェクト

 画像、ビデオ、オーディオ、カスタムグラフなどをサポートしています。リッチメディアをログに記録して、結果を調査し、実行間の比較を視覚化します。

[ ](https://colab.research.google.com/drive/15MJ9nLDIXRvy_lCwAou6C2XN3nppIeEK) [Colabでお試しください](https://colab.research.google.com/drive/15MJ9nLDIXRvy_lCwAou6C2XN3nppIeEK)

### ヒストグラム 

```python
wandb.log({"gradients": wandb.Histogram(numpy_array_or_sequence)})
wandb.run.summary.update({"gradients": wandb.Histogram(np_histogram=np.histogram(data))})
```

シーケンスが最初の引数として指定されている場合、ヒストグラムを自動的にビニングします。np.histogramから返されたものをnp\_histogramキーワード引数に渡して、独自のビニングを行うこともできます。サポートされるビンの最大数は512です。シーケンスを渡すときにオプションのnum\_binsキーワード引数を使用して、デフォルトの64ビンをオーバーライドできます。

  ヒストグラムが要約に含まれている場合、個々の実行ページにスパークラインとして表示されます。それらが履歴にある場合は、時間の経過に伴うビンのヒートマップをプロットします。

###  画像とオーバーレイ

{% tabs %}
{% tab title="画像" %}
`wandb.log({"examples": [wandb.Image(numpy_array_or_pil, caption="Label")]})`

numpy配列が指定されている場合、最後の次元が1の場合はグレースケール、3の場合はRGB、4の場合はRGBAと見なします。配列にfloatが含まれている場合は、0〜255のintに変換します。モードは手動で指定できます。または、`PIL.Image`を指定します。1ステップあたり50枚未満の画像をログに記録することをお勧めします。
{% endtab %}

{% tab title="Segmentation Mask" %}
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

{% tab title="Bounding Box" %}
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

### Media

{% tabs %}
{% tab title="メディア" %}
```python
wandb.log({"examples": [wandb.Audio(numpy_array, caption="Nice", sample_rate=32)]})
```

ステップごとにログに記録できるオーディオクリップの最大数は100です。
{% endtab %}

{% tab title="オーディオ" %}
```python
wandb.log({"video": wandb.Video(numpy_array_or_path_to_video, fps=4, format="gif")})
```

If a numpy array is supplied we assume the dimensions are: time, channels, width, height. By default we create a 4 fps gif image \(ffmpeg and the moviepy python library is required when passing numpy objects\). Supported formats are "gif", "mp4", "webm", and "ogg". If you pass a string to `wandb.Video` we assert the file exists and is a supported format before uploading to wandb. Passing a BytesIO object will create a tempfile with the specified format as the extension.

On the W&B runs page, you will see your videos in the Media section.
{% endtab %}

{% tab title="テキストテーブル" %}
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

{% tab title="分子" %}
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

###  カスタムチャート

これらのプリセットには`wandb.plot`メソッドが組み込まれており、スクリプトから直接グラフをログに記録し、UIで探している正確な視覚化をすばやく確認できます。

{% tabs %}
{% tab title="折れ線グラフ" %}
`wandb.plot.line()`

カスタム折れ線グラフをログに記録します。これは、任意の軸xおよびy上の接続され順序付けられた点（x、y）のリストです。

```python
data = [[x, y] for (x, y) in zip(x_values, y_values)]
table = wandb.Table(data=data, columns = ["x", "y"])
wandb.log({"my_custom_plot_id" : wandb.plot.line(table, "x", "y", title="Custom Y vs X Line Plot")})
```

これを使用して、任意の2次元の曲線をログに記録できます。2つの値のリストを相互にプロットする場合、リスト内の値の数は正確に一致する必要があることに注意してください（つまり、各ポイントにはxとyが必要です）。

![](../.gitbook/assets/line-plot.png)

 [アプリで見る→](https://wandb.ai/wandb/plots/reports/Custom-Line-Plots--VmlldzoyNjk5NTA)

[コードを実行する→](https://tiny.cc/custom-charts)
{% endtab %}

{% tab title="散布図" %}
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

{% tab title="棒グラフ" %}
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

{% tab title="ヒストグラム" %}
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

{% tab title="PR曲線" %}
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

{% tab title="ROC曲線" %}
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

### **カスタムプリセット**

組み込みのカスタムチャートプリセットを微調整するか、新しいプリセットを作成してからチャートを保存します。チャートIDを使用して、スクリプトから直接そのカスタムプリセットにデータを記録します。

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

 [コードを実行する→](https://tiny.cc/custom-charts)

### Matplotlib

```python
import matplotlib.pyplot as plt
plt.plot([1, 2, 3, 4])
plt.ylabel('some interesting numbers')
wandb.log({"chart": plt})
```

`matplotlib`のpyplotまたはfigureオブジェクトを`wandb.log（）`に渡すことができます。デフォルトでは、プロットをPlotlyプロットに変換します。プロットを画像として明示的にログに記録する場合は、プロットを`wandb.Image`に渡すことができます。Plotlyチャートを直接ログに記録することもできます。

###  3Dビジュアライゼーション

{% tabs %}
{% tab title="3Dオブジェクト" %}
Log files in the formats `obj`, `gltf`, or `glb`, and we will render them in the UI when your run finishes.

```python
wandb.log({"generated_samples":
           [wandb.Object3D(open("sample.obj")),
            wandb.Object3D(open("sample.gltf")),
            wandb.Object3D(open("sample.glb"))]})
```

![Ground truth and prediction of a headphones point cloud](../.gitbook/assets/ground-truth-prediction-of-3d-point-clouds.png)

[See a live example →](https://app.wandb.ai/nbaryd/SparseConvNet-examples_3d_segmentation/reports/Point-Clouds--Vmlldzo4ODcyMA)
{% endtab %}

{% tab title="点群" %}
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

##  インクリメンタルログ

コード内のさまざまな場所から単一の履歴ステップにログを記録する場合は、次のようにステップインデックスを`wandb.log（）`に渡すことができます。

```python
wandb.log({'loss': 0.2}, step=step)
```

 ステップに同じ値を渡し続ける限り、W＆Bは各呼び出しからキーと値を1つの統合ディクショナリに収集します。前のステップとは異なるステップの値を使用して`wandb.log（）`を呼び出すとすぐに、W＆Bは収集されたすべてのキーと値を履歴に書き込み、収集を最初からやり直します。これは、ステップ0、1、2、...の連続した値でのみこれを使用する必要があることを意味することに注意してください。この機能では、必要な履歴ステップに絶対に書き込むことはできず、「現在」と「次」のステップのみに書き込むことができます。`wandb.log`で**commit=False**を設定してメトリックを累積することもできます。メトリックを持続化するには、**commit**フラグなしで必ず`wandb.log`を呼び出してください。

```python
wandb.log({'loss': 0.2}, commit=False)
# Somewhere else when I'm ready to report this step:
wandb.log({'accuracy': 0.8})
```

##  メトリックサマリー

要約統計量は、モデルごとに単一のメトリックを追跡するために使用されます。サマリーメトリックが変更されると、更新された状態のみが保存されます。手動で変更しない限り、要約は最後に追加された履歴行に自動的に設定されます。メトリックサマリーを変更すると、最後に設定された値のみが保持されます。

```python
wandb.init(config=args)

best_accuracy = 0
for epoch in range(1, args.epochs + 1):
  test_loss, test_accuracy = test()
  if (test_accuracy > best_accuracy):
    wandb.run.summary["best_accuracy"] = test_accuracy
    best_accuracy = test_accuracy
```

トレーニングが完了した後、評価メトリックを実行サマリーに保存することをお勧めします。サマリーは、numpy配列、pytorchテンソル、またはtensorflowテンソルを処理できます。値がこれらのタイプの1つである場合、テンソル全体をバイナリファイルに持続化し、最小、平均、分散、95％パーセンタイルなどの高レベルのメトリックをサマリーオブジェクトに格納します。

```python
api = wandb.Api()
run = api.run("username/project/run_id")
run.summary["tensor"] = np.random.random(1000)
run.summary.update()
```

### ログに直接アクセスします

 履歴オブジェクトは、wandb.logによってログに記録されたメトリックを追跡するために使用されます。`run.history.row`を介して、メトリックの変更可能なディクショナリにアクセスできます。`run.history.add`または`wandb.log`が呼び出されると、行が保存され、新しい行が作成されます。

#### Tensorflowの例

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

####  PyTorchの例

```python
# Start pytorch training
wandb.init(config=args)

for epoch in range(1, args.epochs + 1):
  train_loss = train(epoch)
  test_loss, test_accuracy = test()

  torch.save(model.state_dict(), 'model')

  wandb.log({"loss": train_loss, "val_loss": test_loss})
```

## よくある質問

### **異なる時代の画像を比較します**

 ステップから画像をログに記録するたびに、UIに表示するために画像を保存します。画像パネルを固定し、ステップスライダーを使用して、さまざまなステップの画像を表示します。これにより、トレーニング中にモデルの出力がどのように変化するかを簡単に比較できます。

```python
wandb.log({'epoch': epoch, 'val_acc': 0.94})
```

### バッチログ

#### すべてのバッチで特定のメトリックをログに記録し、プロットを標準化する場合は、メトリックでプロットするx軸の値をログに記録できます。次に、カスタムプロットで、\[編集\]をクリックして、カスタムx軸を選択します。

```python
wandb.log({'batch': 1, 'loss': 0.3})
```

### **PNGをログ**

wandb.Imageは、デフォルトでPILImageのnumpy配列またはインスタンスをPNGに変換します。

```python
wandb.log({"example": wandb.Image(...)})
# Or multiple images
wandb.log({"example": [wandb.Image(...) for img in images]})
```

### **JPEGをログ**

JPEGを保存するには、ファイルへのパスを渡すことができます。

```python
im = PIL.fromarray(...)
rgb_im = im.convert('RGB')
rgb_im.save('myimage.jpg')
wandb.log({"example": wandb.Image("myimage.jpg")})
```

### **ビデオをログ**

```python
wandb.log({"example": wandb.Video("myvideo.mp4")})
```

これで、メディアブラウザでビデオを表示できます。プロジェクトワークスペースに移動し、ワークスペースを実行するか、レポートを作成し、\[視覚化の追加\]をクリックしてリッチメディアパネルを追加します。

###  カスタムx軸

 デフォルトでは、wandb.logを呼び出すたびにグローバルステップがインクリメントされます。必要に応じて、単調に増加する独自のステップをログに記録し、それをグラフのカスタムx軸として選択できます。たとえば、調整したいトレーニングと検証のステップがある場合は、独自のステップカウンター`wandb.log({“acc”:1, “global_step”:1})`を渡してください。次に、グラフでx軸として「global\_step」を選択します。`wandb.log({“acc”:1,”batch”:10}, step=epoch)`を使用すると、デフォルトのステップ軸に加えて、x軸として「バッチ」を選択できます。

### 点群のナビゲートとズームイン

コントロールを押したまま、マウスを使用してスペース内を移動できます

### グラフには何も表示されません

「視覚化データがまだログに記録されていません」と表示されている場合は、スクリプトから最初のwandb.log呼び出しをまだ取得していないことを意味します。これは、実行がステップを完了するのに長い時間がかかることが原因である可能性があります。各エポックの終わりにログを記録している場合は、エポックごとに数回ログを記録して、データストリームをより迅速に確認できます。

### **メトリック名が重複しています**

同じキーで異なるタイプをログに記録する場合は、データベースでそれらを分割する必要があります。これは、UIのドロップダウンに同じメトリック名の複数のエントリが表示されることを意味します。グループ化するタイプは、数値、文字列、ブール、その他（主に配列）、および任意のwandbタイプ（ヒストグラム、画像など）です。この動作を回避するために、各キーに1つのタイプのみを送信してください。

### パフォーマンスおよび制限

### **サンプリング**

送信するポイントが多いほど、UIにグラフを読み込むのに時間がかかります。1行に1000ポイントを超える場合は、ブラウザにデータを送信する前に、バックエンドで1000ポイントまでサンプリングします。このサンプリングは非決定的であるため、ページを更新すると、サンプリングされたポイントの異なるセットが表示されます。すべての元のデータが必要な場合は、データAPIを使用してサンプリングされていないデータをプルダウンできます。 

**ガイドライン**

メトリックごとに10,000ポイント未満をログに記録することをお勧めします。500列を超える構成および要約メトリックがある場合、テーブルには500のみが表示されます。1行に100万ポイントを超えるログを記録すると、ページの読み込みに時間がかかります。メトリックは大文字と小文字を区別せずに保存されるため、「My-Metric」と「my-metric」のように同じ名前の2つのメトリックがないことを確認してください。

### 画像のアップロードをコントロール

W＆Bをプロジェクトに統合したいのですが、画像はアップロードしたくありません」当社の統合では、画像は自動的にアップロードされません。明示的にアップロードするファイルを指定します。これは、画像を明示的にログに記録するPyTorch用に作成した簡単な例です。[http://bit.ly/pytorch-mnist-colab](http://bit.ly/pytorch-mnist-colab)

```python
wandb.log({
        "Examples": example_images,
        "Test Accuracy": 100. * correct / len(test_loader.dataset),
        "Test Loss": test_loss})
```

