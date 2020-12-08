---
description: 実験構成を保存するための辞書のようなオブジェクト
---

# wandb.config

##  概要

[![](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/wandb/examples/blob/master/colabs/wandb-config/Configs_in_W%26B.ipynb)

syntax. スクリプトに`wandb.config`オブジェクトを設定して、ハイパーパラメータ、データセット名やモデルタイプなどの入力設定、および実験用の他の独立変数などのトレーニング構成を保存します。これは、実験を分析し、将来の作業を再現するのに役立ちます。Webインターフェースで構成値ごとにグループ化し、さまざまな実行設定を比較して、これらが出力にどのように影響するかを確認できます。出力メトリックまたは従属変数（ロスや精度など）は、wandb.loginsteadで保存する必要があることに注意してください。configでネストされた辞書を送信できます。バックエンドでドットを使用して名前をフラット化します。構成変数名にドットを使用せず、代わりにダッシュまたはアンダースコアを使用することをお勧めします。wandb構成ディクショナリを作成した後、スクリプトがルートの下のwandb.configキーにアクセスする場合は、`.`シンタックスの代わりに`[ ]`シンタックスを使用します

##  簡単な例

```python
wandb.config.epochs = 4
wandb.config.batch_size = 32
# you can also initialize your run with a config
wandb.init(config={"epochs": 4})
```

##  効率的な初期化

 `wandb.config`を辞書として扱い、一度に複数の値を更新できます。

```python
wandb.init(config={"epochs": 4, "batch_size": 32})
# or
wandb.config.update({"epochs": 4, "batch_size": 32})
```

## Argparseフラグ

argparseから引数辞書に渡すことができます。これは、コマンドラインからさまざまなハイパーパラメータ値をすばやくテストするのに便利です。

```python
wandb.init()
wandb.config.epochs = 4

parser = argparse.ArgumentParser()
parser.add_argument('-b', '--batch-size', type=int, default=8, metavar='N',
                     help='input batch size for training (default: 8)')
args = parser.parse_args()
wandb.config.update(args) # adds all of the arguments as config variables
```

## Abslフラグ

abslフラグを渡すこともできます。

```python
flags.DEFINE_string(‘model’, None, ‘model to run’) # name, default, help
wandb.config.update(flags.FLAGS) # adds all absl flags to config
```

## ファイルベースの構成

**config-defaults.yaml**というファイルを作成できます。\_\_これは自動的に`wandb.config`に読み込まれます。

```yaml
# sample config defaults file
epochs:
  desc: Number of epochs to train over
  value: 100
batch_size:
  desc: Size of each mini-batch
  value: 32
```

コマンドライン引数`--configsspecial-configs.yaml`を使用して、wandbにさまざまな構成ファイルをロードするように指示できます。

これにより、ファイルspecial-configs.yamlからパラメータがロードされます。ユースケースの例：実行用のメタデータを含むYAMLファイルがあり、Pythonスクリプトにハイパーパラメータのディクショナリがあります。ネストされた構成オブジェクトに両方を保存できます。

```python
hyperparameter_defaults = dict(
    dropout = 0.5,
    batch_size = 100,
    learning_rate = 0.001,
    )

config_dictionary = dict(
    yaml=my_yaml_file,
    params=hyperparameter_defaults,
    )

wandb.init(config=config_dictionary)
```

## データセット識別子

 `wandb.config`を使用して実験への入力として追跡することにより、データセットの実行の構成にユニークな識別子（ハッシュやその他の識別子など）を追加できます。

```yaml
wandb.config.update({'dataset':'ab131'})
```

### 構成ファイルの更新

パブリックAPIを使用して構成ファイルを更新できます

```yaml
import wandb
api = wandb.Api()
run = api.run("username/project/run_id")
run.config["foo"] = 32
run.update()
```

### キーおよび値のペア

任意のキーと値のペアをwandb.configに記録できます。それらは、トレーニングしているモデルのタイプごとに異なります。つまり、 `wandb.config.update({"my_param": 10, "learning_rate": 0.3, "model_architecture": "B"})`

## TensorFlowフラグ（tensorflow v2では非推奨）

TensorFlowフラグを構成オブジェクトに渡すことができます。 

```python
wandb.init()
wandb.config.epochs = 4

flags = tf.app.flags
flags.DEFINE_string('data_dir', '/tmp/data')
flags.DEFINE_integer('batch_size', 128, 'Batch size.')
wandb.config.update(flags.FLAGS)  # adds all of the tensorflow flags as config
```

