# Sweeps Quickstart

任意の機械学習モデルから開始して、ハイパーパラメータスイープを数分で実行します。実用的な例を見たいですか？これが[サンプルコード](https://github.com/wandb/examples/tree/master/examples/pytorch/pytorch-cnn-fashion)と[サンプルダッシュボード](https://app.wandb.ai/carey/pytorch-cnn-fashion/sweeps/v8dil26q)です。

##  

![](../.gitbook/assets/image%20%2847%29%20%282%29%20%283%29%20%282%29.png)

{% hint style="info" %}
## すでにWeights＆Biasesプロジェクトがありますか？[次のスイープチュートリアルにスキップ→](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MN_4xmW6jcYndpU_n9G/v/japanese/sweeps/existing-project)
{% endhint %}

## 1. wandbの追加

**アカウントを設定します**

1. **W＆Bアカウントから始めます。**[**今すぐ作成→**](https://wandb.ai/)\*\*\*\*
2. **ターミナルのプロジェクトフォルダーに移動し、ライブラリをインストールします。`pip install wandb`**
3. **プロジェクトフォルダ内で、W＆Bにログインします。`wandb login`**

### **Pythonトレーニングスクリプトを設定します**

1. ライブラリ`wandb`をインポートします
2. スイープによってハイパーパラメータを適切に設定できることを確認します。スクリプトの上部にある辞書でそれらを定義し、wandb.initに渡します。 
3. メトリックをログに記録して、ライブダッシュボードに表示します。

```python
import wandb

# Set up your default hyperparameters before wandb.init
# so they get properly set in the sweep
hyperparameter_defaults = dict(
    dropout = 0.5,
    channels_one = 16,
    channels_two = 32,
    batch_size = 100,
    learning_rate = 0.001,
    epochs = 2,
    )

# Pass your defaults to wandb.init
wandb.init(config=hyperparameter_defaults)
config = wandb.config

# Your model here ...

# Log metrics inside your training loop
metrics = {'accuracy': accuracy, 'loss': loss}
wandb.log(metrics)
```

 [完全なコード例を見る→](https://github.com/wandb/examples/tree/master/examples/pytorch/pytorch-cnn-fashion)

## 2. スイープの構成

**YAML**ファイルを設定して、トレーニングスクリプト、パラメーター範囲、サーチストラテジー、および停止基準を指定します。W＆Bは、これらのパラメーターとその値をコマンドライン引数としてトレーニングスクリプトに渡し、ステップ1で設定した構成オブジェクトを使用して自動的に解析します。

いくつかの構成リソースは次のとおりです。

1.  [YAMLの例](https://github.com/wandb/examples/blob/master/examples/pytorch/pytorch-cnn-fashion/sweep-grid-hyperband.yaml)：スイープを実行するためのスクリプトとYAMLファイルのコード例
2.  [構成](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MN_4xmW6jcYndpU_n9G/v/japanese/sweeps/configuration)：スイープ構成をセットアップするための完全な仕様
3. J[upyterノートブック](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MN_4xmW6jcYndpU_n9G/v/japanese/sweeps/python-api)：YAMLファイルの代わりにPythonディクショナリを使用してスイープ構成をセットアップします
4.  [UIから構成を生成](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MN_4xmW6jcYndpU_n9G/v/japanese/sweeps/existing-project)：既存のW＆Bプロジェクトを取得し、構成ファイルを生成します
5.  [前の実行でフィード](https://docs.wandb.com/sweeps/overview/add-to-existing#seed-a-new-sweep-with-existing-runs)：前の実行を取得し、新しいスイープに追加します

  
   ****

以下は、**sweep.yaml**と呼ばれるスイープ設定YAMLファイルの例です。

```text
program: train.py
method: bayes
metric:
  name: val_loss
  goal: minimize
parameters:
  learning_rate:
    min: 0.001
    max: 0.1
  optimizer:
    values: ["adam", "sgd"]
```

{% hint style="warning" %}
最適化するメトリックを指定する場合は、それをログに記録していることを確認してください。この例では、構成ファイルに**val\_loss**があるため、その正確なメトリック名をスクリプトに記録する必要があります。

`wandb.log({"val_loss": validation_loss})`
{% endhint %}

```text
python train.py --learning_rate=0.005 --optimizer=adam
python train.py --learning_rate=0.03 --optimizer=sgd
```

{% hint style="info" %}
内部的には、この構成例ではベイズ最適化手法を使用して、プログラムを呼び出すためのハイパーパラメーター値のセットを選択します。次のシンタックス構文で実験を開始します。スクリプトでargparseを使用している場合は、変数名にハイフンではなくアンダースコアを使用することをお勧めします。
{% endhint %}

## 3.  スイープの初期化

中央サーバーは、スイープを実行するすべてのエージェント間で調整します。スイープ設定ファイルを設定し、次のコマンドを実行して開始します。

このコマンドは、エンティティ名とプロジェクト名を含む**スイープID**を出力します。それをコピーして次のステップで使用してください！

```text
wandb sweep sweep.yaml
```

## 4. エージェントの起動 

スイープを実行する各マシンで、スイープIDを使用してエージェントを起動します。同じスイープを実行しているすべてのエージェントに同じスイープIDを使用することをお勧めします。

自分のマシンのシェルで、実行するコマンドをサーバーに要求するwandbエージェントコマンドを実行します。

wandbエージェントは、複数のマシンまたは同じマシンの複数のプロセスで実行できます。各エージェントは、実行するハイパーパラメータの次のセットについて中央のW＆Bスイープサーバーをポーリングします。 

```text
wandb agent your-sweep-id.
```

## 5. 結果の視覚化

 プロジェクトを開いて、スイープダッシュボードでライブ結果を確認します。

 [ダッシュボードの例→](https://wandb.ai/carey/pytorch-cnn-fashion)

![](../.gitbook/assets/image%20%2888%29%20%282%29%20%281%29.png)

