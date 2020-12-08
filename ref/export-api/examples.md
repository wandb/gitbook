---
description: Python APIを使用してW＆Bからデータを取得するための一般的な使用例を次に示します。
---

# Data Export API Examples

### 実行パスを見つけます

パブリックAPIを使用するには、多くの場合、`"<entity>/<project>/<run_id>"`の**実行パス**が必要になります。アプリで実行を開き、\[**概要**\]タブをクリックして、実行の実行パスを確認します。

### 実行からメトリックを読み取ります

この例では、`<entity>/<project>/<run_id>`に保存された実行について、`wandb.log({"accuracy": acc})`で保存されたタイムスタンプと精度を出力します。

```python
import wandb
api = wandb.Api()

run = api.run("<entity>/<project>/<run_id>")
if run.state == "finished":
   for i, row in run.history().iterrows():
      print(row["_timestamp"], row["accuracy"])
```

### 2つの実行を比較します

これにより、run1とrun2で異なる構成パラメータが出力されます。出力：

```python
import wandb
api = wandb.Api()

# replace with your <entity_name>/<project_name>/<run_id>
run1 = api.run("<entity>/<project>/<run_id>")
run2 = api.run("<entity>/<project>/<run_id>")

import pandas as pd
df = pd.DataFrame([run1.config, run2.config]).transpose()

df.columns = [run1.name, run2.name]
print(df[df[run1.name] != df[run2.name]])
```

Outputs:

```text
              c_10_sgd_0.025_0.01_long_switch base_adam_4_conv_2fc
batch_size                                 32                   16
n_conv_layers                               5                    4
optimizer                             rmsprop                 adam
```

### 実行のメトリックを更新します（実行終了後）

この例では、前の実行の精度を0.9に設定します。また、前回の実行の精度ヒストグラムをnumpy\_arrayのヒストグラムに変更します

```python
import wandb
api = wandb.Api()

run = api.run("<entity>/<project>/<run_id>")
run.summary["accuracy"] = 0.9
run.summary["accuracy_histogram"] = wandb.Histogram(numpy_array)
run.summary.update()
```

### 実行時に構成を更新します

この例では、構成設定の1つを更新します

```python
import wandb
api = wandb.Api()
run = api.run("<entity>/<project>/<run_id>")
run.config["key"] = 10
run.update()
```

### 1回の実行からCSVファイルにメトリックをエクスポートします

このスクリプトは、1回の実行で保存されたすべてのメトリックを検索し、それらをCSVに保存します。

```python
import wandb
api = wandb.Api()

# run is specified by <entity>/<project>/<run id>
run = api.run("<entity>/<project>/<run_id>")

# save the metrics for the run to a csv file
metrics_dataframe = run.history()
metrics_dataframe.to_csv("metrics.csv")
```

### サンプリングせずに大規模な単一実行からメトリックをエクスポートします

デフォルトの履歴メソッドは、メトリックを固定数のサンプルにサンプリングします（デフォルトは500です。これは、samples主張で変更できます）。大規模な実行ですべてのデータをエクスポートする場合は、run.scan\_history（）メソッドを使用できます。このスクリプトは、より長い実行のために、すべてのロスメトリックを可変ロスにロードします。

```python
import wandb
api = wandb.Api()

run = api.run("<entity>/<project>/<run_id>")
history = run.scan_history()
losses = [row["Loss"] for row in history]
```

### プロジェクト内のすべての実行からCSVファイルにメトリックをエクスポートします

このスクリプトはプロジェクトを検索し、名前、構成、要約統計量を含む実行のCSVを出力します。

```python
import wandb
api = wandb.Api()

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

### 実行からファイルをダウンロードします

### これにより、cifarプロジェクトの実行ID uxte44z7に関連付けられているファイル「model-best.h5」が検索され、ローカルに保存されます。

```python
import wandb
api = wandb.Api()
run = api.run("<entity>/<project>/<run_id>")
run.file("model-best.h5").download()
```

### 実行からすべてのファイルをダウンロードします

これにより、実行ID uxte44z7に関連付けられているすべてのファイルが検索され、ローカルに保存されます。（注：これは、コマンドラインからwandb restore &lt;RUN\_ID&gt;を実行することによっても実行できます。）

```python
import wandb
api = wandb.Api()
run = api.run("<entity>/<project>/<run_id>")
for file in run.files():
    file.download()
```

###  最高のモデルファイルをダウンロードします

```python
import wandb
api = wandb.Api()
sweep = api.sweep("<entity>/<project>/<sweep_id>")
runs = sorted(sweep.runs, key=lambda run: run.summary.get("val_acc", 0), reverse=True)
val_acc = runs[0].summary.get("val_acc", 0)
print(f"Best run {runs[0].name} with {val_acc}% validation accuracy")
runs[0].file("model-best.h5").download(replace=True)
print("Best model saved to model-best.h5")
```

### 特定のスイープから実行を取得します

```python
import wandb
api = wandb.Api()
sweep = api.sweep("<entity>/<project>/<sweep_id>")
print(sweep.runs)
```

###  システムメトリクスデータをダウンロードします

 これにより、実行のすべてのシステムメトリックを含むデータフレームが得られます。

```python
import wandb
api = wandb.Api()
run = api.run("<entity>/<project>/<run_id>")
system_metrics = run.history(stream = 'events')
```

### Update summary metrics

You can pass your dictionary to update summary metrics.

```python
summary.update({“key”: val})
```

###  サマリーメトリックを更新します

あなたの辞書を渡して、サマリーメトリックを更新できます。

```python
api = wandb.Api()
run = api.run("username/project/run_id")
meta = json.load(run.file("wandb-metadata.json").download())
program = ["python"] + [meta["program"]] + meta["args"]
```

### 履歴からページ付けされたデータを取得します

バックエンドでメトリックがゆっくりとフェッチされ、またはAPIリクエストがタイムアウトしている場合は、`scan_history`のページサイズを小さくして、個々のリクエストがタイムアウトしないようにすることができます。デフォルトのページサイズは1000であるため、さまざまなサイズを試して、最適なサイズを確認できます。

```python
api = wandb.Api()
run = api.run("username/project/run_id")
run.scan_history(keys=sorted(cols), page_size=100)
```



