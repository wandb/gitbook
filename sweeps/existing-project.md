---
description: プロジェクトですでにwandb.init、wandb.config、およびwandb.logを使用している場合は、ここから始めてください。
---

# Sweep from an existing project

既存のW＆Bプロジェクトがある場合は、ハイパーパラメータスイープを使用してモデルの最適化を簡単に開始できます。実用的な例を使用して手順を説明します。[W＆Bダッシュボード](https://wandb.ai/carey/pytorch-cnn-fashion)を開くことができます。このサンプルリポジトリのコードを使用しています。このコードは、PyTorch畳み込みニューラルネットワークをトレーニングして、[Fashion MNIST ](https://github.com/zalandoresearch/fashion-mnist)データセットからの画像を分類します。

## 1. **プロジェクトの作成**

最初のベースライン実行を手動で実行して、W＆Bロギングが正しく機能していることを確認します。この単純なサンプルモデルをダウンロードし、数分間トレーニングすると、サンプルがWebダッシュボードに表示されます。

* このリポジトリの git clone [https://github.com/wandb/examples.gitのクローンを作成します](https://github.com/wandb/examples.gitのクローンを作成します)
* **•この例の\`cd examples/pytorch/pytorch-cnn-fashion\`を開きます**
* **手動で \`python train.py実行を実行します**

[プロジェクトページの例を見る→](https://app.wandb.ai/carey/pytorch-cnn-fashion)

## 2. **スイープの作成**

プロジェクトページから、サイドバーの\[スイープ\]タブを開き、\[スイープの作成\]をクリックします。

 自動生成された構成は、すでに実行した実行に基づいてスイープする値を推測します。構成を編集して、試行するハイパーパラメーターの範囲を指定します。スイープを起動すると、ホストされているW＆Bスイープサーバーで新しいプロセスが開始されます。この一元化されたサービスは、エージェント、つまりトレーニングジョブを実行しているマシンを調整します。

![](../.gitbook/assets/sweep2.png)

**3. エージェントの起動**次に、エージェントをローカルで起動します。作業を分散してスイープをより迅速に終了したい場合は、異なるマシンで数十の

![](../.gitbook/assets/sweep3.png)

 それで完了しました！これで、スイープを実行しています。スイープの例を開始すると、ダッシュボードは次のようになります。[プロジェクトページの例を見る→](https://wandb.ai/carey/pytorch-cnn-fashion?workspace=)

![](https://paper-attachments.dropbox.com/s_5D8914551A6C0AABCD5718091305DD3B64FFBA192205DD7B3C90EC93F4002090_1579066494222_image.png)

## **既存の実行で新しいスイープをシードします**以前に記録した既存の実行を使用して、新しいスイープを起動します。

1. プロジェクトテーブルを開きます。
2. テーブルの左側にあるチェックボックスで使用する実行を選択します。
3. ドロップダウンをクリックして、新しいスイープを作成します。これで、スイープがサーバーに設定されます。実行を開始するには、1つ以上のエージェントを起動するだけです。

![](../.gitbook/assets/create-sweep-from-table%20%281%29.png)

