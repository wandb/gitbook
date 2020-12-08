---
description: How to integrate a TensorFlow script to log metrics to W&B
---

# TensorFlow

すでにTensorBoardを使用している場合は、wandbと簡単に統合できます。

```python
import tensorflow as tf
import wandb
wandb.init(config=tf.flags.FLAGS, sync_tensorboard=True)
```

 完全なスクリプト例については、[サンプルプロジェクト](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MN_4xmW6jcYndpU_n9G/v/japanese/examples)を参照してください。

## **カスタムメトリック**

TensorBoardに記録されていない追加のカスタムメトリックをログに記録する必要がある場合は、TensorBoardが使用しているのと同じステップ引数を使用してコードでwandb.logを呼び出すことができます。つまり、 `wandb.log({"custom": 0.8}, step=global_step)`です。

### **TensorFlowフック**

ログに記録される内容をより細かく制御したい場合、wandbはTensorFlow推定器のフックも提供します。グラフ内のすべてのtf.summary値をログに記録します。

```python
import tensorflow as tf
import wandb

wandb.init(config=tf.FLAGS)

estimator.train(hooks=[wandb.tensorflow.WandbHook(steps_per_log=1000)])
```

### **手動ログ**

TensorFlowでメトリックをログに記録する最も簡単な方法は、TensorFlowロガーでtf.summaryをログに記録することです。

```python
import wandb

with tf.Session() as sess:
    # ...
    wandb.tensorflow.log(tf.summary.merge_all())
```

TensorFlow 2では、カスタムループを使用してモデルをトレーニングするための推奨される方法は、`tf.GradientTape`を使用することです。あなたはそれについて[ここで](https://www.tensorflow.org/tutorials/customization/custom_training_walkthrough)もっと知ることができます。`wandb`を組み込んでカスタムTensorFlowトレーニングループにメトリックを記録する場合は、このスニペットに従うことができます

```python
    with tf.GradientTape() as tape:
        # Get the probabilities
        predictions = model(features)
        # Calculate the loss
        loss = loss_func(labels, predictions)

    # Log your metrics
    wandb.log("loss": loss.numpy())
    # Get the gradients
    gradients = tape.gradient(loss, model.trainable_variables)
    # Update the weights
    optimizer.apply_gradients(zip(gradients, model.trainable_variables))
```

 完全な例は[ここに](https://www.wandb.com/articles/wandb-customizing-training-loops-in-tensorflow-2)あります。

## **W＆BはTensorBoardとどう違うのですか？**

当社は、すべての人のために実験追跡ツールの改善を促しました。共同創設者がW＆Bに取り組み始めたとき、彼らはOpenAIで欲求不満のTensorBoardユーザーのためのツールを構築するように促されました。以下は、当社が改善する上で力を入れた項目です。

1. **モデルの再現**：Weights＆Biasesは、実験、調査、および後でモデルを再現するのに適しています。メトリックだけでなく、ハイパーパラメータとコードのバージョンもキャプチャし、プロジェクトの再現性を高めるためにモデルのチェックポイントを保存できます。
2. **自動編成**：プロジェクトを共同作業者に引き渡したり、休暇を取ったりした場合、W＆Bを使用すると、試したすべてのモデルを簡単に確認できるため、古い実験を再実行するのに時間を無駄にすることはありません。
3. **高速で柔軟な統合**：5分でプロジェクトにW＆Bを追加します。無料のオープンソースPythonパッケージをインストールし、コードに数行追加すると、モデルを実行するたびに、ログに記録された優れたメトリックとレコードが得られます。
4. **統一化および一元化されたダッシュボード**：ローカルマシン、ラボクラスター、クラウド内のスポットインスタンスなど、モデルをトレーニングする場所ならどこでも、同じ一元化されたダッシュボードを提供します。異なるマシンからTensorBoardファイルをコピーして整理するのに時間を費やす必要はありません。
5.  **効果的な表**：さまざまなモデルの結果を検索、フィルタリング、並べ替え、グループ化します。何千ものモデルバージョンを調べて、さまざまなタスクに最適なモデルを見つけるのは簡単です。TensorBoardは、大規模なプロジェクトでうまく機能するようには構築されていません
6. **コラボレーションのためのツール**：W＆Bを使用して、複雑な機械学習プロジェクトを整理します。W＆Bへのリンクを共有するのは簡単で、プライベートチームを使用して、全員が共有プロジェクトに結果を送信することができます。また、レポートを介したコラボレーションもサポートしています。インタラクティブな視覚化を追加し、マークダウンで作業を説明します。これは、作業ログを保持したり、調査結果を上司と共有したり、調査結果をラボに提示したりするための優れた方法です。

 [無料の個人アカウント](http://app.wandb.ai/)を始めましょう[→](http://app.wandb.ai/)

## **例**

統合がどのように機能するかを確認するために、いくつかの例を作成しました。

* [Githubの例](https://github.com/wandb/examples/blob/master/examples/tensorflow/tf-estimator-mnist/mnist.py)：TensorFlowEstimatorsを使用したMNISTの例
*  [Githubの例](https://github.com/wandb/examples/blob/master/examples/tensorflow/tf-cnn-fashion/train.py)：RawTensorFlowを使用したファッションMNISTの例
* [Wandbダッシュボード](https://wandb.ai/l2k2/examples-tf-estimator-mnist/runs/p0ifowcb)：W＆Bで結果を表示
*  TensorFlow2でのトレーニングループのカスタマイズ―[記事](https://www.wandb.com/articles/wandb-customizing-training-loops-in-tensorflow-2) \| [Colabノートブック](https://colab.research.google.com/drive/1JCpAbjkCFhYMT7LCQ399y35TS3jlMpvM) \| [ダッシュボード](https://wandb.ai/sayakpaul/custom_training_loops_tf)

