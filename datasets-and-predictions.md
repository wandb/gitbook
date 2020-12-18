---
description: データセットを反復し、モデルの予測を理解します。
---

# Datasets & Predictions \[Early Access\]

 __この機能は現在、早期アクセス段階にあります。一定の制限はありますが、wandb.aiの本番サービスで使用できます。APIは変更を前提としています。質問、コメント、アイデアをぜひお聞かせください。[feedback@wandb.com](mailto:feedback@wandb.com)までご連絡ください

データはすべてのMLワークフローの中核にあります。私たちは、W＆B Artifactsに強力な新機能を追加して、データセットとモデル評価をサンプルレベルで視覚化しクエリできるようにしました。この新しいツールを使用して、あなたのデータセットを分析および理解し、モデルのパフォーマンスを測定およびデバッグできます。

勇気を出して、エンドツーエンドのデモ版を試しましょう。 [![](https://colab.research.google.com/assets/colab-badge.svg)](http://wandb.me/dsviz-demo-colab)

![&#x3053;&#x308C;&#x304C;&#x30C7;&#x30FC;&#x30BF;&#x30BB;&#x30C3;&#x30C8;&#x304A;&#x3088;&#x3073;&#x4E88;&#x6E2C;&#x30C0;&#x30C3;&#x30B7;&#x30E5;&#x30DC;&#x30FC;&#x30C9;&#x306E;&#x30D7;&#x30EC;&#x30D3;&#x30E5;&#x30FC;&#x3067;&#x3059;](https://paper-attachments.dropbox.com/s_21D0DE4B22EAFE9CB1C9010CBEF8839898F3CCD92B5C6F38DBE168C2DB868730_1605673880422_image.png)

## **使い方**

私たちの目標は、一般タスクで利用できる革新的で豊富な視覚化を備えた、拡張性が高く、柔軟性があり、構成可能なツールを提供することです。システムは以下のもので構築されています：

* W＆B Artifacts内に、リッチメディア（バウンディングボックスの画像など）を選択的に含む大きなwandb.Tableオブジェクトを保存する機能。
* クロスアーティファクトファイル参照のサポート、およびUIで表を結合する機能。これは、たとえば、ソース画像とラベルを複製することなく、グラウンドトゥルースデータセットアーティファクトに対する一連のバウンディングボックス予測をログに記録するために使用されます。
* \[将来\]W＆B Artifactsに貯蔵されている表に対する大規模なクエリのバックエンドAPIサポート。
* 斬新な「タイプの、実行時に交換可能なUIパネルアーキテクチャ」。これが、データ表を比較およびグループ化するときに表示される豊富な視覚化とグラフを強化するものです。したがって私たちがこれを開始すると、ユーザーはW＆B UIのどこでも機能する完全にカスタムのビジュアライザーを追加できるようになります。

## UI

 __続いて、[demo colab](http://wandb.me/dsviz-demo-colab)から生成されたこの[サンプルプロジェクト](https://wandb.ai/shawn/dsviz_demo/artifacts/dataset/train_results/18bab424be78561de9cd/files)を開きます。

ログに記録された表とメディアオブジェクトを視覚化するには、アーティファクトを開き、\[**ファイル**\]タブに移動して、表またはオブジェクトをクリックします。**グラフビュー**に切り替えて、プロジェクト内のアーティファクトと試行がどのように接続されているかを確認します。**Explode関数**を切り替えて、パイプラインの各ステップの個々の実行を確認します。  


### **表の視覚化**

 \[**ファイル**\]タブを開いて、メインの視覚化UIを表示します。[サンプルプロジェクト](https://wandb.ai/shawn/dsviz_demo/artifacts/dataset/train_results/18bab424be78561de9cd/files)では、「train\_iou\_score\_table.table.json」をクリックして視覚化します。データを探索するために、表をフィルタリング、グループ化、および並べ替えします。

![](.gitbook/assets/image%20%2899%29.png)

### **フィルタリング**

フィルタは[mongodbの集計言語](https://docs.mongodb.com/manual/meta/aggregation-quick-reference/)で指定されており、ネストされたオブジェクトへのクエリを適切にサポートしています\[ただし、バックエンドでは実際にはmongodbを使用していません！\]。次に2つの例を示します。

**「道路」列で0.05以上の例を検索します**

`{$gt: ['$0.road', 0.05]}` 

**1つ以上の「車」のバウンディングボックスの予測がある例を検索します**

`{  
  $gte: [   
    {$size:   
      {$filter:   
        {input: '$0.pred_mask.boxes.ground_truth',  
          as: 'box',  
          cond: {$eq: ['$$box.class_id', 13]}  
        }  
      }  
    },  
  1]  
}`

### **グループ化**

「dominant\_pred」でグループ化してみてください。表がグループ化されると、数値列が自動的にヒストグラムになることがわかります。

![](https://paper-attachments.dropbox.com/s_21D0DE4B22EAFE9CB1C9010CBEF8839898F3CCD92B5C6F38DBE168C2DB868730_1605673736462_image.png)

\*\*\*\*

###  **並べ替え**

 \[**並べ替え**\]タブをクリックし、表内の任意の列を選択して並べ替えます。

###  **比較**

 表内の任意の2つのアーティファクトバージョンを比較します。そして、サイドバーの「v3」にカーソルを合わせ、「比較」ボタンをクリックします。これにより、両方のバージョンからの予測が1つの表に表示されます。両方の表が互いに重なり合っていると考えてください。表は、入力される数値列の棒グラフをレンダリングすることを決定し、表ごとに1つの棒が比較されます。

上部の「クイックフィルター」を使用して、両方のバージョンにのみ存在する例にあなたの結果を制限できます。

![](https://paper-attachments.dropbox.com/s_21D0DE4B22EAFE9CB1C9010CBEF8839898F3CCD92B5C6F38DBE168C2DB868730_1605673764298_image.png)

  
 比較とグループ化を同時に行ってみてください。入力表ごとに1つの色を使用する「マルチヒストグラム」が表示されます。  


![](https://paper-attachments.dropbox.com/s_21D0DE4B22EAFE9CB1C9010CBEF8839898F3CCD92B5C6F38DBE168C2DB868730_1605673664913_image.png)

## Python API

 __エンドツーエンドの例については、私たちの[demo colab](http://wandb.me/dsviz-demo-colab)をお試しください。

 データセットと予測を視覚化するには、リッチメディアをアーティファクトに記録します。W＆B Artifactsにrawファイルを保存することにより、wandb APIによって提供される他のリッチメディアタイプの保存、取得、および視覚化ができます。

現在、次のタイプがサポートされています。

* wandb.Table\(\)
* wandb.Image\(\)

追加のメディアタイプに対するサポートは間もなく開始されます。

###  **新しいアーティファクトメソッド**

アーティファクトオブジェクトには2つの新しいメソッドがあります。

`artifact.add(object, name)`

* メディアオブジェクトをアーティファクトに追加します。現在サポートされているタイプはwandb.Tableとwandb.Imageで、今後さらに追加される予定です。
* これにより、幼児のメディアオブジェクトとアセット（生の「.png」ファイルなど）がアーティファクトに再帰的に追加されます。

`artifact.get(name)`

* 保存されたアーティファクトから再構築されたメディアオブジェクトを返します。

 これらの方法は対称的です。.add（）を使用してオブジェクトをアーティファクトに保存し、必要なマシンで.get（）を使用してまったく同じオブジェクトを取得できるようにします。

### **wandb.\* media objects**

`wandb.Table`

表は、データセットと予測の視覚化の中核です。データセットを視覚化するには、データセットをwandb.Tableに配置し、必要に応じてwandb.Imageオブジェクト、配列、辞書、文字列、数値を追加してから、アーティファクトに表を追加します。現在、各表は50,000行に制限されています。アーティファクトには、必要な数の表をログに記録できます。

 次のサンプルコードは、Keras cifar10テストデータセットから1000個の画像とラベルをアーティファクト内のwandb.Tableとして保存します。

```python
import tensorflow as tf
import wandb

classes = ['airplane', 'automobile', 'bird', 'cat',
           'deer', 'dog', 'frog', 'horse', 'ship', 'truck']
_, (x_test, y_test) = tf.keras.datasets.cifar10.load_data()

wandb.init(job_type='create-dataset') # start tracking program execution

# construct a table containing our dataset
table = wandb.Table(('image', 'label'))
for x, y in zip(x_test[:1000], y_test[:1000]):
    table.add_data(wandb.Image(x), classes[y[0]])

# put the table in an artifact and save it
dataset_artifact = wandb.Artifact('my-dataset', type='dataset')
dataset_artifact.add(table, 'dataset')
wandb.log_artifact(dataset_artifact)
```

 このコードを実行すると、W＆B UIで表を視覚化できるようになります。アーティファクトの\[ファイル\]タブで\[dataset.table.json\]をクリックします。「ラベル」でグループ化して、「画像」列の各クラスの例を取得してみてください。

  
`wandb.Image`

 ****[wandb.logドキュメント](https://docs.wandb.com/library/log#images-and-overlays)で説明されているように、wandb.Imageオブジェクトを作成できます。

 上記のドキュメントで指定されているように、wandb.Imageを使用すると、セグメンテーションマスクとバウンディングボックスを画像に添付できます。wandb.Image（）オブジェクトをアーティファクトに保存する場合、1つの変更があります。以前は各wandb.Imageに保存すべきであった「class\_labels」を取り除きました。

次のように、バウンディングボックスまたはセグメンテーションマスクを使用する場合は、クラスラベルを個別に作成する必要があります。

```python
class_set = wandb.Classes(...)
example = wandb.Image(<path_to_image_file>, classes=class_set, masks={
            "ground_truth": {"path": <path_to_mask>}})
```

別のアーティファクトに記録されたwandb.Imageを参照するwandb.Imageを作成することもできます。これにより、クロスアーティファクトファイル参照を使用して、基礎となる画像の重複を回避できます。

```python
artifact = wandb.use_artifact('my-dataset-1:v1')
dataset_image = artifact.get('an-image')  # if you've logged a wandb.Image here 
predicted_image = wandb.Image(dataset_image, classes=class_set, masks={
            "predictions": {"path": <path_to_mask>}})
```

  
`wandb.Classes`

クラスID（数値）からラベル（文字列）へのマッピングを定義するために使用されます。

```python
CLASSES = ['dog', 'cat']
class_set = wandb.Classes([{'name': c, 'id': i} for i, c in enumerate(CLASSES)])
```



`wandb.JoinedTable`

2つの表の結合をレンダリングするためのW＆B UIの指示に使用されます。表は他のアーティファクトに保存される場合があります。

```python
jt = wandb.JoinedTable(table1, table2, 'id')
artifact.add(jt, 'joined')
```

##  **エンドツーエンドの例**

 以下をカバーするエンドツーエンドの例については、[colabノートブック](http://wandb.me/dsviz-demo-colab)をお試しください。

*  データセットの構築と視覚化
* モデルトレーニング
* データセットに対する予測のロギング、およびその視覚化

## **よくある質問**

**トレーニング用にデータセットをバイナリ形式にパックします。それはW＆Bデータセットアーティファクトとどのような関連がありますか？**

 ここに取ることができるいくつかのアプローチがあります：

1. データセットの記録システムとしてwandb.Table形式を使用します。ここであなたは次の2つのいずれかを実行できます。
   1. トレーニング時に、W＆Bフォーマットアーティファクトからパックフォーマットを導出します。
   2. または、パック形式のアーティファクトを生成するパイプラインステップを用意し、そのアーティファクトからトレーニングします
2. パックされたフォーマットとwandb.Tableを同じアーティファクトに保存します
3. パックされた形式を指定して、wandb.Tableアーティファクトをログに記録するジョブを作成します

   \[注：アルファフェーズでは、W＆Bアーティファクトに保存される表に50kの行制限があります\]　モデル予測をクエリして視覚化する場合は、トレーニングステップでサンプルIDをパスする方法を検討する必要があります。これにより、予測表がソースデータセット表に再び結合されます。いくつかのアプローチについては、リンクされた例を参照してください。私たちは今後、一般的な形式のコンバーター、さらに多くの例、一般的なフレームワークとの緊密な統合を提供していくつもりです。

## **現在の制限**

   __この機能は現在早期アクセス段階にあり、wandb.aiの本番サービスで使用できますが、いくつかの制限があります。APIは変更される可能性があります。ご質問、コメント、アイデアがございましたら、お問い合わせください！[feedback@wandb.com](mailto:feedback@wandb.com)までご連絡ください。

* スケール：アーティファクトに記録される表は現在、50,000行に制限されています。将来的には表ごと1億行以上の処理を目標とし、リリースごとに徐々に引き上げていきます。
* 現在サポートされているwandb.\*メディアタイプ：
  * wandb.Table
  * wandb.Image
* W＆B UIでクエリとビューを保存および永続化できません
* W＆Bレポートにビジュアライゼーションを追加できません

##  **今後の課題**

* 多数のUXおよびUIの継続的な改善
* 表ごとの行制限の増加
* 表ストレージ向けの列型バイナリ形式（Parquet形式）の使用
* wandb.Tableと、データフレームやその他の一般的なPython表形式の処理
* より強力なクエリシステムの追加、およびより詳細な集計と分析の提供
* より多くのメディアタイプの提供
* ビュー/ワークスペースの保存を介してUI状態を保持する機能の追加
* 同僚と共有するために、視覚化と分析をW＆Bレポートに保存する機能
* ラベルの付け直しやその他のワークフローのためにクエリとサブセットを保存する機能
* Pythonからクエリを実行する機能
* 大きな表データを使用する他のビジュアライゼーションを追加する機能（クラスタービジュアライザーなど）
* 完全なるカスタムの視覚化のための、ユーザーが作成したパネルの提供

