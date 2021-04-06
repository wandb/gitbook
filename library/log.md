---
description: 指標、ビデオ、カスタムプロットなどをトラッキングします
---

# wandb.log\(\)

`wandb.log（dict）`を呼び出して、メトリックまたはカスタムオブジェクトのディクショナリをステップに記録します。デフォルトでは、ステップを毎回インクリメントするため、メトリックが時間の経過とともに視覚化されます。

### **使用例** <a id="example-usage"></a>

```text
wandb.log({'accuracy': 0.9, 'epoch': 5})
```

### **一般的なワークフロー** <a id="common-workflows"></a>

1. **精度の最高値を比較する**: 実行全体におけるメトリックの最高値を比較するには、そのメトリックの集計値を設定します。集計値は、デフォルトではキーごとにログに記録した最新値に設定されています。これは、集計メトリックに基づいて実行の並べ替えやフィルタリングができるので、UIのテーブルで便利です。最終的な精度ではなく、「最高」の精度に基づいてテーブルやグラフで実行を比較できます。 たとえば、この集計は次のように設定できます。`wandb.run.summary["accuracy"] = best_accuracy`
2. **1つのチャートに複数のメトリック：** `wandb.log（）`への同一の呼び出しで、複数のメトリックをログに記録します。例： `wandb.log({'acc': 0.9, 'loss': 0.1})`　この場合、２つともUIでプロットできるようになります。
3. **カスタムx軸：**同一のログコールにカスタムx軸を追加すると、W＆Bダッシュボードの異なる軸に対するメトリックを視覚化できます。 例：`wandb.log({'acc': 0.9, 'custom_step': 3})`

### **参照ドキュメント** <a id="reference-documentation"></a>

参照ドキュメントをご覧ください。（wandb Pythonライブラリで生成したものです。）

## **オブジェクトのロギング** <a id="logging-objects"></a>

画像、ビデオ、オーディオ、カスタムグラフなどをサポートしています。リッチメディアをログに記録して、結果を調査し、実行間の比較を視覚化します。

​​[ ](https://colab.research.google.com/drive/15MJ9nLDIXRvy_lCwAou6C2XN3nppIeEK)[Colabでお試しください](https://colab.research.google.com/drive/15MJ9nLDIXRvy_lCwAou6C2XN3nppIeEK)​

### **ヒストグラム**  <a id="histograms"></a>

```text
wandb.log({"gradients": wandb.Histogram(numpy_array_or_sequence)})wandb.run.summary.update({"gradients": wandb.Histogram(np_histogram=np.histogram(data))})
```

シーケンスが最初の引数として指定されている場合、ヒストグラムを自動的にビニングします。np.histogramから返されたものをnp\_histogramのキーワード引数に渡して、独自のビニングを行うこともできます。サポートされるビンの最大数は512です。シーケンスを渡すときにオプションのnum\_binsのキーワード引数を使用して、デフォルトの64ビンをオーバーライドできます。

ヒストグラムが集計に含まれている場合、個々の実行ページにスパークラインとして表示されます。もし履歴にあれば、時間の経過に伴うビンのヒートマップがプロットされます。

### **画像とオーバーレイ** <a id="images-and-overlays"></a>

画像

セグメンテーションマスク境界ボックス

`wandb.log({"examples": [wandb.Image(numpy_array_or_pil, caption="Label")]})`

**インクリメンタルログ**

さまざまなx軸に対してメトリックをプロットする場合は、wandb.log\({'loss': 0.1, 'epoch': 1, 'batch': 3}\)のように、ステップをメトリックとしてログに記録できます。UIでは、チャート設定でx軸間の切り替えができます。

コード内のさまざまな場所から単一の履歴ステップにログを記録する場合は、次のようにステップインデックスをwandb.log（）に渡すことができます。

### Custom Charts <a id="custom-charts"></a>

These presets have builtin `wandb.plot` methods that make it fast to log charts directly from your script and see the exact visualizations you're looking for in the UI.LineScatterBarHistogramPRROCConfusion MatrixMulti-line

`wandb.plot.line()`

x-y 軸の共有セット1つに、複数の線分、または x-Y 座標ペアの複数の異なるリストをプロットします。

```text
data = [[x, y] for (x, y) in zip(x_values, y_values)]table = wandb.Table(data=data, columns = ["x", "y"])wandb.log({"my_custom_plot_id" : wandb.plot.line(table, "x", "y",           title="Custom Y vs X Line Plot")})
```

xとyポイントの数は正確に一致する必要があります。y値の複数リストに一致するようにx値のリストを1つ指定するか、y値の各リストに個別のx値リストを指定できます。![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-MJaG8HZKyQEhoXx6nli%2F-MJaHVHvrobTQ-nsSj8Z%2Fline%20plot.png?alt=media&token=34bc94f4-16ed-44dd-acb6-773d7b5ec1ed)

​   [アプリで見る →](https://wandb.ai/wandb/plots/reports/Custom-Multi-Line-Plots--VmlldzozOTMwMjU)​

### **カスタムプリセット** <a id="custom-presets"></a>

 組み込みのカスタムチャートプリセットを微調整するか、新しいプリセットを作成してから、チャートを保存します。チャートIDを使用して、スクリプトから直接そのカスタムプリセットにデータを記録します。

```text
# Create a table with the columns to plottable = wandb.Table(data=data, columns=["step", "height"])​# Map from the table's columns to the chart's fieldsfields = {"x": "step",          "value": "height"}​# Use the table to populate the new custom chart preset# To use your own saved chart preset, change the vega_spec_namemy_custom_chart = wandb.plot_table(vega_spec_name="carey/new_chart",              data_table=table,              fields=fields,              )
```

​[コードを実行する→](https://tiny.cc/custom-charts)​​

### Matplotlib <a id="matplotlib"></a>

```text
import matplotlib.pyplot as pltplt.plot([1, 2, 3, 4])plt.ylabel('some interesting numbers')wandb.log({"chart": plt})
```

 `matplotlib`のpyplotまたはfigureオブジェクトを`wandb.log（）`に渡すことができます。デフォルトでは、プロットを[Plotly](https://plot.ly/)プロットに変換します。プロットを画像として明示的にログに記録する場合は、プロットを`wandb.Image`に渡すことができます。Plotlyチャートを直接ログに記録することもできます。

### **3Dビジュアライゼーション** <a id="3d-visualizations"></a>

3Dオブジェクト

`obj`、`gltf`、または`glb`形式でログファイルを作成し、実行が完了したらUIに表示します。

```text
wandb.log({"generated_samples":           [wandb.Object3D(open("sample.obj")),            wandb.Object3D(open("sample.gltf")),            wandb.Object3D(open("sample.glb"))]})
```

ヘッドフォンの点群のグラウンドトゥルースと予測

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-M5hpoiCSnWMLlXYvPxY%2F-M5hrF-saOkhhpHeooUd%2Fground%20truth%20-%20prediction%20of%203d%20point%20clouds.png?alt=media&token=3d1eb19c-a4eb-4479-bbe2-a9393c15bfb0)

​[実例を見る→](https://app.wandb.ai/nbaryd/SparseConvNet-examples_3d_segmentation/reports/Point-Clouds--Vmlldzo4ODcyMA)​​

## **インクリメンタルログ** <a id="incremental-logging"></a>

さまざまなx軸に対してメトリックをプロットする場合は、`wandb.log({'loss': 0.1, 'epoch': 1, 'batch': 3})`のように、ステップをメトリックとしてログに記録できます。UIでは、チャート設定でx軸間の切り替えができます。

コード内のさまざまな場所から単一の履歴ステップにログを記録する場合は、次のようにステップインデックスを`wandb.log（）`に渡すことができます。

```text
wandb.log({'loss': 0.2}, step=step)
```

`step`に同じ値を渡し続ける限り、W＆Bは各呼び出しからキーと値を1つの統合ディクショナリに収集します。前のステップとは異なる`step`の値を使用して`wandb.log（）`を呼び出すとすぐに、W＆Bは収集されたすべてのキーと値を履歴に書き込み、そしてまた収集を最初からやり直します。これは、`step:0,1, 2,...`の連続した値でのみ使用する必要があるという意味ですので、ご注意ください。この機能では、完全な履歴ステップを書き込むことはできず、「現在」と「次」のみ書き込むことができます。

wandb.logで**commit=False**を設定してメトリックを累積することもできます。メトリックを持続化するには、**commit**フラグを使わず、必ずwandb.logを呼び出してください。

```text
wandb.log({'loss': 0.2}, commit=False)# Somewhere else when I'm ready to report this step:wandb.log({'accuracy': 0.8})
```

## **サマリー・メトリック** <a id="summary-metrics"></a>

要約統計量は、モデルごとに単一のメトリックをトラッキングするために使用されます。サマリー・メトリックが変更されると、更新された状態のみが保存されます。手動で変更しない限り、要約は最後に追加された履歴行に自動的に設定されます。サマリー・メトリックを変更すると、最後に設定された値のみが保持されます。

```text
wandb.init(config=args)​best_accuracy = 0for epoch in range(1, args.epochs + 1):  test_loss, test_accuracy = test()  if (test_accuracy > best_accuracy):    wandb.run.summary["best_accuracy"] = test_accuracy    best_accuracy = test_accuracy
```

トレーニング完了後、評価メトリックを実行サマリーに保存すると良いでしょう。サマリーは、numpy配列、pytorchテンソル、またはtensorflowテンソルを処理できます。値がこれらのタイプの1つである場合、テンソル全体をバイナリファイルに保持し、最小、平均、分散、95％パーセンタイルなどの高レベルのメトリックをサマリーオブジェクトに格納します。

```text
api = wandb.Api()run = api.run("username/project/run_id")run.summary["tensor"] = np.random.random(1000)run.summary.update()
```

### **ログに直接アクセスします** <a id="accessing-logs-directly"></a>

履歴オブジェクトは、「wandb.log」によってログに記録されたメトリックを追跡するために使用されます。`run.history.row`を介して、メトリックの変更可能なディクショナリにアクセスできます。`run.history.add`または`wandb.log`が呼び出されると、行が保存され、新しい行が作成されます。

####  **Tensorflowの例** <a id="tensorflow-example"></a>

```text
wandb.init(config=flags.FLAGS)​# Start tensorflow trainingwith tf.Session() as sess:  sess.run(init)​  for step in range(1, run.config.num_steps+1):      batch_x, batch_y = mnist.train.next_batch(run.config.batch_size)      # Run optimization op (backprop)      sess.run(train_op, feed_dict={X: batch_x, Y: batch_y})      # Calculate batch loss and accuracy      loss, acc = sess.run([loss_op, accuracy], feed_dict={X: batch_x, Y: batch_y})​      wandb.log({'acc': acc, 'loss':loss}) # log accuracy and loss
```

####  **PyTorchの例** <a id="pytorch-example"></a>

```text
# Start pytorch trainingwandb.init(config=args)​for epoch in range(1, args.epochs + 1):  train_loss = train(epoch)  test_loss, test_accuracy = test()​  torch.save(model.state_dict(), 'model')​  wandb.log({"loss": train_loss, "val_loss": test_loss})
```

##  **よくある質問** <a id="common-questions"></a>

###  **異なるエポックの画像を比較します** <a id="compare-images-from-different-epochs"></a>

ステップから画像をログに記録するたびに、UIに表示するために画像が保存されます。画像パネルを固定し、**ステップスライダー**を使用して、さまざまなステップの画像を表示します。これにより、トレーニング中にモデルの出力がどのように変化するかを簡単に比較できます。

```text
wandb.log({'epoch': epoch, 'val_acc': 0.94})
```

### **バッチのロギング** <a id="batch-logging"></a>

**すべてのバッチで特定のメトリックをログに記録し、プロットを標準化する場合は、メトリックでプロットしたいx軸の値をログに記録できます。次に、カスタムプロットで\[編集\]をクリックして、カスタムx軸を選択します。**

```text
wandb.log({'batch': 1, 'loss': 0.3})
```

###  **PNGをログする** <a id="log-a-png"></a>

wandb.Imageは、デフォルトでPILImageのnumpy配列またはインスタンスをPNGに変換します。

```text
wandb.log({"example": wandb.Image(...)})# Or multiple imageswandb.log({"example": [wandb.Image(...) for img in images]})
```

### **JPEGをログする** <a id="log-a-jpeg"></a>

 JPEGを保存するには、ファイルへのパスを渡すことができます。

```text
im = PIL.fromarray(...)rgb_im = im.convert('RGB')rgb_im.save('myimage.jpg')wandb.log({"example": wandb.Image("myimage.jpg")})
```

### **ビデオをログ** <a id="log-a-video"></a>

```text
wandb.log({"example": wandb.Video("myvideo.mp4")})
```

これで、メディアブラウザでビデオを表示できます。プロジェクトワークスペースに移動、またはワークスペースの実行、またはレポートで\[視覚化の追加\]をクリックしてリッチメディアパネルを追加します。

###  **カスタムx軸** <a id="custom-x-axis"></a>

デフォルトでは、wandb.logを呼び出すたびにグローバルステップがインクリメントされます。必要に応じて、単調増加する独自のステップをログに記録し、それをグラフのカスタムx軸として選択できます。

たとえば、調整したいトレーニングと検証のステップがある場合は、独自のステップカウンター`wandb.log({“acc”:1, “global_step”:1})`を渡します。次に、グラフでx軸として「global\_step」を選択します。`wandb.log({“acc”:1,”batch”:10}, step=epoch)`を使用すると、デフォルトのステップ軸に加えて、x軸として「バッチ」を選択できます。

###  **点群のナビゲートとズームイン** <a id="navigating-and-zooming-in-point-clouds"></a>

コントロール長押し+マウスでスペース内を移動できます

### **グラフに何も表示されない場合** <a id="duplicate-metric-names"></a>

「視覚化データがまだログに記録されていません」と表示されている場合は、スクリプトから最初のwandb.log呼び出しがまだ取得されていないことを意味します。この場合、ステップの完了に時間がかかることが原因である可能性があります。各エポックの終わりにログを記録している場合は、エポックごとに数回ログを記録して、データストリームをより迅速に確認できます。

###  **パフォーマンスおよび制限** <a id="performance-and-limits"></a>

**サンプリング**

送信するポイントが多いほど、UIのグラフ読み込みに時間がかかります。1行に1000ポイントを超える場合は、ブラウザにデータが送信される前に、バックエンドで1000ポイントまでサンプリングが行われます。このサンプリングは非決定的であるため、ページを更新すると、別のサンプリングされたポイントセットが表示されます。

すべてのオリジナルデータが必要な場合は、[データAPI](https://docs.wandb.com/library/api)を使用してサンプリングされていないデータをプルダウンできます。

**ガイドライン**

メトリックごとにログに記録するのは10,000ポイント未満をお勧めします。500列を超える構成および要約メトリックがあっても、テーブルには500のみが表示されます。1行に100万ポイントを超えるログを記録すると、ページの読み込みに時間がかかります。

メトリックは大文字と小文字を区別せずに保存されるため、「My-Metric」と「my-metric」のように同じ名前の2つのメトリックがないことを確認してください。

### **画像のアップロードをコントロール** <a id="control-image-uploading"></a>

「W＆Bをプロジェクトに統合したいのですが、画像はアップロードしたくありません」

当社の統合では、画像は自動的にアップロードされません。明示的にアップロードするファイルを指定します。これは、画像を明示的にログに記録するPyTorch用に作成した簡単な例です。[http://bit.ly/pytorch-mnist-colab](http://bit.ly/pytorch-mnist-colab)​​

```text
wandb.log({        "Examples": example_images,        "Test Accuracy": 100. * correct / len(test_loader.dataset),        "Test Loss": test_loss})
```

[  
](https://docs.wandb.ai/library/config)

