# Ray Tune

W＆Bは、2つの軽量統合を提供することにより、 [Ray](https://github.com/ray-project/ray)と統合します。

つは`WandbLogger`で、Tuneに報告されたメトリックをWandb APIに自動的に記録します。もう1つは`@wandb_mixin`デコレータで、関数APIで使用できます。Tuneのトレーニング情報を使用してWandbAPIを自動的に初期化します。通常どおりにWandbAPIを使用できます。`wandb.log（）`を使用してトレーニングプロセスをログに記録します。

## WandbLogger

```python
from ray.tune.integration.wandb import WandbLogger
```

Wandbの構成は、`wandb`キーを`tune.run（）`の構成パラメータに渡すことによって行われます（以下の例を参照）。

wandb構成エントリの内容は、キーワード引数として`wandb.init（）`に渡されます。例外は、WandbLogger自体を構成するために使用される次の設定です。

### **パラメータ**

`api_key_file (str)` – `Wandb API KEY`を含むファイルへのパス。

`api_key (str)` – `Wandb API Key`。api\_key\_fileを設定する代わりに。

`excludes (list)` – ログから除外する必要があるメトリックのリスト。

`log_config (bool)` – 結果dictのconfigパラメータをログに記録する必要があるかどうかを示すブール値。これは、トレーニング中にパラメータが変更される場合に意味があります。例えば、PopulationBasedTrainingを使用する場合です。デフォルトはFalseです。

### **例**

```python
from ray.tune.logger import DEFAULT_LOGGERS
from ray.tune.integration.wandb import WandbLogger
tune.run(
    train_fn,
    config={
        # define search space here
        "parameter_1": tune.choice([1, 2, 3]),
        "parameter_2": tune.choice([4, 5, 6]),
        # wandb configuration
        "wandb": {
            "project": "Optimization_Project",
            "api_key_file": "/path/to/file",
            "log_config": True
        }
    },
    loggers=DEFAULT_LOGGERS + (WandbLogger, ))
```

## wandb\_mixin

```python
ray.tune.integration.wandb.wandb_mixin(func)
```

 このRayTune Trainable `mixin`は、`Trainable`クラスまたは関数APIの`@wandb_mixin`で使用するためにWandb APIを初期化するのに役立ちます。基本的な使用法については、トレーニング関数の前に`@wandb_mixin`デコレータを追加するだけです。

```python
from ray.tune.integration.wandb import wandb_mixin

@wandb_mixin
def train_fn(config):
    wandb.log()
```

Wandb configuration is done by passing a `wandb key` to the `config` parameter of `tune.run()` \(see example below\).

Wandbの構成は、wandbキーを`tune.run（）`の構成パラメータに渡すことによって行われます（以下の例を参照）。wandb構成エントリの内容は、キーワード引数として`wandb.init（）`に渡されます。例外は、WandbTrainableMixin自体を構成するために使用される次の設定です。

### **パラメータ**

`api_key_file (str)` – Wandb `API KEY`を含むファイルへのパス。

`api_key (str)` – Wandb API Key。`api_key_file`を設定する代わりに。

 Wandbのグループ`run_id`と`run_name`は、Tuneによって自動的に選択されますが、それぞれの構成値を入力することで上書きできます。

他のすべての有効な構成設定については、[https://docs.wandb.com/library/init](https://docs.wandb.com/library/init)を参照してください。

### **例：**

```python
from ray import tune
from ray.tune.integration.wandb import wandb_mixin

@wandb_mixin
def train_fn(config):
    for i in range(10):
        loss = self.config["a"] + self.config["b"]
        wandb.log({"loss": loss})
        tune.report(loss=loss)

tune.run(
    train_fn,
    config={
        # define search space here
        "a": tune.choice([1, 2, 3]),
        "b": tune.choice([4, 5, 6]),
        # wandb configuration
        "wandb": {
            "project": "Optimization_Project",
            "api_key_file": "/path/to/file"
        }
    })
```

##  **サンプルコード**

統合がどのように機能するかを確認するために、いくつかの例を作成しました。

* [Colab](https://colab.research.google.com/drive/1an-cJ5sRSVbzKVRub19TmmE4-8PUWyAi?usp=sharing): 統合を試すための簡単なデモ
*  [ダッシュボード](https://app.wandb.ai/authors/rayTune?workspace=user-cayush)：例から生成されたダッシュボードを表示

