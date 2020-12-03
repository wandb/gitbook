---
description: How to integrate a Keras script to log metrics to W&B
---

# Keras

Kerasコールバックを使用して、`model.fit`で追跡されたすべてのメトリックと損失値を自動的に保存します。

{% code title="example.py" %}
```python
import wandb
from wandb.keras import WandbCallback
wandb.init(config={"hyper": "parameter"})

# Magic

model.fit(X_train, y_train,  validation_data=(X_test, y_test),
          callbacks=[WandbCallback()])
```
{% endcode %}

 [colabノートブック](https://colab.research.google.com/github/wandb/examples/blob/master/colabs/keras/Simple_Keras_Integration.ipynb)で統合を試してください。[ビデオチュートリアル](https://www.youtube.com/watch?v=Bsudo7jbMow&ab_channel=Weights%26Biases)を完備しているか、完全なスクリプトの例については[サンプルプロジェクト](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MN_4xmW6jcYndpU_n9G/v/japanese/examples)を参照してください。

#### オプション

Keras `WandbCallback（）`クラスは、いくつかのオプションをサポートしています

| キーワード引数 | デフォルト | 説明 |
| :--- | :--- | :--- |
| monitor | val\_loss |  最良のモデルを保存するためのパフォーマンスを測定するために使用されるトレーニングメトリック。つまり、val\_loss |
| mode | auto | ‘min'、‘max'、または‘auto'：モニターで指定されたトレーニングメトリックをステップ間で比較する方法 |
| save\_weights\_only | False | モデル全体ではなく、重みのみを保存します |
| save\_model | True | 各ステップで改善された場合はモデルを保存します |
| log\_weights | False | 各エポックでの各レイヤーパラメータの値をログに記録します |
| log\_gradients | False | 各エポックでの各レイヤーパラメータのグラデーションをログに記録します |
| training\_data | None | グラデーションの計算に必要なタプル（X、y） |
| data\_type | None |  保存しているデータの種類。現在、「画像」のみがサポートされています |
| labels | None | data\_typeが指定されている場合にのみ使用され、分類子を作成している場合に数値出力を変換するラベルのリスト。（二項分類をサポート） |
| predictions | 36 | data\_typeが指定されている場合に行う予測の数。最大は100です。 |
| generator | None | データ拡張とdata\_typeを使用する場合は、予測を行うためのジェネレーターを指定できます。 |

## よくある質問

### **wandbでKerasマルチプロセッシングを使用します**

`use_multiprocessing=True`を設定していて、`Error('You must call wandb.init() before wandb.config.batch_size')`のエラーが表示されている場合は、次のことを試してください。

1. Sequenceクラスのinitに、次を追加します。`wandb.init(group='...')`
2. メインプログラムで、`if __name__ == "__main__":`を使用していることを確認してから、残りのスクリプトロジックをその中に配置します。

##  例

 統合がどのように機能するかを確認するために、いくつかの例を作成しました。

*  [Githubの例](https://github.com/wandb/examples/blob/master/examples/keras/keras-cnn-fashion/train.py)：PythonスクリプトのファッションMNISTの例
* Google Colabで実行：開始するための簡単なノートブックの例
* [Wandbダッシュボード](https://wandb.ai/wandb/keras-fashion-mnist/runs/5z1d85qs)：W＆Bで結果を表示

