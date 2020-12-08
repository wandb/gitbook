# Jupyter

 JupyterノートブックでWeights＆Biasesを使用して、インタラクティブな視覚化を取得し、トレーニングの実行時にカスタム分析を行います。

### **Jupyterノートブックを使用したW＆Bのユースケース**

1. **反復実験**：実験を実行および再実行し、パラメータを微調整し、途中で手動でメモをとることなく、実行したすべての実行をW＆Bに自動的に保存します。
2. **コードの保存**：モデルを再現する場合、ノートブックのどのセルがどの順序で実行されたかを知るのは困難です。[設定ページ](https://wandb.ai/settings)でコード保存をオンにして、各実験のセル実行の記録を保存します。
3. **カスタム分析**：実行がW＆Bに記録されると、APIからデータフレームを取得してカスタム分析を実行し、それらの結果をW＆Bに記録して、レポートに保存および共有するのは簡単です。

##  **ノートブックの構成**

 次のコードでノートブックを起動して、W＆Bをインストールし、アカウントをリンクします。

```python
!pip install wandb -qqq
import wandb
wandb.login()
```

次に、実験を設定し、ハイパーパラメータを保存します。

```python
wandb.init(project="jupyter-projo",
           config={
               "batch_size": 128,
               "learning_rate": 0.01,
               "dataset": "CIFAR-100",
           })
```

 `wandb.init()`を実行した後、`%%wandb`で新しいセルを開始して、ノートブックにライブグラフを表示します。このセルを複数回実行すると、データが実行に追加されます

```python
%%wandb

# Your training loop here
```

この[簡単なサンプルスクリプト](https://colab.research.google.com/drive/1XarrwLYCGmMUGBSe7eymzofIfSwytuKC?usp=sharing)で実際に試してみてください→

![](../.gitbook/assets/jupyter-widget.png)

`%%wandb`デコレータの代わりに、`wandb.init()`の後に線を追加して、インライングラフを表示できます

```python
# Initialize wandb.run first
wandb.init()

# If cell outputs wandb.run, you'll see live graphs
wandb.run
```

### **W＆Bの追加のJupyter機能**

1. **Colab**：Colabで初めて`wandb.init（）`を呼び出すと、ブラウザーで現在W＆Bにログインしている場合、ランタイムが自動的に認証されます。実行ページの\[概要\]タブに、Colabへのリンクが表示されます。[設定](https://wandb.ai/settings)でコード保存をオンにすると、実験を実行するために実行されたセルも表示され、再現性が向上します。
2. **Docker Jupyterの起動**：`wandb docker --jupyter`を呼び出してdockerコンテナーを起動し、コードをマウントして、Jupyterがインストールされていることを確認し、ポート8888で起動します。
3. **run.finish\(\)**: デフォルトでは、次にwandb.init（）が呼び出されて、実行が終了したことを示すまで待機します。これにより、個々のセルを実行し、それらすべてを同じ実行に記録させることができます。Jupyterノートブックで手動で実行を完了としてマークするには、**run.finish（）**機能を使用します。

```python
import wandb
run = wandb.init()
# Training script and logging goes here
run.finish()
```

### **W＆B情報メッセージの無効化**

情報メッセージを無効にするには、ノートブックセルで以下を実行します。

```python
import logging
logger = logging.getLogger("wandb")
logger.setLevel(logging.ERROR)
```

### **よくある質問**

#### **ノートブック名**

「ノートブック名のクエリに失敗しました。WANDB\_NOTEBOOK\_NAME環境変数を使用して手動で設定できます」というエラーメッセージが表示された場合は、次のようにスクリプトから環境変数を設定することでこれを解決できます。os.environ\['WANDB\_NOTEBOOK\_NAME'\] = 'some text here'

