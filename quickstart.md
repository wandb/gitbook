---
description: スクリプトを簡単に設定して、あなた独自のプロジェクトの実験記録や視覚化の機能について、見てみましょう。
---

# Quickstart

 3つの簡単なステップで機械学習実験のログを開始します。

## 1. **ライブラリのインストール** <a id="1-install-library"></a>

Python3の使用環境に当社のライブラリをインストールします。

```text
pip install wandb
```

GoogleのCloudMLなど、シェルコマンド実行に適さない自動環境でモデルをトレーニングしている場合は、[自動環境での実行](https://docs.wandb.com/advanced/automated)に関するドキュメントをご参照ください。  


## 2. **アカウントの作成** <a id="2-create-account"></a>

あなたの[シェルで無料アカウント](https://wandb.ai/login?signup=true)にサインアップするか、または当社のサインアップページにアクセスしてください。

```text
wandb login
```

## **3. トレーニングスクリプトの変更** <a id="3-modify-your-training-script"></a>

スクリプトに数行追加して、ハイパーパラメータとメトリックをログに記録します。

Weights＆Biasesはフレームワークに依存していませんが、一般的なMLフレームワークを使用している場合は、フレームワーク固有の例が入門には比較的に簡単な場合があります。当社は[Keras](file:////integrations/keras)、[TensorFlow](file:////integrations/tensorflow)、[PyTorch](file:////integrations/pytorch)、[Fast.ai](file:////integrations/fastai)、[Scikit](file:////integrations/scikit)、[XGBoost](file:////integrations/xgboost)、[Catalyst](file:////integrations/catalyst)の統合を簡素化するために、フレームワーク固有のフックを構築しました。

### **W&Bの初期化** <a id="initialize-w-and-b"></a>

ログ開始前に、スクリプトの開始時に`wandb`を初期化します。 [Hugging Face](file:////integrations/huggingface)統合などの一部の統合には、内部にwandb.init\(\)が含まれています。

```text
# Inside my model training codeimport wandbwandb.init(project="my-project")
```

プロジェクトが存在しない場合は、自動的に作成されます。上記のトレーニングスクリプトを実行すると、「my-project」という名前のプロジェクトに同期されます。その他の初期化オプションについては、[wandb.init](https://docs.wandb.ai/v/japanese/library/init) のドキュメントをご参照ください。

### **ハイパーパラメータの宣言** <a id="declare-hyperparameters"></a>

​[wandb.config](https://docs.wandb.ai/v/japanese/library/config) オブジェクトを使用してハイパーパラメータを保存するのは簡単です。

```text
wandb.config.dropout = 0.2wandb.config.hidden_layer_size = 128
```

### **メトリックのログ** <a id="log-metrics"></a>

モデルのトレーニング中に損失や精度などのメトリックをログに記録します（多くの場合、フレームワーク固有のデフォルトが提供されます）。 [wandb.log](https://docs.wandb.ai/v/japanese/library/log)を使用して、ヒストグラム、グラフ、画像などのより複雑な出力または結果をログに記録します。

```text
def my_train_loop():    for epoch in range(10):        loss = 0 # change as appropriate :)        wandb.log({'epoch': epoch, 'loss': loss})
```

### **ファイルの保存** <a id="save-files"></a>

`wandb.run.dir`ディレクトリに保存されたものは全部W＆Bにアップロードされ、完了時にあなたの実行とともに保存されます。これは、モデルの重みとバイアスをそのまま保存するのに特に便利です。

```text
# by default, this will save to a new subfolder for files associated# with your run, created in wandb.run.dir (which is ./wandb by default)wandb.save("mymodel.h5")​# you can pass the full path to the Keras model APImodel.save(os.path.join(wandb.run.dir, "mymodel.h5"))
```

すばらしいですね！それでは、スクリプトを通常どおり実行しましょう。すると、バックグラウンドプロセスでログが同期されます。gitリポジトリから実行している場合は、ターミナル出力、メトリック、およびファイルが、git状態のレコードとともにクラウドに同期されます。

テストする中でwandb同期を無効にしたい場合は、[環境変数](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MNTAj1Pg4WBXiUUFUpS/v/japanese/library/environment-variables)WANDB\_MODE=dryrunを設定してください。

## **次のステップ** <a id="next-steps"></a>



これで機能するようになりました。次は、素敵な機能についての簡単な概要です。

1. **プロジェクトページ**：プロジェクトダッシュボードでさまざまな実験を比較します。プロジェクトでモデルを実行する度に、グラフとテーブルに新しい線が表示されます。左側のサイドバーにあるテーブルアイコンをクリックしてテーブルを展開し、すべてのハイパーパラメータとメトリックを表示します。複数のプロジェクトを作成して実行を整理したり、テーブルで実行内容にタグやメモを追加できます。
2. **カスタムビジュアライゼーション**：平行座標チャート、散布図、およびその他の高度なビジュアライゼーションを追加して、結果を調査します。
3. [**レポート**](file:////reports)：マークダウンパネルを追加して、ライブグラフや表と一緒に調査結果を説明できます。このレポート機能で、プロジェクトのスナップショットを共同編集者、教授、または上司と簡単に共有できます。
4. [**インテグレーション**](file:////integrations)：PyTorch、Keras、XGBoostなどの一般的なフレームワークに特別に統合できます。
5. **ショーケース**：あなたの研究を共有することにご関心をお持ちですか？当社は、コミュニティのすばらしい成果に注目を集めるために、常にブログ投稿に取り組んでいます。[contact@wandb.com](mailto:contact@wandb.com)までご連絡ください。

### ​ **​**[**ご不明な点がございましたらお問い合わせください →**](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MNTAj1Pg4WBXiUUFUpS/v/japanese/company/getting-help)**​**​ <a id="contact-us-with-questions"></a>

### ​[**OpenAIのケーススタディをご覧ください →**](https://bit.ly/wandb-learning-dexterity)**​**​[ ](https://docs.wandb.ai/) <a id="see-the-openai-case-study"></a>

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-MAedsmfoXhB-i_THY73%2F-MAeeMymG3voCEOvuTq-%2Fimage.png?alt=media&token=9869cd6e-2485-455a-908b-0b9f7149135d)

