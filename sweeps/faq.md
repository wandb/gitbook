# Common Questions

##  **プロジェクトとエンティティの設定**

コマンドを実行してスイープを開始するときは、そのコマンドにプロジェクトとエンティティを指定する必要があります。別のものが必要な場合は、次の4つの方法で指定できます。

1. **1.wandbスイープへのコマンドライン引数（--projectおよび--entity引数）**
2. **2.wandb設定ファイル（プロジェクトキーおよびエンティティキー）**
3. [sweep configuration](configuration.md) \(`project` and `entity` keys\) 
4. [environment variables](../library/environment-variables.md) \(`WANDB_PROJECT` and `WANDB_ENTITY` variables\)

##  **スイープエージェントは、最初の実行が終了した後に停止します**

`wandb: ERROR Error while calling W&B API: anaconda 400 error: {"code":400,"message":"TypeError: bad operand type for unary -: 'NoneType'"}`

これの一般的な理由の1つは、構成YAMLファイルで最適化するメトリックがログに記録するメトリックではないことです。たとえば、メトリック**f1**を最適化するが、**validation\_f1**をログに記録することができます。最適化する正確なメトリック名をログに記録していることを再確認してください。

## 試行する実行回数を設定します

ランダム検索は、スイープを停止するまで永久に実行されます。メトリックの特定の値に達したときにスイープを自動的に停止するようにターゲットを設定するか、エージェントが試行する実行回数を指定できます。wandbエージェント—count NUM SWEEPID

## Slurmでスイープを実行します

`wandb agent --count 1 SWEEP_ID`を実行することをお勧めします。これにより、単一のトレーニングジョブが実行されて終了します。

## グリッド検索を再実行します

グリッド検索を使い果たしたが、一部の実行を再実行したい場合は、再実行したいものを削除してから、スイープ制御ページの再開ボタンを押して、そのスイープIDの新しいエージェントを開始できます。



##  スイープとRunは同じプロジェクトに含まれている必要があります

`wandb: WARNING Ignoring project='speech-reconstruction-baseline' passed to wandb.init when running a sweep`

スイープの実行時にwandb.init（）を使用してプロジェクトを設定することはできません。スイープと実行は同じプロジェクト内にある必要があるため、プロジェクトはスイープの作成によって設定されます。`wandb.sweep(sweep_config, project=“fdsfsdfs”)`

## アップロード中にエラーが発生しました

&lt;file&gt;のアップロード中にERRORエラーが表示された場合：CommError、Runが存在しません。実行のID wandb.init\(id="some-string"\)を設定している可能性があります。このIDはプロジェクト内でユニークである必要があり、ユニークでない場合はスローされてエラーが発生します。スイープのコンテキストでは、実行に対してランダムで一意のIDが自動的に生成されるため、実行の手動IDを設定することはできません。表やグラフに表示する適切な名前を取得しようとしている場合は、idの代わりにnameを使用することをお勧めします。例えば：`wandb.init(name="a helpful readable run name")`

```python
wandb.init(name="a helpful readable run name")
```

## カスタムコマンドでスイープ

通常、コマンドと引数を使用してトレーニングを実行する場合、次に例を示します。

```text
edflow -b <your-training-config> --batch_size 8 --lr 0.0001
```

 これを次のようにスイープ構成に変換できます。

```text
program:
  edflow
command:
  - ${env}
  - python
  - ${program}
  - "-b"
  - your-training-config
  - ${args}
```

${args}キーは、スイープ構成ファイル内のすべてのパラメーターに展開され、argparseで解析できるように展開されます。--param1 value1 --param2 value2argparseで指定したくない余分な引数がある場合は、次を使用できます。parser = argparse.ArgumentParser\(\) args, unknown = parser.parse\_known\_args\(\)

**Python3でスイープを実行します**

スイープがPython2を使用しようとしているという問題が発生している場合は、代わりにPython3を使用するように指定するのは簡単です。これをスイープ設定YAMLファイルに追加するだけです。

```text
program:
  script.py
command:
  - ${env}
  - python3
  - ${program}
  - ${args}
```

\*\*\*\*

##  ベイズ最適化の詳細

ベイズ最適化に使用されるガウス過程モデルは、[オープンソースのスイープロジック](https://github.com/wandb/client/tree/master/wandb/sweeps)で定義されています。追加の構成可能性と制御が必要な場合は、[Ray Tune](https://docs.wandb.com/sweeps/ray-tune)のサポートをお試しください。ここでオープンソースコードで定義されているRBFの一般化である[Maternカーネル](https://scikit-learn.org/stable/modules/generated/sklearn.gaussian_process.kernels.Matern.html)を使用します。

## **スイープの一時停止とスイープwandb.agentの停止**

スイープを一時停止したために使用可能なジョブがなくなったときに `wandb`エージェントを終了させる方法はありますか？スイープを一時停止せずに停止すると、エージェントは終了します。一時停止の場合は、エージェントを再度起動せずにスイープを再開できるように、エージェントを実行したままにします。

## スイープで設定パラメータを設定するための推奨される方法

`wandb.init(config=config_dict_that_could_have_params_set_by_sweep)`  
or:  
`experiment = wandb.init()    
experiment.config.setdefaults(config_dict_that_could_have_params_set_by_sweep)`

 この利点は、スイープによってすでに設定されているキーの設定を無視することです。

##  スイープにカテゴリ値を追加する方法はありますか？それとも新しい値を開始する必要がありますか？

 スイープが開始されると、スイープ構成を変更することはできませんが、任意のテーブルビューに移動し、チェックボックスを使用して実行を選択してから、\[スイープの作成\]メニューオプションを使用して、以前の実行で新しいスイープを作成できます

