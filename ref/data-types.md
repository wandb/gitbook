---
description: wandb.data_types
---

# Data Types Reference

 [ソース](https://github.com/wandb/client/blob/master/wandb/data_types.py#L0)

Wandbには、豊富な視覚化をログに記録するための特別なデータ型があります。

特別なデータ型はすべてWBValueのサブクラスです。すべてのデータ型はJSONにシリアル化されます。これは、wandbがオブジェクトをローカルに保存してW＆Bサーバーにアップロードするために使用するためです。

## WBValue

 [ソース](https://github.com/wandb/client/blob/master/wandb/data_types.py#L43)

```python
WBValue(self)
```

wandb.log（）によってログに記録され、wandbによって視覚化できるものの抽象親クラス。

オブジェクトはJSONとしてシリアル化され、他のフィールドの解釈方法を示す\_type属性が常にあります。

 **リターンズ：**

後で文字列にシリアル化できる、このオブジェクトのJSON対応の`dict`表現。

###  **ヒストグラム**

 [ソース](https://github.com/wandb/client/blob/master/wandb/data_types.py#L64)

```python
Histogram(self, sequence=None, np_histogram=None, num_bins=64)
```

ヒストグラムのwandbクラス

このオブジェクトは、numpyのヒストグラム関数[https://docs.scipy.org/doc/numpy/reference/generated/numpy.histogram.html](https://docs.scipy.org/doc/numpy/reference/generated/numpy.histogram.html)と同じように機能します

**例：**

 シーケンスからヒストグラムを生成します

```python
wandb.Histogram([1,2,3])
```

np.histogramから効率的に初期化します。

```python
hist = np.histogram(data)
wandb.Histogram(np_histogram=hist)
```

**主張：**

* **sequence array\_like‐ヒストグラムの入力データ**
* **np\_histogram numpy histogram‐事前に計算されたヒストグラムの代替入力**
* **num\_bins int‐ヒストグラムのビンの数。 ビンのデフォルト数は64です。ビンの最大数は512です。**

### **属性：**

* bins \[float\] ‐ビンの端
* histogram \[int\]‐各ビンに含まれる要素の数

## **メディア**

 [ソース](https://github.com/wandb/client/blob/master/wandb/data_types.py#L122)

```python
Media(self, caption=None)
```

JSONの外部にファイルとして保存し、フロントエンドのメディアパネルに表示するWBValue。

必要に応じて、ファイルをRunのメディアディレクトリに移動またはコピーして、アップロードします。

## BatchableMedia

   [ソース](https://github.com/wandb/client/blob/master/wandb/data_types.py#L232)

```python
BatchableMedia(self, caption=None)
```

メディアの親クラスは、画像やサムネイルなど、バッチで特別に扱います。

画像とは別に、これらのバッチを使用して、メディアディレクトリ内のファイルを名前で整理します。

##  **表**

 [ソース](https://github.com/wandb/client/blob/master/wandb/data_types.py#L244)

```python
Table(self,
      columns=['Input', 'Output', 'Expected'],
      data=None,
      rows=None,
      dataframe=None)
```

これは、レコードの小さなセットを表示するように設計されたテーブルです。

**主張：**

* columns \[str\]‐テーブル内の列の名前。デフォルトは\["Input", "Output", "Expected"\]です。
* data array‐文字列として表示される値の2D配列。
* dataframe pandas.DataFrame‐テーブルの作成に使用されるDataFrameオブジェクト。設定すると、他の主張は無視されます。

##  **オーディオ**

[ソース ](https://github.com/wandb/client/blob/master/wandb/data_types.py#L305)

```python
Audio(self, data_or_path, sample_rate=None, caption=None)
```

オーディオクリップのWandbクラス。

**主張：**

* `data_or_path` _string or numpy array_ -オーディオファイルまたはオーディオデータのnumpy配列へのパス。
* `sample_rate` _int_ - int‐サンプルレート。オーディオデータの生のnumpy配列を渡すときに必要です。
* `caption` _string_ - 音声で表示するキャプション。

## Object3D

 [ソース](https://github.com/wandb/client/blob/master/wandb/data_types.py#L404)

```python
Object3D(self, data_or_path, **kwargs)
```

3D点群のWandbクラス。

 **主張：**

data\_or\_path \(numpy array \| string \| io \)：Object3Dは、ファイルまたはnumpy配列から初期化できます。

サポートされているファイルタイプは、obj、gltf、babylon、stlです。ファイルまたはioオブジェクトへのパスと、`'obj', 'gltf', 'babylon', 'stl'`のいずれかである必要があるfile\_typeを渡すことができます。

numpy配列の形状は、次のいずれかである必要があります。

```python
[[x y z],       ...] nx3
[x y z c],     ...] nx4 where c is a category with supported range [1, 14]
[x y z r g b], ...] nx4 where is rgb is color
```

## Molecule

 [ソース](https://github.com/wandb/client/blob/master/wandb/data_types.py#L527)

```python
Molecule(self, data_or_path, **kwargs)
```

分子データのWandbクラス

**主張：**

data\_or\_path \( string \| io \)：分子はファイル名またはioオブジェクトから初期化できます。

## Html

 [ソース](https://github.com/wandb/client/blob/master/wandb/data_types.py#L611)

```python
Html(self, data, inject=True)
```

 任意のhtmlのWandbクラス

**主張：**

* `data string` or io object‐wandbに表示するHTML
* `inject boolean‐HTML`オブジェクトにスタイルシートを追加します。Falseに設定すると、HTMLは変更されずに通過します。

##  **ビデオ**

[source](https://github.com/wandb/client/blob/master/wandb/data_types.py#L680)

```python
Video(self, data_or_path, caption=None, fps=4, format=None)
```

**主張：**

data\_or\_path \(numpy array \| string \| io\)：ビデオは、ファイルまたはioオブジェクトへのパスで初期化できます。形式は「gif」、「mp4」、「webm」、または「ogg」である必要があります。formatは、format主張で指定する必要があります。ビデオは、numpyテンソルで初期化できます。 ゴツゴツしたテンソルは4次元または5次元でなければなりません。チャネルは（時間、チャネル、高さ、幅）または（バッチ、時間、チャネル、高さ幅）である必要があります

* caption string‐表示するビデオに関連付けられたキャプション
* fps int‐ビデオの1秒あたりのフレーム数。デフォルトは4です。
* format string‐ビデオのフォーマット。パスまたはioオブジェクトで初期化する場合に必要です

### **画像**。

[ソース](https://github.com/wandb/client/blob/master/wandb/data_types.py#L827)

```python
Image(self,
      data_or_path,
      mode=None,
      caption=None,
      grouping=None,
      boxes=None,
      masks=None)
```

. 画像のWandbクラス。

**主張：**

* data\_or\_path numpy array \| string \| io‐画像データのnumpy配列またはPIL画像を受け入れます。クラスはデータ形式を推測して変換しようとします。
* mode string‐画像のPILモード。最も一般的なのは「L」、「RGB」、「RGBA」です。[https://pillow.readthedocs.io/en/4.2.x/handbook/concepts.html\#concept-modes](https://pillow.readthedocs.io/en/4.2.x/handbook/concepts.html#concept-modes)で完全な説明。
* caption string‐画像を表示するためのラベル。 

## JSONMetadata

 [ソース](https://github.com/wandb/client/blob/master/wandb/data_types.py#L1093)

```python
JSONMetadata(self, val, **kwargs)
```

JSONMetadataは、任意のメタデータをファイルとしてエンコードするためのタイプです。

## BoundingBoxes2D

 [ソース ](https://github.com/wandb/client/blob/master/wandb/data_types.py#L1126)

```python
BoundingBoxes2D(self, val, key, **kwargs)
```

 2DバウンディングボックスのWandbクラス

## ImageMask

 [ソース](https://github.com/wandb/client/blob/master/wandb/data_types.py#L1204) 

```python
ImageMask(self, val, key, **kwargs)
```

セグメンテーションタスクに役立つ画像マスクのWandbクラス

## Plotly

[ソース ](https://github.com/wandb/client/blob/master/wandb/data_types.py#L1274)

```python
Plotly(self, val, **kwargs)
```

プロットプロット用のWandbクラス。

**主張：**

* val-matplotlibまたはplotly figure

##  **グラフ**

 [ソース](https://github.com/wandb/client/blob/master/wandb/data_types.py#L1314)

```python
Graph(self, format='keras')
```

**例：**

kerasモデルをインポートします。

```python
Graph.from_keras(keras_model)
```

* format string‐wandbがグラフを適切に表示するのに役立つフォーマット。
* nodes \[wandb.Node\] ‐wandb.Nodesのリスト
* nodes\_by\_id dict‐idのdict-&gt;ノードエッジ\(\[\(wandb.Node, wandb.Node\)\]\)：エッジとして解釈されるノードのペアのリスト
* loaded boolean‐グラフが完全にロードされているかどうかを示すフラグ
* root wandb.Node‐グラフのルートノード

##  **ノード**

[ソース](https://github.com/wandb/client/blob/master/wandb/data_types.py#L1470)

```python
Node(self,
     id=None,
     name=None,
     class_name=None,
     size=None,
     parameters=None,
     output_shape=None,
     is_output=None,
     num_parameters=None,
     node=None)
```

[Graph](https://docs.wandb.com/ref/data-types#graph)で使用されるノード

##  **エッジ**

[ソース](https://github.com/wandb/client/blob/master/wandb/data_types.py#L1636)

```python
Edge(self, from_node, to_node)
```

[Graph](https://docs.wandb.com/ref/data-types#graph)で使用されるエッジ

