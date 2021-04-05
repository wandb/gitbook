# Data Import/Export API

カスタム分析用にデータフレームをエクスポートするか、完了した実行に非同期でデータを追加します。詳細については、[API](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MN_4xmW6jcYndpU_n9G/v/japanese/ref/export-api/api)リファレンスを参照してください。

###  認証

次の2つの方法のいずれかで、APIキーを使用してあなたの機器を認証します。

1. コマンドラインでwandb loginを実行し、[APIキー](https://wandb.ai/authorize)を貼り付けます。
2. **WANDB\_API\_KEY**環境変数をAPIキーに設定します。

### 実行データのエクスポート

終了した実行またはアクティブな実行からデータをダウンロードします。一般的な使用法には、Jupyterノートブックでのカスタム分析用のデータフレームのダウンロード、または自動化環境でのカスタムロジックの使用が含まれます。

```python
import wandb
api = wandb.Api()
run = api.run("<entity>/<project>/<run_id>")
```

実行オブジェクトの最も一般的に使用される属性は次のとおりです。

| 属性 | 意味 |
| :--- | :--- |
| run.config | ハイパーパラメータなどのモデル入力の辞書 |
| run.history\(\) | モデルがロスなどのトレーニング中に変化する値を格納することを目的とした辞書のリスト。コマンドwandb.log\(\)がこのオブジェクトに追加されます |
| run.summary | 出力の辞書。これは、精度やロスなどのスカラー、または大きなファイルの場合があります。デフォルトでは、wandb.log\(\)は、要約をログに記録された時系列の最終値に設定します。これは直接設定することもできます。 |

 過去の実行のデータを変更または更新することもできます。デフォルトでは、APIオブジェクトの単一インスタンスがすべてのネットワークリクエストをキャッシュします。ユースケースで実行中のスクリプトにリアルタイムの情報が必要な場合は、api.flush\(\)を呼び出して更新された値を取得します。

###  サンプリング

デフォルトの履歴メソッドは、メトリックを固定数のサンプルにサンプリングします（デフォルトは500です。これは、samples主張で変更できます）。大規模な実行ですべてのデータをエクスポートする場合は、run.scan\_history（）メソッドを使用できます。詳細については、[API](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MN_4xmW6jcYndpU_n9G/v/japanese/ref/export-api/api)リファレンスを参照してください。

### 複数の実行に関するクエリ

{% tabs %}
{% tab title="MongoDBスタイル" %}
W＆B APIは、api.runs（）を使用してプロジェクト内の実行間でクエリを実行する方法も提供します。最も一般的な使用例は、カスタム分析のために実行データをエクスポートすることです。クエリインターフェイスは、[MongoDBの使用](https://docs.mongodb.com/manual/reference/operator/query/)と同じです。

```python
runs = api.runs("username/project", {"$or": [{"config.experiment_name": "foo"}, {"config.experiment_name": "bar"}]})
print("Found %i" % len(runs))
```
{% endtab %}

{% tab title="Dataframes and CSVs" %}
このサンプルスクリプトは、プロジェクトを検索し、名前、構成、および要約統計量を含む実行のCSVを出力します。

```python
import wandb
api = wandb.Api()

# Change oreilly-class/cifar to <entity/project-name>
runs = api.runs("<entity>/<project>")
summary_list = [] 
config_list = [] 
name_list = [] 
for run in runs: 
    # run.summary are the output key/values like accuracy.  We call ._json_dict to omit large files 
    summary_list.append(run.summary._json_dict) 

    # run.config is the input metrics.  We remove special values that start with _.
    config_list.append({k:v for k,v in run.config.items() if not k.startswith('_')}) 

    # run.name is the name of the run.
    name_list.append(run.name)       

import pandas as pd 
summary_df = pd.DataFrame.from_records(summary_list) 
config_df = pd.DataFrame.from_records(config_list) 
name_df = pd.DataFrame({'name': name_list}) 
all_df = pd.concat([name_df, config_df,summary_df], axis=1)

all_df.to_csv("project.csv")
```
{% endtab %}
{% endtabs %}

`api.runs（...）`を呼び出すと、反復可能でリストのように機能する**Runs**オブジェクトが返されます。オブジェクトは、必要に応じて一度に50回の実行を順番にロードします。ページごとにロードされる数は、**per\_page**キーワード主張を使用して変更できます。

`api.runs（...）`は、**order**キーワード主張も受け入れます。デフォルトの順序は`-created_at`です。結果を昇順で取得するには、`+created_at`を指定します。構成値または要約値、つまり`summary.val_acc`または`config.experiment_name`で並べ替えることもできます。

### エラー処理

W＆Bサーバーとの通信中にエラーが発生すると、wandb.CommErrorが発生します。元の例外は、**exc**属性を介してイントロスペクトできます。

### を取得します

UIで、実行をクリックしてから、実行ページの\[概要\]タブをクリックして、最新のgitcommitを確認します。これは、ファイル`wandb-metadata.json`にもあります。パブリックAPIを使用すると、**run.commit**でgitハッシュを取得できます。

##  よくある質問

### Export data to visualize in matplotlib or seaborn

Check out our [API examples](examples.md) for some common export patterns. You can also click the download button on a custom plot or on the expanded runs table to download a CSV from your browser. データをエクスポートしてmatplotlibまたはseabornで視覚化します

一般的なエクスポートパターンについては、APIの例をご覧ください。カスタムプロットまたは展開された実行テーブルのダウンロードボタンをクリックして、ブラウザからCSVをダウンロードすることもできます。

### Get the random run ID and run name from your script

スクリプトからランダムな実行IDと実行名を取得します

* **wandb.init（）を呼び出した後、次のようにスクリプトからランダムな実行IDまたは人間が読める形式の実行名にアクセスできます。**
* **•ユニークな実行ID（8文字のハッシュ）：wandb.run.id**
* **ランダムな実行名（人間が読める形式）：wandb.run.name**

\*\*\*\*

* **実行ID**：生成されたハッシュのままにします。これは、プロジェクトの実行全体でユニークである必要があります。
*  **実行名**：グラフの異なる線の違いがわかるように、短く、読みやすく、できればユニークである必要があります。
*  **ランニングノート**：これは、ランニングで何をしているかを簡単に説明するのに最適な場所です。これはwandb.init（notes = "your notes here"）で設定できます
* **実行タグ**：実行タグで動的に追跡し、UIのフィルターを使用して、関心のある実行だけに表をフィルターします。スクリプトからタグを設定し、UIで実行表と実行ページの概要タブの両方でタグを編集できます。

