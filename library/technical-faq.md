# Technical FAQ

### テクニカルFAQ

**これは私のトレーニングプロセスにどのように影響しますか？**トレーニングスクリプトから`wandb.init`（）が呼び出されると、API呼び出しが行われ、サーバー上に実行オブジェクトが作成されます。メトリックをストリーミングおよび収集するために新しいプロセスが開始され、それによってすべてのスレッドとロジックがプライマリプロセスから除外されます。スクリプトは正常に実行され、ローカルファイルに書き込みますが、別のプロセスがシステムメトリックとともにそれらをサーバーにストリーミングします。トレーニングディレクトリからwandbをオフにするか、**WANDB\_MODE**環境変数を「dryrun」に設定することで、いつでもストリーミングをオフにできます。

###  **wandbがクラッシュした場合、トレーニングの実行がクラッシュする可能性がありますか？**

私たちがあなたのトレーニングの実行を決して妨害しないことは私たちにとって非常に重要です。wandbを別のプロセスで実行して、wandbが何らかの理由でクラッシュした場合でも、トレーニングが引き続き実行されるようにします。インターネットが切断された場合、wandbは引き続きwandb.comへのデータの送信を再試行します。

### **wandbは私のトレーニングを遅くするようなことがありますか？**

Wandbを通常使用する場合、トレーニングパフォーマンスへの影響はごくわずかです。wandbの通常の使用は、各ステップで1秒に1回未満のログを記録し、数メガバイト未満のデータをログに記録することを意味します。Wandbは別のプロセスで実行され、関数呼び出しはブロックされないため、ネットワークが一時的にダウンしたり、ディスク上で断続的な読み取り/書き込みの問題が発生したりしても、パフォーマンスに影響はありません。大量のデータをすばやくログに記録することが可能であり、そうすると、ディスクI/Oの問題が発生する可能性があります。ご不明な点がございましたら、お気軽にお問い合わせください。

### **wandbをオフラインで実行できますか？**

当社は、オフラインマシンでトレーニングしていて、後で結果をサーバーにアップロードする機能を有しています。

1. 環境変数WANDB\_MODE=dryrunを設定して、メトリックをローカルに保存します。インターネットは必要ありません
2. 準備ができたら、ディレクトリでwandb initを実行して、プロジェクト名を設定します。wandb sync YOUR\_RUN\_DIRECTORYを実行して、メトリックをクラウドサービスにプッシュし、ホストされているWebアプリで結果を確認します。

### **ツールはトレーニングデータを追跡または保存していますか？**

SHAまたはその他の一意の識別子を`wandb.config.update(...)`に渡して、データセットをトレーニングの実行に関連付けることができます。`wandb.save`がローカルファイル名で呼び出されない限り、W＆Bはデータを保存しません。

### **システムメトリックはどのくらいの頻度で収集されますか？**

デフォルトでは、メトリックは2秒ごとに収集され、30秒間の平均になります。より高い解像度の指標が必要な場合は、[contact@wandb.com](mailto:contact@wandb.com)までEメールでお問い合わせください。

### **これはPythonでのみ機能しますか？**

現在、ライブラリはPython2.7以降および3.6以降のプロジェクトでのみ機能します。上記のアーキテクチャにより、他の言語と簡単に統合できるようになります。他の言語を表示する必要がある場合は、[contact@wandb.com](mailto:contact@wandb.com)までご連絡ください。

### **コードやデータセットの例ではなく、メトリックのみをログに記録できますか？**

データセットの例

デフォルトでは、データセットの例はログに記録されません。この機能を明示的にオンにすると、Webイ

ンターフェイスで予測例を確認できます。

**コードロギング**

コードロギングをオフにする方法は2つあります。

1. **WANDB\_DISABLE\_CODE**をtrueに設定して、すべてのコード追跡をオフにします。git SHAまたはdiffパッチは取得しません。
2.  **WANDB\_IGNORE\_GLOBS**を\* .patchに設定して、サーバーへのdiffパッチの同期をオフにします。 あなたはまだそれをローカルに持っていて、[wandbrestore](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MNTo635YwwyToLxk-CQ/v/japanese/library/cli#restore-the-state-of-your-code)コマンドでそれを適用することができます。

### **ロギングは私のトレーニングをブロックしますか？**

「ロギング機能は怠惰ですか？結果をサーバーに送信してからローカル操作を続行するためにネットワークに依存したくありません。」

wandb.logを呼び出すと、ローカルファイルに行が書き込まれます。ネットワーク呼び出しをブロックしません。wandb.initを呼び出すと、ファイルシステムの変更をリッスンし、トレーニングプロセスとは非同期にWebサービスと通信する新しいプロセスを同じマシンで起動します。

### **平滑化アルゴリズムにはどの式を使用しますか？**

TensorBoardと同じ指数移動平均式を使用します。次のリンクからその説明を見つけることができます。[https://stackoverflow.com/questions/42281844/what-is-the-mathematics-behind-the-smoothing-parameter-in-tensorboards-scalar](https://stackoverflow.com/questions/42281844/what-is-the-mathematics-behind-the-smoothing-parameter-in-tensorboards-scalar)

### **W＆BはTensorBoardとどう違いますか？**

私たちはTensorBoardに関わった人たちとの協力を望んでおり、TensorBoardとの統合を成し遂げています！私たちは、すべての人のための実験追跡ツールを改善するために努力しています。共同創設者がW＆Bに取り組み始めたとき、彼らはOpenAIで欲求不満のTensorBoardユーザーのためのツールを構築するように求められました。以下は、当社が改善する上で力を入れた項目です。

1. **モデルの再現**：Weights＆Biasesは、実験、調査、および後でモデルを再現するのに適しています。メトリックだけでなく、ハイパーパラメータとコードのバージョンもキャプチャし、プロジェクトの再現性を高めるためにモデルのチェックポイントを保存できます。
2. **自動編成**：プロジェクトを共同作業者に引き渡したり、休暇を取ったりした場合、W＆Bを使用すると、試したすべてのモデルを簡単に確認できるため、古い実験を再実行するのに時間を無駄にすることはありません。
3. **高速で柔軟な統合**：5分でプロジェクトにW＆Bを追加します。無料のオープンソースPythonパッケージをインストールし、コードに数行追加すると、モデルを実行するたびに、ログに記録された優れたメトリックとレコードが得られます。
4. **統一化および一元化されたダッシュボード**：ローカルマシン、ラボクラスター、クラウド内のスポットインスタンスなど、モデルをトレーニングする場所ならどこでも、同じ一元化されたダッシュボードを提供します。異なるマシンからTensorBoardファイルをコピーして整理するのに時間を費やす必要はありません。
5. **強力なテーブル**：さまざまなモデルの結果を検索、フィルタリング、並べ替え、グループ化します。何千ものモデルバージョンを調べて、さまざまなタスクに最適なモデルを見つけるのは簡単です。TensorBoardは、大規模なプロジェクトでうまく機能するようには構築されていません。
6. **コラボレーションのためのツール**：W＆Bを使用して、複雑な機械学習プロジェクトを整理します。 W＆Bへのリンクを共有するのは簡単で、プライベートチームを使用して、全員が共有プロジェクトに結果を送信することができます。また、レポートを介したコラボレーションもサポートしています。インタラクティブな視覚化を追加し、マークダウンで作業を説明します。これは、作業ログを保持したり、調査結果を上司と共有したり、調査結果をラボに提示したりするための優れた方法です。無料の個人アカウントを始めましょう→

### **トレーニングコードで実行の名前を構成するにはどうすればよいですか？**

 `wandb.init`を呼び出すときは、トレーニングスクリプトの先頭で、`wandb.init(name="my awesome run")`のような実験名を渡します。

### **スクリプトでランダムな実行名を取得するにはどうすればよいですか？**

`wandb.run.save（）`を呼び出してから、

`wandb.run.name`で名前を取得します。

### **anacondaパッケージはありますか？**

anacondaパッケージはありませんが、以下を使用してwandbをインストールできます。

```text
conda activate myenv
pip install wandb
```

 このインストールで問題が発生した場合は、お知らせください。この[パッケージ管理に関するAnacondaドキ](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-pkgs.html)ュメントには、助けになるガイダンスがあります。

###  **wandbがターミナルまたはjupyterノートブック出力に書き込むのを停止するにはどうすればよいですか？**

環境変数[WANDB\_SILENT](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MNTo635YwwyToLxk-CQ/v/japanese/library/environment-variables)を設定します。

ノートブックの場合：

```text
%env WANDB_SILENT true
```

Pythonスクリプトの場合：

```text
os.environ["WANDB_SILENT"] = "true"
```

### **wandbでジョブを強制終了するにはどうすればよいですか？**

キーボードのctrl+Dを押して、wandbでインストルメントされたスクリプトを停止します。

###  **ネットワークの問題に対処するにはどうすればよいですか？**

**SSL CERTIFICATE\_VERIFY\_FAILED:** this error could be due to your company's firewall. You can set up local CAs and then use: **ネットワークの問題に対処するにはどうすればよいですか？**SSLまたはネットワークで、`wandb: Network error (ConnectionError), entering retry loop`のようなエラーが発生している場合、この問題を解決するために、いくつかの異なるアプローチを試すことができます。

1. SSL証明書をアップグレードします。Ubuntuサーバーでスクリプトを実行している場合は、`update-ca-certificates`を実行します。セキュリティの脆弱性があるため、有効なSSL証明書がないとトレーニングログを同期できません。
2. ネットワークが不安定な場合は、[オフラインモード](https://docs.wandb.com/resources/technical-faq#can-i-run-wandb-offline)でトレーニングを実行し、インターネットにアクセスできるマシンからファイルを同期します。
3. [W＆B Local](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MNTo635YwwyToLxk-CQ/v/japanese/self-hosted/local)を実行してみてください。これは、マシン上で動作し、ファイルをクラウドサーバーに同期しません。**SSL CERTIFICATE\_VERIFY\_FAILED**：このエラーは、会社のファイアウォールが原因である可能性があります。ローカルCAを設定して、次を使用してください。

`export REQUESTS_CA_BUNDLE=/etc/ssl/certs/ca-certificates.crt`

### **モデルのトレーニング中にインターネット接続が失われた場合はどうなりますか？**

ライブラリがインターネットに接続できない場合、ライブラリは再試行ループに入り、ネットワークが復元されるまでメトリックのストリーミングを試み続けます。この間、プログラムは実行を継続できます。インターネットのないマシンで実行する必要がある場合は、`WANDB_MODE=dryrun`を設定して、メトリックをハードドライブにローカルにのみ保存することができます。後でwandb sync DIRECTORYを呼び出して、データをサーバーにストリーミングすることができます。

### **2つの異なる時間スケールでメトリックをログに記録できますか？**

**（たとえば、バッチごとのトレーニング精度とエポックごとの検証精度をログに記録したいと思います。）**はい、そうです。これを行うには、複数のメトリックをログに記録してから、それらにx軸の値を設定します。したがって、あるステップでは`wandb.log({'train_accuracy': 0.9, 'batch': 200})`を呼び出し、別のステップでは`wandb.log({'val_acuracy': 0.8, 'epoch': 4})`を呼び出すことができます。 

### **最終評価の精度など、時間の経過とともに変化しないメトリックをログに記録するにはどうすればよいですか？**

wandb.log（{'final\_accuracy': 0.9}）を使用すると、これで問題なく動作します。デフォルトでは、wandb.log（{'final\_accuracy'}）は、runsテーブルに表示される値であるwandb.settings\['final\_accuracy'\]を更新します。

### How can I log additional metrics after a run completes?

There are several ways to do this.

For complicated workflows, we recommend using multiple runs and setting group parameter in [wandb.init](init.md) to a unique value in all the processes that are run as part of a single experiment. The [runs table](../app/pages/run-page.md) will automatically group the table by the group ID and the visualizations will behave as expected. This will allow you to run multiple experiments and training runs as separate processes log all the results into a single place.

For simpler workflows, you can call wandb.init with resume=True and id=UNIQUE\_ID and then later call wandb.init with the same id=UNIQUE\_ID. Then you can log normally with [wandb.log](log.md) or wandb.summary and the runs values will update.

At any point you can always use the[ API](../ref/export-api/) to add additional evaluation metrics.

 **実行が完了した後、追加のメトリックをログに記録するにはどうすればよいですか？**

これはいくつかの方法によって行われます。複雑なワークフローの場合は、複数の実行を使用し、[wandb.init](init.md)のグループパラメータを、単一の実験の一部として実行されるすべてのプロセスでユニークな値に設定することをお勧めします。実行テーブルは、グループIDによってテーブルを自動的にグループ化し、ビジュアライゼーションは期待どおりに動作します。これにより、個別のプロセスがすべての結果を1つの場所に記録するため、複数の実験とトレーニングを実行できます。より単純なワークフローの場合、resume=Trueおよびid=UNIQUE\_IDを指定してwandb.initを呼び出し、後で同じid=UNIQUE\_IDを指定してwandb.initを呼び出すことができます。次に、wandb.logまたはwandb.summaryを使用して通常どおりログに記録すると、実行値が更新されます。APIを使用して、いつでも評価指標を追加できます。

### What is the difference between  .log\(\) and .summary?  

The summary is the value that shows in the table while log will save all the values for plotting later.  

For example you might want to call `wandb.log` every time the accuracy changes.   Usually you can just use .log.  `wandb.log()` will also update the summary value by default unless you have set summary manually for that metric

The scatterplot and parallel coordinate plots will also use the summary value while the line plot plots all of the values set by .log

The reason we have both is that some people like to set the summary manually because they want the summary to reflect for example the optimal accuracy instead of the last accuracy logged.

### How do I install the wandb Python library in environments without gcc?

If you try to install `wandb` and see this error:

```text
unable to execute 'gcc': No such file or directory
error: command 'gcc' failed with exit status 1
```

You can install psutil directly from a pre-built wheel. Find your Python version and OS here: [https://pywharf.github.io/pywharf-pkg-repo/psutil](https://pywharf.github.io/pywharf-pkg-repo/psutil)  

For example, to install psutil on python 3.8 in linux:

```text
pip install https://github.com/pywharf/pywharf-pkg-repo/releases/download/psutil-5.7.0-cp38-cp38-manylinux2010_x86_64.whl/psutil-5.7.0-cp38-cp38-manylinux2010_x86_64.whl#sha256=adc36dabdff0b9a4c84821ef5ce45848f30b8a01a1d5806316e068b5fd669c6d
```

After psutil has been installed, you can install wandb with `pip install wandb`

  


