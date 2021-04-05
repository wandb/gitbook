# Config

## wandb.sdk.wandb\_config

 [\[ソースを表示\]](https://github.com/wandb/client/blob/4a4de49c33117fcbb069439edeb509d54fd41176/wandb/sdk/wandb_config.py#L3)

 構成

###  構成オブジェクト

```python
class Config(object)
```

 [\[ソースを表示\]](https://github.com/wandb/client/blob/4a4de49c33117fcbb069439edeb509d54fd41176/wandb/sdk/wandb_config.py#L28)

 構成オブジェクト

構成オブジェクトは、wandb試行に関連付けられたすべてのハイパーパラメーターを保持することを目的としており、wandb.initが呼び出されたときに試行オブジェクトとともに保存されます。

トレーニング実験の先頭で一度wandb.configを設定するか、設定をinitのパラメータとして設定することをお勧めします。例：wandb.init（config=my\_config\_dict）

config-defaults.yamlというファイルを作成すると、自動的にwandb.configに読み込まれます。[https://docs.wandb.com/library/config\#file-based-configs](https://docs.wandb.com/library/config#file-based-configs)を参照してください。

  
カスタム名を使用して構成YAMLファイルをロードし、ファイル名をwandb.init（config="special\_config.yaml"）に渡すこともできます。[https://docs.wandb.com/library/config\#file-based-configs](https://docs.wandb.com/library/config#file-based-configs)を参照してください。

**例：**

 基本的な使い方

```text
wandb.config.epochs = 4
wandb.init()
for x in range(wandb.config.epochs):
# train
```

 wandb.initを使用して構成を設定する

```text
- `wandb.init(config={"epochs"` - 4, "batch_size": 32})
for x in range(wandb.config.epochs):
# train
```

 ネストされた構成

```text
wandb.config['train']['epochs] = 4
wandb.init()
for x in range(wandb.config['train']['epochs']):
# train
```

abslフラグの使用

```text
flags.DEFINE_string(‘model’, None, ‘model to run’) # name, default, help
wandb.config.update(flags.FLAGS) # adds all absl flags to config
```

 Argparseフラグ

```text
wandb.init()
wandb.config.epochs = 4

parser = argparse.ArgumentParser()
parser.add_argument('-b', '--batch-size', type=int, default=8, metavar='N',
help='input batch size for training (default: 8)')
args = parser.parse_args()
wandb.config.update(args)
```

TensorFlowフラグの使用（tensorflow v2では非推奨）

```text
flags = tf.app.flags
flags.DEFINE_string('data_dir', '/tmp/data')
flags.DEFINE_integer('batch_size', 128, 'Batch size.')
wandb.config.update(flags.FLAGS)  # adds all of the tensorflow flags to config
```

 **持続**

```python
 | persist()
```

 [\[ソースを表示\]](https://github.com/wandb/client/blob/4a4de49c33117fcbb069439edeb509d54fd41176/wandb/sdk/wandb_config.py#L163)

設定されている場合はコールバックを呼び出します

