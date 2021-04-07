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

TensorBoardに記録されていない追加のカスタムメトリックをログに記録する必要がある場合は、コード`wandb.log({"custom": 0.8}`で`wandb.log`を呼び出すことができます。

Tensorboardを同期する場合、`wandb.log`でステップ引数を設定することは無効です。別のステップ数を設定する場合は、次のようなステップメトリックで、メトリックをログに記録できます。

`wandb.log({"custom": 0.8, "global_step"=global_step})`

### **TensorFlowフック**

ログに記録される内容をより細かく制御したい場合、wandbはTensorFlow推定器のフックも提供します。グラフ内のすべてのtf.summary値をログに記録します。

```python
import tensorflow as tf
import wandb

wandb.init(config=tf.FLAGS)

estimator.train(hooks=[wandb.tensorflow.WandbHook(steps_per_log=1000)])
```

### **マニュアルログ**

TensorFlowでメトリックをログに記録する最も簡単な方法は、TensorFlowロガーでtf.summaryをログに記録することです。

```python
import wandb

with tf.Session() as sess:
    # ...
    wandb.tensorflow.log(tf.summary.merge_all())
```

 TensorFlow 2において、私たちが推奨する、カスタムループを使用したモデルのトレーニング方法は、tf.GradientTapeの使用です。詳細は[こちら](https://www.tensorflow.org/tutorials/customization/custom_training_walkthrough)をご覧ください。wandbを組み込んでカスタムTensorFlowトレーニングループにメトリックを記録する場合は、このスニペットに従うことができます

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

 完全な例は[こちら](https://www.wandb.com/articles/wandb-customizing-training-loops-in-tensorflow-2)をご覧ください。

## **W＆BはTensorBoardとどう違うのですか？**

私たちは、実験トラッキングツールをより良くするために努力しています。共同創設者がW＆Bの取り組みを始めたとき、彼らはOpenAIで不満を持っていたTensorBoardユーザーへ、ツールを構築しようとしていました。以下は、当社が改善に力を入れた項目です。

1. **モデルの再現**：Weights＆Biasesは、実験や調査、それに後で行うモデルの再現にも適しています。メトリックだけでなく、ハイパーパラメータとコードのバージョンもキャプチャします。モデルのチェックポイントも保存でき、プロジェクトの再現性が高まります。
2.  **自動編成**：プロジェクトを共同作業者に引き渡したり、休暇を取る場合などに、W＆Bを使用すると、試したすべてのモデルを簡単に確認できます。以前の実験を再実行するのに時間を無駄にすることはありません。
3.  **高速で柔軟な統合**：5分でプロジェクトにW＆Bを追加できます。無料のオープンソースPythonパッケージをインストールし、コードに数行追加するだけで、モデルを実行するたびに、メトリックとレコードのすばらしいログ記録が得られます。
4. **持続的で一元化されたダッシュボード**：ローカルマシン、ラボクラスター、クラウド内のスポットインスタンスなど、どこでモデルをトレーニングしても、一元化された同じダッシュボードを提供します。異なるマシンからTensorBoardファイルをコピーしたり整理したりする時間は必要ありません。
5.  **強力なテーブル**：さまざまなモデルの結果を検索したり、フィルタリング・並べ替え・グループ化が可能。何千ものモデルバージョンを調べて、さまざまなタスクに最適なモデルを簡単に見つけられます。一方、TensorBoardは、大規模なプロジェクトでうまく機能するようには構築されていません。
6.  **コラボレーションのためのツール**：W＆Bを使用して、複雑な機械学習プロジェクトをまとめることができます。W＆Bへのリンク共有は簡単で、プライベートチーム全員が共有プロジェクトに結果を送信できます。また、レポートを介したコラボレーションもサポートしています。インタラクティブな視覚化を追加し、マークダウンで作業を説明します。これは、作業ログを保持したり、調査結果を上司と共有したり、調査結果をラボに提示したりできる優れた方法です

 [無料の個人アカウントを始めましょう→](http://app.wandb.ai/)[\[KinoTrans1\]](applewebdata://684FD5B4-F3EB-456A-A2DD-45EF7C3915FC#_msocom_1) 

## **例**

このインテグレーションの機能を確認するために、いくつかの例を作成しました。

* [Githubの例](https://github.com/wandb/examples/blob/master/examples/tensorflow/tf-estimator-mnist/mnist.py)：TensorFlow Estimatorsを使用したMNISTの例
*  [Githubの例](https://github.com/wandb/examples/blob/master/examples/tensorflow/tf-cnn-fashion/train.py)：Raw TensorFlowを使用したファッションMNISTの例
* [Wandbダッシュボード](https://wandb.ai/l2k2/examples-tf-estimator-mnist/runs/p0ifowcb)：W＆Bで結果を表示
*  TensorFlow2でのトレーニングループのカスタマイズ―[記事](https://www.wandb.com/articles/wandb-customizing-training-loops-in-tensorflow-2) \| [Colabノートブック](https://colab.research.google.com/drive/1JCpAbjkCFhYMT7LCQ399y35TS3jlMpvM) \| [ダッシュボード](https://wandb.ai/sayakpaul/custom_training_loops_tf)

