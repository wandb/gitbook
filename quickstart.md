---
description: スクリプトを簡単にインストルメント化して、独自のプロジェクトの実験追跡および視覚化機能を確認できます。
---

# Quickstart

3つの簡単なステップで機械学習実験のログを開始します。

## 1. **ライブラリのインストール**

Python3を使用する環境に当社のライブラリをインストールします。

```bash
pip install wandb
```

{% hint style="info" %}
 GoogleのCloudMLなど、シェルコマンドの実行が不便な自動環境でモデルをトレーニングしている場合は、[自動環境での実行](https://docs.wandb.com/advanced/automated)に関するドキュメントをご参照ください。
{% endhint %}

## 2.  **アカウントの作成**

 あなたの[シェルで無料アカウント](https://wandb.ai/login?signup=true)にサインアップするか、または当社のサインアップページにアクセスしてください。

```bash
wandb login
```

## 3. **トレーニングスクリプトの変更**

スクリプトに数行を追加して、ハイパーパラメータとメトリックをログに記録します。

{% hint style="info" %}
Weights＆Biasesはフレームワークに依存していません。もし、一般的なMLフレームワークを使用しているならば、フレームワーク固有の例が入門には比較的に簡単な場合があります。当社は [Keras](https://docs.wandb.com/frameworks/keras), [TensorFlow](https://docs.wandb.com/frameworks/tensorflow), [PyTorch](https://docs.wandb.com/frameworks/pytorch), [Fast.ai](https://docs.wandb.com/frameworks/fastai), [Scikit-learn](https://docs.wandb.com/frameworks/scikit), [XGBoost](https://docs.wandb.com/frameworks/xgboost), [Catalyst](https://docs.wandb.com/frameworks/catalyst), および[Jax](https://docs.wandb.com/frameworks/jax-example)の統合を簡素化するために、フレームワーク固有のフックを構築しています。
{% endhint %}

###  **Wandbの初期化**

インポート直後にスクリプトの最初で`wandb`を初期化します。

```python
# Inside my model training code
import wandb
wandb.init(project="my-project")
```

プロジェクトが存在しない場合は、自動的に作成されます。上記のトレーニングスクリプトを実行すると、「my-project」という名前のプロジェクトに同期されます。その他の初期化オプションについては、 [wandb.init](library/init.md) のドキュメントをご参照ください。

###  **ハイパーパラメータの宣言**

[wandb.config](library/config.md) オブジェクトを使用してハイパーパラメータを保存するのは簡単です。

```python
wandb.config.dropout = 0.2
wandb.config.hidden_layer_size = 128
```

### **メトリックのログ**

 モデルのトレーニング中に損失や精度などのメトリックをログに記録します（多くの場合、フレームワーク固有のデフォルトを提供します）。 [wandb.log](library/log.md)を使用して、ヒストグラム、グラフ、画像などのより複雑な出力または結果をログに記録します。

```python
def my_train_loop():
    for epoch in range(10):
        loss = 0 # change as appropriate :)
        wandb.log({'epoch': epoch, 'loss': loss})
```

###  **ファイルの保存**

`wandb.run.dir`ディレクトリに保存されたものは全部W＆Bにアップロードされ、完了するとあなたの実行とともに保存されます。これは、モデルの重みとバイアスをそのまま保存するのに特に便利です。

```python
# by default, this will save to a new subfolder for files associated
# with your run, created in wandb.run.dir (which is ./wandb by default)
wandb.save("mymodel.h5")

# you can pass the full path to the Keras model API
model.save(os.path.join(wandb.run.dir, "mymodel.h5"))
```

すごいです！スクリプトを通常どおり実行すると、バックグラウンドプロセスでログが同期されます。gitリポジトリから実行している場合は、ターミナル出力、メトリック、およびファイルが、git状態のレコードとともにクラウドに同期されます。

{% hint style="info" %}
 テストする中でwandb同期を無効にしたい場合は、[環境変数](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MNTAj1Pg4WBXiUUFUpS/v/japanese/library/environment-variables)WANDB\_MODE=dryrunを設定してください。
{% endhint %}

## **次のステップ**

これでインストルメンテーションが機能するようになりました。次は、クールな機能についての簡単な概要です。

1. 1.  **プロジェクトページ**：プロジェクトダッシュボードでさまざまな実験を比較します。プロジェクトでモデルを実行するたびに、グラフとテーブルに新しい線が表示されます。左側のサイドバーにあるテーブルアイコンをクリックしてテーブルを展開し、すべてのハイパーパラメータとメトリックを表示します。複数のプロジェクトを作成して実行を整理し、テーブルを使用して実行にタグとメモを追加します。
2.  **カスタムビジュアライゼーション**：平行座標チャート、散布図、およびその他の高度なビジュアライゼーションを追加して、結果を調査します。
3.  **レポート**：マークダウンパネルを追加して、ライブグラフや表と一緒に調査結果を説明します。レポートを使用すると、プロジェクトのスナップショットを共同編集者、教授、または上司と簡単に共有できます。
4. **フレームワーク**：当社には、PyTorch、Keras、XGBoostなどの一般的なフレームワーク用の特別な統合があります。
5.   **ショーケース**：あなたの研究を共有することに興味がありますか？当社は、コミュニティのすばらしい成果を強調するために、常にブログ投稿に取り組んでいます。[contact@wandb.com](mailto:contact@wandb.com)までご連絡ください。

### [**ご不明な点がございましたらお問い合わせください→**](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MNTAj1Pg4WBXiUUFUpS/v/japanese/company/getting-help)\*\*\*\*

###  [**OpenAIのケーススタディをご覧ください→**](https://bit.ly/wandb-learning-dexterity)\*\*\*\*

![](.gitbook/assets/image%20%2891%29.png)

