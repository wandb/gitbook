---
description: >-
  Syntax to set the hyperparameter ranges, search strategy, and other aspects of
  your swe
---

# Configuration

ハイパーパラメータ範囲、サーチストラテジー、およびスイープの他の側面を設定するためのシンタックスこれらの構成フィールドを使用して、スイープをカスタマイズします。

構成を指定する方法は2つあります。

1. [YAMLファイル](https://docs.wandb.com/sweeps/overview/quickstart#2-sweep-config)：分散スイープに最適です。[こちら](https://docs.wandb.com/sweeps/overview/quickstart#2-sweep-config)の例をご覧ください。
2. [Pythonデータ構造](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MNdlQobOrN8f63KfkRZ/v/japanese/sweeps/python-api)：JupyterNotebookからスイープを実行するのに最適です

| トップレベルキー | 意味 |
| :--- | :--- |
| name | W＆B UIに表示されるスイープの名前 |
| description | スイープのテキスト説明（注） |
| program |  ****実行するトレーニングスクリプト（必須） |
| metric |   最適化するメトリックを指定します（一部の検索戦略および停止基準で使用されます） |
| method | サーチストラテジーを指定します（必須）  |
| early\_terminate | 停止基準を指定します（オプション、デフォルトでは早期停止なし） |
| parameters | 停止基準を指定します（オプション、デフォルトでは早期停止なし） |
| project | このスイープのプロジェクトを指定します |
| entity |  このスイープのプロジェクトを指定します |
| command | このスイープのエンティティを指定します |

###  メトリック

 最適化するメトリックを指定します。このメトリックは、トレーニングスクリプトによってW＆Bに明示的に記録する必要があります。たとえば、モデルの検証損失を最小限に抑えたい場合は、次のようにします。

```python
# [model training code that returns validation loss as valid_loss]
wandb.log({"val_loss" : valid_loss})
```

| `metric` sub-key | Meaning |
| :--- | :--- |
| name | 最適化するメトリックの名前 |
| goal | 最適化するメトリックの名前 |
| target | 最適化する指標に対して達成したい値。スイープのいずれかの実行がその目標値に達すると、スイープの状態は「終了」に設定されます。つまり、アクティブな実行を持つすべてのエージェントはそれらのジョブを終了しますが、スイープで新しい実行は開始されません。 |

{% hint style="danger" %}
**指定するメトリックは、「トップレベル」メトリックである必要があります。**

**これは機能無し：スイープ構成：メトリック：名前：**`nested_metrics = {"nested": 4} wandb.log({"my_metric", nested_metrics}`

**この制限を回避するには、スクリプトは次のようにネストされたメトリックをトップレベルでログに記録する必要があります。スイープ構成：メトリック：名前：**my\_metric\_nested Code: `nested_metrics = {"nested": 4} wandb.log{{"my_metric", nested_metric} wandb.log({"my_metric_nested", nested_metric["nested"]})`  
metric:  
name: my\_metric\_nested  
Code:  
`nested_metrics = {"nested": 4}    
wandb.log{{"my_metric", nested_metric}    
wandb.log({"my_metric_nested", nested_metric["nested"]})`
{% endhint %}

**Examples** 

{% tabs %}
{% tab title="Maximize" %}
```text
metric:
  name: val_loss
  goal: maximize
```
{% endtab %}

{% tab title="Minimize" %}
```text
metric:
  name: val_loss
```
{% endtab %}

{% tab title="Target" %}
```text
metric:
  name: val_loss
  goal: maximize
  target: 0.1
```
{% endtab %}
{% endtabs %}

###  **例**

スイープ構成の`method`キーを使用してサーチストラテジーを指定します。

| `method` | Meaning |
| :--- | :--- |
| grid | グリッド検索は、パラメーター値のすべての可能な組み合わせを繰り返します。 |
| random | ランダム検索では、ランダムな値のセットが選択されます |
| bayes |  ベイズ最適化は、ガウス過程を使用して関数をモデル化し、パラメーターを選択して改善の確率を最適化します。この戦略では、メトリックキーを指定する必要があります。 |

 **例**

{% tabs %}
{% tab title="Random search" %}
```text
method: random
```
{% endtab %}

{% tab title="Grid search" %}
```text
method: grid
```
{% endtab %}

{% tab title="Bayes search" %}
```text
method: bayes
metric:
  name: val_loss
  goal: minimize
```
{% endtab %}
{% endtabs %}

### 停止基準

早期終了は、パフォーマンスの低い実行を停止することでハイパーパラメータ検索を高速化するオプション機能です。早期停止がトリガーされると、エージェントは現在の実行を停止し、試行するハイパーパラメータの次のセットを取得します。

| `early_terminate` sub-key | Meaning |
| :--- | :--- |
| type | specify the stopping algorithm |

We support the following stopping algorithm\(s\):

| `type` | Meaning |
| :--- | :--- |
| hyperband | Use the [hyperband method](https://arxiv.org/abs/1603.06560) |

ハイパーバンド停止は、プログラムの実行中にプログラムを停止するか、1つ以上の括弧で続行することを許可するかを評価します。ブラケットは、指定されたメトリックの静的反復で構成されます（反復は、メトリックがログに記録された回数です。メトリックがエポックごとにログに記録される場合、エポック反復があります）。

ブラケットスケジュールを指定するには、`min_iter`または`max_iter`のいずれかを定義する必要があります。

| `early_terminate` sub-key | Meaning |
| :--- | :--- |
| min\_iter | 最初の括弧の反復を指定します |
| max\_iter | プログラムの最大反復回数を指定します |
| s | 角かっこの総数を指定します（`max_iter`に必要） |
| eta | ブラケット乗数スケジュールを指定します（デフォルト：3） |

 **例**

{% tabs %}
{% tab title="ハイパーバンド（min\_iter）" %}
```text
early_terminate:
  type: hyperband
  min_iter: 3
```

Brackets: 3, 9 \(3\*eta\), 27 \(9 \* eta\), 81 \(27 \* eta\)
{% endtab %}

{% tab title="ハイパーバンド（max\_iter）" %}
```text
early_terminate:
  type: hyperband
  max_iter: 27
  s: 2
```

Brackets: 9 \(27/eta\), 3 \(9/eta\)
{% endtab %}
{% endtabs %}

### パラメーター

探索するハイパーパラメータについて説明します。ハイパーパラメータごとに、名前と可能な値を定数のリストとして指定するか（任意のメソッドの場合）、分布を指定します（ランダムまたはベイの場合）。

| Values | Meaning |
| :--- | :--- |
| values: \[\(type1\), \(type2\), ...\] |  このハイパーパラメータのすべての有効な値を指定します。グリッドと互換性があります。 |
| value: \(type\) | このハイパーパラメータの単一の有効な値を指定します。グリッドと互換性があります。 |
| distribution: \(distribution\) | 以下の分布表から分布を選択します。指定しない場合、値が設定されている場合はデフォルトでカテゴリカル、最大値と最小値が整数に設定されている場合は |
| min: \(float\) max: \(float\) | int\_uniform、最大値と最小値が浮動小数点数に設定されている場合は均一、値が設定されている場合は一定になります。 |
| min: \(int\) max: \(int\) | 均一に分散されたハイパーパラメータの有効な最大値と最小値。 |
| mu: \(float\) | int\_uniform分散ハイパーパラメータの最大値と最小値 |
| sigma: \(float\) | 正規分布または対数正規分布のハイパーパラメータの標準偏差パラメータ。 |
| q: \(float\) | 量子化されたハイパーパラメータの量子化ステップサイズ |

 **例**

{% tabs %}
{% tab title="grid - single value" %}
```text
parameter_name:
  value: 1.618
```
{% endtab %}

{% tab title="grid - multiple values" %}
```text
parameter_name:
  values:
  - 8
  - 6
  - 7
  - 5
  - 3
  - 0
  - 9
```
{% endtab %}

{% tab title="random or bayes - normal distribution" %}
```text
parameter_name:
  distribution: normal
  mu: 100
  sigma: 10
```
{% endtab %}
{% endtabs %}

### Distributions

| 名前 |  意味 |
| :--- | :--- |
| constant | 一定の分布。値を指定する必要があります。 |
| categorical | カテゴリ分布。値を指定する必要があります。 |
| int\_uniform | 整数の離散一様分布。maxとminを整数として指定する必要があります。 |
| uniform | 連続一様分布。maxとminをfloatとして指定する必要があります。 |
| q\_uniform | 量子化された一様分布。round（X / q）\* qを返します。ここで、Xは均一です。qのデフォルトは1です。 |
| log\_uniform | 対数均一分布。自然対数が最小値と最大値の間で均一に分布するように、exp（最小）とexp（最大）の間の値を返します。 |
| q\_log\_uniform | 量子化されたログユニフォーム。round（X / q）\* qを返します。ここで、Xはlog\_uniformです。qのデフォルトは1です。 |
| normal | 正規分布。戻り値は通常、平均ミュー（デフォルトは0）と標準偏差シグマ（デフォルトは1）で分布しています。 |
| q\_normal | Quantized normal distribution. Returns `round(X / q) * q` where `X` is `normal`. Q defaults to 1. |
| log\_normal | Log normal distribution. Returns a value `X` such that the natural logarithm `log(X)` is normally distributed with mean `mu` \(default `0`\) and standard deviation `sigma` \(default `1`\). |
| q\_log\_normal | 量子化された対数正規分布。round（X / q）\* qを返します。ここで、Xはlog\_normalです。qのデフォルトは1です。 |

 **例**

{% tabs %}
{% tab title="constant" %}
```text
parameter_name:
  distribution: constant
  value: 2.71828
```
{% endtab %}

{% tab title="categorical" %}
```text
parameter_name:
  distribution: categorical
  values:
  - elu
  - celu
  - gelu
  - selu
  - relu
  - prelu
  - lrelu
  - rrelu
  - relu6
```
{% endtab %}

{% tab title="uniform" %}
```text
parameter_name:
  distribution: uniform
  min: 0
  max: 1
```
{% endtab %}

{% tab title="q\_uniform" %}
```text
parameter_name:
  distribution: q_uniform
  min: 0
  max: 256
  q: 1
```
{% endtab %}
{% endtabs %}

### コマンドライン <a id="command"></a>

スイープエージェントは、デフォルトで次の形式でコマンドラインを作成します。

```text
/usr/bin/env python train.py --param1=value1 --param2=value2
```

{% hint style="info" %}
Windowsマシンでは、/usr/bin/envは省略されます。UNIXシステムでは、環境に基づいて適切なpythonインタープリターが選択されるようにします。
{% endhint %}

このコマンドラインは、構成ファイルで`command`キーを指定することで変更できます。デフォルトでは、コマンドは次のように定義されています。

```text
command:
  - ${env}
  - ${interpreter}
  - ${program}
  - ${args}
```

| コマンドマクロ | 拡張 |
| :--- | :--- |
| ${env} | UNIXシステムでは/usr/bin/env、Windowsでは省略します |
| ${interpreter\| | 「python」に展開されます。 |
| ${program} | スイープ構成プログラムキーで指定されたトレーニングスクリプト |
| ${args} | --param1=value1 --param2=value2の形式の展開された引数 |
| ${args\_no\_hyphens} | Expanded arguments in the form param1=value1 param2=value2 |
| ${json} |  param1=value1 param2=value2の形式の展開された引数 |
| ${json\_file} |  JSONとしてエンコードされた引数 |

: 例：

{% tabs %}
{% tab title="Set python interpreter" %}
Pythonインタープリターをハードコーディングするために、インタープリターを明示的に指定できます

```text
command:
  - ${env}
  - python3
  - ${program}
  - ${args}
```
{% endtab %}

{% tab title="Add extra parameters" %}
Add extra command line arguments not specified by sweep configuration parameters:

```text
command:
  - ${env}
  - ${interpreter}
  - ${program}
  - "-config"
  - your-training-config
  - ${args}
```
{% endtab %}

{% tab title="Omit arguments" %}
If your program does not use argument parsing you can avoid passing arguments all together and take advantage of `wandb.init()` picking up sweep parameters automatically:

```text
command:
  - ${env}
  - ${interpreter}
  - ${program}
```
{% endtab %}

{% tab title="Use with Hydra" %}
You can change the command to pass arguments they way tools like Hydra expect.

```text
command:
  - ${env}
  - ${interpreter}
  - ${program}
  - ${args_no_hyphens}
```
{% endtab %}
{% endtabs %}

##  よくある質問

### ネストされた構成現在、

スイープはネストされた値をサポートしていませんが、近い将来これをサポートする予定です。.

