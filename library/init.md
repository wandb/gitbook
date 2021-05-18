---
description: スクリプトの最上部にwandb.init（）を呼び出して新しい実行を開始します
---

# wandb.init\(\)

新規ジョブの初期設定には、スクリプトの最初に`wandb.init（）`を呼び出します。これによってW&Bで新規実行が作成され、データを同期するためのバックグラウンドプロセスが開始されます。

* **オンプレミス**： プライベートクラウドまたはW＆Bのローカルインスタンスが必要な場合は、[セルフホスト](https://d.docs.live.net/self-hosted)をご覧ください。
*  **自動環境：** これらの設定のほとんどは、[環境変数](https://d.docs.live.net/library/environment-variables)を介して制御することもできます。 これは、クラスターでジョブを実行しているときによく役立ちます。

### 参照ドキュメント <a id="reference-docs"></a>

引数については、こちらの参照ドキュメントを確認してください。

## **よくある質問** <a id="common-questions"></a>

**1つのスクリプトから複数のrunを起動するにはどうすればよいですか？**

1つのスクリプトから複数のrunを開始しようとしている場合は、コードに次の2つを追加します。

1. run = wandb.init（**reinit=True**）：この設定を使用して、runの再初期化を許可します
2. **run.finish\(\)**：runの最後にこれを使用して、そのrunのログ記録を終了します

```text
import wandbfor x in range(10):    run = wandb.init(project="runs-from-for-loop", reinit=True)    for y in range (100):        wandb.log({"metric": x+y})    run.finish()
```

または、自動的にロギングを終了するPythonコンテキストマネージャーを使用することもできます。

```text
import wandbfor x in range(10):    run = wandb.init(reinit=True)    with run:        for y in range(100):            run.log({"metric": x+y})
```

### **LaunchError：アクセスが拒否されました**

**LaunchError：Launch exception：Permission denied**エラーが発生した場合、あなたはrunを送信しようとしているプロジェクトにログに記録する許可がありません。これにはいくつかの理由が考えられます。

1. このマシンにログインしていません。コマンドラインで**wandb login**を実行します。
2. 存在しないエンティティを設定しました。「エンティティ」は、ユーザー名または既存のチーム名である必要があります。チームを作成する必要がある場合は、[サブスクリプションページ](https://app.wandb.ai/billing)にアクセスしてください。
3. プロジェクトの権限がありません。プロジェクトの作成者に依頼してプライバシーを**Open**に設定してもらい、このプロジェクトの実行をログに記録できるようにしてください。

###  InitStartError: wandbプロセスとのコミュニケーションエラー <a id="init-start-error"></a>

**InitStartError：wandbプロセスとのコミュニケーションエラー**は、データをサーバーに同期するプロセスをライブラリが起動できないエラーです。

 以下の対応策は、特定の環境における問題解決に役立ちます。

```text
# Try this if using linux or macoswandb.init(settings=wandb.Settings(start_method="fork"))# Try this if using google colabwandb.init(settings=wandb.Settings(start_method="thread"))
```

###  マルチプロセッシングはまだサポートされていません <a id="multiprocess"></a>

トレーニングプログラムが複数のプロセスを使用する場合は、wandb.init（）を実行しなかったプロセスからwandbメソッド呼び出しを行わないようにプログラムを構造化する必要があります。

マルチプロセストレーニングを管理するには、いくつかのアプローチがあります。

1. すべてのプロセスに共通のグループを指定してwandb.init（）を呼び出します。各プロセスには独自のwandbが実行され、UIでトレーニングプロセスがまとめてグループ化されます。
2. 1プロセスのみからwandb.init（）を呼び出し、データを渡してマルチプロセッシングキューに記録します。

### **読み取り可能な実行名を取得します** <a id="get-the-readable-run-name"></a>

runに適した読みやすい名前を取得します。

```text
import wandb​wandb.init()run_name = wandb.run.name
```

### **実行名を生成されたrun IDにします** <a id="set-the-run-name-to-the-generated-run-id"></a>

 実行名（snowy-owl-10など）をrun ID（qvlp96vkなど）で上書きする場合は、次のスニペットを使用できます。

```text
import wandbwandb.init()wandb.run.name = wandb.run.idwandb.run.save()
```

### **git commitを保存します** <a id="save-the-git-commit"></a>

スクリプトでwandb.init（）が呼び出されると、git情報が自動的に検索され、最新のコミットのSHAをリポジトリへのリンクに保存するために、自動的にgit情報を検索します。git情報が[実行ページ](https://docs.wandb.ai/app/pages/run-page#overview-tab)に表示されます。そこに表示される設定にしない場合は、wandb.init（）を呼び出すスクリプトがgit情報のあるフォルダー中に入るようにしてください。

実験の実行に使用されるgitコミットとコマンドはあなたには表示されますが、外部ユーザーには表示されないため、もしパブリックプロジェクトがある場合、これらの詳細は公開されません。

### **ログをオフラインで保存します** <a id="save-logs-offline"></a>

デフォルトでは、wandb.init（）は、クラウドがホストのアプリにメトリックをリアルタイムに同期するプロセスを開始します。マシンがオフラインであるか、またはインターネットにアクセスできない場合は、オフラインモードでwandbを実行し、後で同期する方法を説明します。

2つの環境変数を設定します。

1. **WANDB\_API\_KEY**: [設定ページ](https://wandb.ai/settings)でこれをアカウントのAPIキーに設定します
2. **WANDB\_MODE**: dryrun

スクリプトでこれがどのように表示されるかのサンプルです。

```text
import wandbimport os​os.environ["WANDB_API_KEY"] = YOUR_KEY_HEREos.environ["WANDB_MODE"] = "dryrun"​config = {  "dataset": "CIFAR10",  "machine": "offline cluster",  "model": "CNN",  "learning_rate": 0.01,  "batch_size": 128,}​wandb.init(project="offline-demo")​for i in range(100):  wandb.log({"accuracy": i})
```

ターミナル出力の例を次に示します。

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-M4ZqIaDYRFSEiZrYTaI%2F-M4Zx9NGlicWWRF-Zcgh%2Fimage.png?alt=media&token=6f32064c-d58e-412e-8344-ed43baee721e)

インターネットにつながると、syncコマンドを実行してそのフォルダーをクラウドに送信します。

`wandb sync wandb/dryrun-folder-name`

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-M4ZqIaDYRFSEiZrYTaI%2F-M4ZxQU2WrG9S0MZzqDI%2Fimage.png?alt=media&token=0295541a-90bf-464f-8899-2f9a53c45e1c)

