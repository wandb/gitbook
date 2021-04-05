# TensorBoard

 W＆Bは、TensorBoardにパッチを適用して、スクリプトのすべての指標をネイティブチャートに自動的に記録することをサポートしています。

```python
import wandb
wandb.init(sync_tensorboard=True)
```

 TensorFlowのすべてのバージョンでTensorBoardをサポートしています。別のフレームワークでTensorBoardを使用している場合、W＆BはPyTorchおよびTensorBoardXでTensorBoard&gt; 1.14をサポートします。

### **カスタムメトリック**

TensorBoardに記録されていない追加のカスタムメトリックをログに記録する必要がある場合は、TensorBoardが使用しているのと同じステップ引数を使用してコードで`wandb.log`を呼び出すことができます。つまり、`wandb.log({"custom": 0.8}, step=global_step)`です。

###  **高度な構成**

TensorBoardのパッチ適用方法をより細かく制御したい場合は、`sync_tensorboard = True`をinitに渡す代わりに、`wandb.tensorboard.patch`を呼び出すことができます。 このメソッドに`tensorboardX = False`を渡して、バニラTensorBoardにパッチが適用されていることを確認できます。PyTorchでTensorBoard&gt; 1.14を使用している場合は、`pytorch = True`を渡してパッチが適用されていることを確認できます。 これらのオプションはどちらも、インポートされたライブラリのバージョンに応じて、スマートなデフォルトになっています。

デフォルトでは、tfeventsファイルと\*.pbtxtファイルも同期します。これにより、お客様に代わってTensorBoardインスタンスを起動できます。実行ページにTensorBoardタブが表示されます。この動作は、~~~~がsave = Falseを`wandb.tensorboard.patch`に渡すことで無効にできます。

```python
import wandb
wandb.init()
wandb.tensorboard.patch(save=False, tensorboardX=True)
```

### **以前のTensorBoard実行の同期**

wandbライブラリ統合を使用してすでに生成された既存のtfeventsファイルがローカルに保存されていて、それらをwandbにインポートする場合は、wandb sync log\_dirを実行できます。log\_dirは、tfeventsファイルを含むローカルディレクトリです。

`wandb syncdirectory_with_tf_event_file`を実行することもできます

```python
"""This script will import a directory of tfevents files into a single W&B run.
You must install wandb from a special branch until this feature is merged into the mainline: 
```bash
pip install --upgrade git+git://github.com/wandb/client.git@feature/import#egg=wandb
```

このスクリプトは、python `no_image_import.pydir_with_tf_event_file`を使用して呼び出すことができます。これにより、そのディレクトリ内のイベントファイルからのメトリックを使用してwandbで単一の実行が作成されます。これを多くのディレクトリで実行する場合は、実行ごとに1回だけこのスクリプトを実行する必要があるため、ローダーは次のようになります。

```python
import glob
for run_dir in glob.glob("logdir-*"):
  subprocess.Popen(["python", "no_image_import.py", run_dir], stderr=subprocess.PIPE, stdout=subprocess.PIPE)
```

""" import glob import os import wandb import sys import time import tensorflow as tf from wandb.tensorboard.watcher import Consumer, Event from six.moves import queue

if len\(sys.argv\) == 1: raise ValueError\("Must pass a directory as the first argument"\)

paths = glob.glob\(sys.argv\[1\]+"/_\*/_.tfevents.\*", recursive=True\) root = os.path.dirname\(os.path.commonprefix\(paths\)\).strip\("/"\) namespaces = {path: path.replace\(root, ""\).replace\( path.split\("/"\)\[-1\], ""\).strip\("/"\) for path in paths} finished = {namespace: False for path, namespace in namespaces.items\(\)} readers = \[\(namespaces\[path\], tf.train.summary\_iterator\(path\)\) for path in paths\] if len\(readers\) == 0: raise ValueError\("Couldn't find any event files in this directory"\)

directory = os.path.abspath\(sys.argv\[1\]\) print\("Loading directory %s" % directory\) wandb.init\(project="test-detection"\)

Q = queue.PriorityQueue\(\) print\("Parsing %i event files" % len\(readers\)\) con = Consumer\(Q, delay=5\) con.start\(\) total\_events = 0 while True:

```text
# Consume 500 events at a time from all readers and push them to the queue
for namespace, reader in readers:
    if not finished[namespace]:
        for i in range(500):
            try:
                event = next(reader)
                kind = event.value.WhichOneof("value")
                if kind != "image":
                    Q.put(Event(event, namespace=namespace))
                    total_events += 1
            except StopIteration:
                finished[namespace] = True
                print("Finished parsing %s event file" % namespace)
                break
if all(finished.values()):
    break
```

print\("Persisting %i events..." % total\_events\) con.shutdown\(\) print\("Import complete"\)

\`\`\`

###  **GoogleColabおよびTensorBoard**

Colabのコマンドラインからコマンドを実行するには、`！wandb syncdirectoryname`を実行する必要があります。現在、テンソルボードの同期は、Tensorflow2.1以降のノートブック環境では機能しません。Colabに以前のバージョンのTensorBoardを使用させるか、コマンドラインから！`pythonyour_script.py`を使用してスクリプトを実行することができます。

## **W＆BはTensorBoardとどう違うのですか？**

私たちは、すべての人のための実験追跡ツールを改善するように促されました。共同創設者がW＆Bに取り組み始めたとき、彼らはOpenAIで欲求不満のTensorBoardユーザーのためのツールを構築するように促されました。以下は、当社が改善する上で力を入れた項目です。

1. **モデルの再現**：Weights＆Biasesは、実験、調査、および後でモデルを再現するのに適しています。メトリックだけでなく、ハイパーパラメータとコードのバージョンもキャプチャし、プロジェクトの再現性を高めるためにモデルのチェックポイントを保存できます。
2. **自動編成**：プロジェクトを共同作業者に引き渡したり、休暇を取ったりした場合、W＆Bを使用すると、試したすべてのモデルを簡単に確認できるため、古い実験を再実行するのに時間を無駄にすることはありません。
3. **高速で柔軟な統合**：5分でプロジェクトにW＆Bを追加します。無料のオープンソースPythonパッケージをインストールし、コードに数行追加すると、モデルを実行するたびに、ログに記録された優れたメトリックとレコードが得られます。
4. **統一化および一元化されたダッシュボード**：ローカルマシン、ラボクラスター、クラウド内のスポットインスタンスなど、モデルをトレーニングする場所ならどこでも、同じ一元化されたダッシュボードを提供します。異なるマシンからTensorBoardファイルをコピーして整理するのに時間を費やす必要はありません。
5. **効果的な表**：さまざまなモデルの結果を検索、フィルタリング、並べ替え、グループ化します。何千ものモデルバージョンを調べて、さまざまなタスクに最適なモデルを見つけるのは簡単です。TensorBoardは、大規模なプロジェクトでうまく機能するようには構築されていません。
6. **コラボレーションのためのツール**：W＆Bを使用して、複雑な機械学習プロジェクトを整理します。 W＆Bへのリンクを共有するのは簡単で、プライベートチームを使用して、全員が共有プロジェクトに結果を送信することができます。また、レポートを介したコラボレーションもサポートしています。インタラクティブな視覚化を追加し、マークダウンで作業を説明します。これは、作業ログを保持したり、調査結果を上司と共有したり、調査結果をラボに提示したりするための優れた方法です。

   | ·     |
   | :--- |



