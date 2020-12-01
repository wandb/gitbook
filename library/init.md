---
description: モデルメトリックをログに記録するトレーニングループの前に、新しい実行を開始するたびにwandb.init（）を呼び出します
---

# wandb.init\(\)

`wandb.init（）`を呼び出すと、[**Run**](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MNTRmPsdM-fSSOyCL8w/v/japanese/ref/export-api/api#run)オブジェクトが返されます。wandb.runを呼び出してRunオブジェクトにアクセスすることもできます。

通常、トレーニングスクリプトの開始時に一度`wandb.init（）`を呼び出す必要があります。これにより、新しい実行が作成され、単一のバックグラウンドプロセスが起動して、データがクラウドに同期されます。マシンをオフラインで実行し、後でデータをアップロードする場合は、[オフラインモード](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MNTRmPsdM-fSSOyCL8w/v/japanese/self-hosted)を使用します。

wandb.init（）は、いくつかのキーワード引数を受け入れます。

### Optional Arguments

<table>
  <thead>
    <tr>
      <th style="text-align:left">Argument</th>
      <th style="text-align:left">Type</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">project</td>
      <td style="text-align:left">str</td>
      <td style="text-align:left">&#x3053;&#x306E;run&#x304C;&#x5C5E;&#x3059;&#x308B;&#x30D7;&#x30ED;&#x30B8;&#x30A7;&#x30AF;&#x30C8;&#x306E;&#x540D;&#x524D;</td>
    </tr>
    <tr>
      <td style="text-align:left">entity</td>
      <td style="text-align:left">str</td>
      <td style="text-align:left">&#x3053;&#x306E;run&#x3092;&#x6295;&#x7A3F;&#x3059;&#x308B;&#x30C1;&#x30FC;&#x30E0;&#xFF08;&#x30C7;&#x30D5;&#x30A9;&#x30EB;&#x30C8;&#xFF1A;&#x30E6;&#x30FC;&#x30B6;&#x30FC;&#x540D;&#x307E;&#x305F;&#x306F;&#x30C7;&#x30D5;&#x30A9;&#x30EB;&#x30C8;&#x306E;&#x30C1;&#x30FC;&#x30E0;&#xFF09;</td>
    </tr>
    <tr>
      <td style="text-align:left">save_code</td>
      <td style="text-align:left">bool</td>
      <td style="text-align:left">&#x30E1;&#x30A4;&#x30F3;&#x306E;Python&#x307E;&#x305F;&#x306F;&#x30CE;&#x30FC;&#x30C8;&#x30D6;&#x30C3;&#x30AF;&#x30D5;&#x30A1;&#x30A4;&#x30EB;&#x3092;wandb&#x306B;&#x4FDD;&#x5B58;&#x3057;&#x3066;&#x3001;&#x5DEE;&#x5206;&#x3092;&#x6709;&#x52B9;&#x306B;&#x3057;&#x307E;&#x3059;&#xFF08;&#x30C7;&#x30D5;&#x30A9;&#x30EB;&#x30C8;&#xFF1A;
        <a
        href="https://wandb.ai/settings">&#x8A2D;&#x5B9A;</a>&#x30DA;&#x30FC;&#x30B8;&#x304B;&#x3089;&#x7DE8;&#x96C6;&#x53EF;&#x80FD;&#xFF09;</td>
    </tr>
    <tr>
      <td style="text-align:left">group</td>
      <td style="text-align:left">str</td>
      <td style="text-align:left">&#x4ED6;&#x306E;run&#x3092;&#x30B0;&#x30EB;&#x30FC;&#x30D7;&#x5316;&#x3059;&#x308B;&#x305F;&#x3081;&#x306E;&#x6587;&#x5B57;&#x5217;&#x3002;
        <a
        href="https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MNTRmPsdM-fSSOyCL8w/v/japanese/library/grouping">&#x30B0;&#x30EB;&#x30FC;&#x30D7;&#x5316;</a>&#x3092;&#x53C2;&#x7167;&#x3057;&#x3066;&#x304F;&#x3060;&#x3055;&#x3044;</td>
    </tr>
    <tr>
      <td style="text-align:left">job_type</td>
      <td style="text-align:left">str</td>
      <td style="text-align:left">&#x30ED;&#x30B0;&#x306B;&#x8A18;&#x9332;&#x3057;&#x3066;&#x3044;&#x308B;&#x30B8;&#x30E7;&#x30D6;&#x306E;&#x30BF;&#x30A4;&#x30D7;&#x3002;&#x4F8B;&#xFF1A;eval&#x3001;worker&#x3001;ps&#xFF08;&#x30C7;&#x30D5;&#x30A9;&#x30EB;&#x30C8;&#xFF1A;&#x30C8;&#x30EC;&#x30FC;&#x30CB;&#x30F3;&#x30B0;&#xFF09;</td>
    </tr>
    <tr>
      <td style="text-align:left">name</td>
      <td style="text-align:left">str</td>
      <td style="text-align:left">&#x3053;&#x306E;run&#x306E;&#x8868;&#x793A;&#x540D;&#x3002;UI&#x306B;&#x8868;&#x793A;&#x3055;&#x308C;&#x3001;&#x7DE8;&#x96C6;&#x53EF;&#x80FD;&#x3067;&#x3042;&#x308A;&#x3001;&#x7279;&#x5225;&#x3067;&#x3042;&#x308B;&#x5FC5;&#x8981;&#x306F;&#x3042;&#x308A;&#x307E;&#x305B;&#x3093;&#x3002;</td>
    </tr>
    <tr>
      <td style="text-align:left">notes</td>
      <td style="text-align:left">str</td>
      <td style="text-align:left">run&#x306B;&#x95A2;&#x9023;&#x4ED8;&#x3051;&#x3089;&#x308C;&#x305F;&#x8907;&#x6570;&#x884C;&#x306E;&#x6587;&#x5B57;&#x5217;&#x306E;&#x8AAC;&#x660E;</td>
    </tr>
    <tr>
      <td style="text-align:left">config</td>
      <td style="text-align:left">dict</td>
      <td style="text-align:left">&#x521D;&#x671F;&#x69CB;&#x6210;&#x3068;&#x3057;&#x3066;&#x8A2D;&#x5B9A;&#x3059;&#x308B;&#x8F9E;&#x66F8;&#x306E;&#x3088;&#x3046;&#x306A;&#x30AA;&#x30D6;&#x30B8;&#x30A7;&#x30AF;&#x30C8;</td>
    </tr>
    <tr>
      <td style="text-align:left">tags</td>
      <td style="text-align:left">str[]</td>
      <td style="text-align:left">&#x3053;&#x306E;run&#x306B;&#x30BF;&#x30B0;&#x3068;&#x3057;&#x3066;&#x95A2;&#x9023;&#x4ED8;&#x3051;&#x308B;&#x6587;&#x5B57;&#x5217;&#x306E;&#x30EA;&#x30B9;&#x30C8;</td>
    </tr>
    <tr>
      <td style="text-align:left">dir</td>
      <td style="text-align:left">path</td>
      <td style="text-align:left">&#x30A2;&#x30FC;&#x30C6;&#x30A3;&#x30D5;&#x30A1;&#x30AF;&#x30C8;&#x304C;&#x66F8;&#x304D;&#x8FBC;&#x307E;&#x308C;&#x308B;&#x30C7;&#x30A3;&#x30EC;&#x30AF;&#x30C8;&#x30EA;&#x3078;&#x306E;&#x30D1;&#x30B9;&#xFF08;&#x30C7;&#x30D5;&#x30A9;&#x30EB;&#x30C8;&#xFF1A;/wandb&#xFF09;</td>
    </tr>
    <tr>
      <td style="text-align:left">sync_tensorboard</td>
      <td style="text-align:left">bool</td>
      <td style="text-align:left">&#x3059;&#x3079;&#x3066;&#x306E;TensorBoard&#x30ED;&#x30B0;wandb&#x3092;&#x30B3;&#x30D4;&#x30FC;&#x3059;&#x308B;&#x304B;&#x3069;&#x3046;&#x304B;&#x3092;&#x793A;&#x3059;&#x30D6;&#x30FC;&#x30EB;&#x5024;&#x3002;Tensorboard&#x3092;&#x53C2;&#x7167;&#x3057;&#x3066;&#x304F;&#x3060;&#x3055;&#x3044;&#xFF08;&#x30C7;&#x30D5;&#x30A9;&#x30EB;&#x30C8;&#xFF1A;False&#xFF09;</td>
    </tr>
    <tr>
      <td style="text-align:left">resume</td>
      <td style="text-align:left">bool or str</td>
      <td style="text-align:left">True&#x306B;&#x8A2D;&#x5B9A;&#x3059;&#x308B;&#x3068;&#x3001;&#x81EA;&#x52D5;&#x5B9F;&#x884C;&#x304C;&#x518D;&#x958B;&#x3055;&#x308C;&#x307E;&#x3059;&#x3002;&#x624B;&#x52D5;&#x3067;&#x518D;&#x958B;&#x3059;&#x308B;&#x305F;&#x3081;&#x306E;&#x30E6;&#x30CB;&#x30FC;&#x30AF;&#x306A;&#x6587;&#x5B57;&#x5217;&#x306B;&#x3059;&#x308B;&#x3053;&#x3068;&#x3082;&#x3067;&#x304D;&#x307E;&#x3059;&#x3002;&#x518D;&#x958B;&#x3092;&#x53C2;&#x7167;&#x3057;&#x3066;&#x304F;&#x3060;&#x3055;&#x3044;&#xFF08;&#x30C7;&#x30D5;&#x30A9;&#x30EB;&#x30C8;&#xFF1A;False&#xFF09;</td>
    </tr>
    <tr>
      <td style="text-align:left">reinit</td>
      <td style="text-align:left">bool</td>
      <td style="text-align:left">&#x540C;&#x3058;&#x30D7;&#x30ED;&#x30BB;&#x30B9;&#x3067;wandb.init&#x3078;&#x306E;&#x8907;&#x6570;&#x306E;&#x547C;&#x3073;&#x51FA;&#x3057;&#x3092;&#x8A31;&#x53EF;&#x3059;&#x308B;&#x304B;&#x3069;&#x3046;&#x304B;&#xFF08;&#x30C7;&#x30D5;&#x30A9;&#x30EB;&#x30C8;&#xFF1A;False&#xFF09;</td>
    </tr>
    <tr>
      <td style="text-align:left">anonymous</td>
      <td style="text-align:left">&quot;allow&quot; &quot;never&quot; &quot;must&quot;</td>
      <td style="text-align:left">&#x8A31;&#x53EF;&#x300D;&#x3001;&#x300C;&#x6C7A;&#x3057;&#x3066;&#x300D;&#x3001;&#x307E;&#x305F;&#x306F;&#x300C;&#x5FC5;&#x9808;&#x300D;&#x306B;&#x3059;&#x308B;&#x3053;&#x3068;&#x304C;&#x3067;&#x304D;&#x307E;&#x3059;&#x3002;&#x3053;&#x308C;&#x306B;&#x3088;&#x308A;&#x3001;&#x533F;&#x540D;&#x30ED;&#x30AE;&#x30F3;&#x30B0;&#x304C;&#x6709;&#x52B9;&#x307E;&#x305F;&#x306F;&#x660E;&#x793A;&#x7684;&#x306B;&#x7121;&#x52B9;&#x306B;&#x306A;&#x308A;&#x307E;&#x3059;&#x3002;&#xFF08;&#x30C7;&#x30D5;&#x30A9;&#x30EB;&#x30C8;&#xFF1A;&#x306A;&#x3057;&#xFF09;</td>
    </tr>
    <tr>
      <td style="text-align:left">force</td>
      <td style="text-align:left">bool</td>
      <td style="text-align:left">&#x30B9;&#x30AF;&#x30EA;&#x30D7;&#x30C8;&#x306E;&#x5B9F;&#x884C;&#x6642;&#x306B;&#x30E6;&#x30FC;&#x30B6;&#x30FC;&#x3092;&#x5F37;&#x5236;&#x7684;&#x306B;wandb&#x306B;&#x30ED;&#x30B0;&#x30A4;&#x30F3;&#x3055;&#x305B;&#x308B;&#x304B;&#x3069;&#x3046;&#x304B;&#xFF08;&#x30C7;&#x30D5;&#x30A9;&#x30EB;&#x30C8;&#xFF1A;False&#xFF09;</td>
    </tr>
    <tr>
      <td style="text-align:left">magic</td>
      <td style="text-align:left">bool</td>
      <td style="text-align:left">bool&#x3001;dict&#x3001;&#x307E;&#x305F;&#x306F;str&#x3001;optional&#xFF09;&#xFF1A;bool&#x3001;dict&#x3001;json
        string&#x3001;yaml&#x30D5;&#x30A1;&#x30A4;&#x30EB;&#x540D;&#x3068;&#x3057;&#x3066;&#x306E;&#x9B54;&#x6CD5;&#x306E;&#x69CB;&#x6210;&#x3002;True&#x306B;&#x8A2D;&#x5B9A;&#x3059;&#x308B;&#x3068;&#x3001;&#x30B9;&#x30AF;&#x30EA;&#x30D7;&#x30C8;&#x306E;&#x81EA;&#x52D5;&#x8A08;&#x6E2C;&#x304C;&#x8A66;&#x884C;&#x3055;&#x308C;&#x307E;&#x3059;&#x3002;&#xFF08;&#x30C7;&#x30D5;&#x30A9;&#x30EB;&#x30C8;&#xFF1A;&#x306A;&#x3057;&#xFF09;</td>
    </tr>
    <tr>
      <td style="text-align:left">id</td>
      <td style="text-align:left">unique str</td>
      <td style="text-align:left">&#x4E3B;&#x306B;<a href="https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MNTRmPsdM-fSSOyCL8w/v/japanese/library/resuming">&#x518D;&#x958B;</a>&#x306B;&#x4F7F;&#x7528;&#x3055;&#x308C;&#x308B;&#x3053;&#x306E;run&#x306E;&#x30E6;&#x30CB;&#x30FC;&#x30AF;&#x306A;ID&#x3002;&#x30B0;&#x30ED;&#x30FC;&#x30D0;&#x30EB;&#x306B;&#x30E6;&#x30CB;&#x30FC;&#x30AF;&#x3067;&#x3042;&#x308B;<b>&#x5FC5;&#x8981;&#x304C;&#x3042;&#x308A;</b>&#x3001;run&#x3092;&#x524A;&#x9664;&#x3059;&#x308B;&#x3068;ID&#x3092;&#x518D;&#x5229;&#x7528;&#x3067;&#x304D;&#x307E;&#x305B;&#x3093;&#x3002;run&#x306E;&#x8AAC;&#x660E;&#x7684;&#x3067;&#x6709;&#x7528;&#x306A;&#x540D;&#x524D;&#x306B;&#x306F;&#x3001;<b>&#x540D;&#x524D;</b>&#x30D5;&#x30A3;&#x30FC;&#x30EB;&#x30C9;&#x3092;&#x4F7F;&#x7528;&#x3057;&#x307E;&#x3059;&#x3002;ID&#x306B;&#x7279;&#x6B8A;&#x6587;&#x5B57;&#x3092;&#x542B;&#x3081;&#x3066;&#x306F;&#x3044;&#x3051;&#x307E;&#x305B;&#x3093;&#x3002;</td>
    </tr>
    <tr>
      <td style="text-align:left">monitor_gym</td>
      <td style="text-align:left">bool</td>
      <td style="text-align:left">OpenAI Gym&#x306B;&#x3088;&#x3063;&#x3066;&#x751F;&#x6210;&#x3055;&#x308C;&#x305F;&#x30D3;&#x30C7;&#x30AA;&#x3092;&#x30ED;&#x30B0;&#x306B;&#x8A18;&#x9332;&#x3059;&#x308B;&#x304B;&#x3069;&#x3046;&#x304B;&#x3092;&#x793A;&#x3059;&#x30D6;&#x30FC;&#x30EB;&#x5024;&#x3002;
        <a
        href="../integrations/ray-tune.md">Ray Tune</a>&#x3092;&#x53C2;&#x7167;&#x3057;&#x3066;&#x304F;&#x3060;&#x3055;&#x3044;&#xFF08;&#x30C7;&#x30D5;&#x30A9;&#x30EB;&#x30C8;&#xFF1A;False&#xFF09;</td>
    </tr>
    <tr>
      <td style="text-align:left">allow_val_change</td>
      <td style="text-align:left">bool</td>
      <td style="text-align:left">
        <p>wandb.<a href="config.md">config</a>&#x5024;&#x306E;&#x5909;&#x66F4;&#x3092;&#x8A31;&#x53EF;&#x3059;&#x308B;&#x304B;&#x3069;&#x3046;&#x304B;&#x3002;&#x30C7;&#x30D5;&#x30A9;&#x30EB;&#x30C8;&#x3067;&#x306F;&#x3001;&#x69CB;&#x6210;&#x5024;&#x304C;&#x4E0A;&#x66F8;&#x304D;&#x3055;&#x308C;&#x308B;&#x3068;&#x4F8B;&#x5916;&#x304C;&#x30B9;&#x30ED;&#x30FC;&#x3055;&#x308C;&#x307E;&#x3059;&#x3002;&#xFF08;&#x30C7;&#x30D5;&#x30A9;&#x30EB;&#x30C8;&#xFF1A;False&#xFF09;</p>
        <p>&#x3053;&#x308C;&#x3089;&#x306E;&#x8A2D;&#x5B9A;&#x306E;&#x307B;&#x3068;&#x3093;&#x3069;&#x306F;&#x3001;&#x74B0;&#x5883;&#x5909;&#x6570;&#x3092;&#x4ECB;&#x3057;&#x3066;&#x5236;&#x5FA1;&#x3059;&#x308B;&#x3053;&#x3068;&#x3082;&#x3067;&#x304D;&#x307E;&#x3059;&#x3002;&#x3053;&#x308C;&#x306F;&#x3001;&#x30AF;&#x30E9;&#x30B9;&#x30BF;&#x30FC;&#x3067;&#x30B8;&#x30E7;&#x30D6;&#x3092;&#x5B9F;&#x884C;&#x3057;&#x3066;&#x3044;&#x308B;&#x3068;&#x304D;&#x306B;&#x3088;&#x304F;&#x5F79;&#x7ACB;&#x3061;&#x307E;&#x3059;&#x3002;</p>
      </td>
    </tr>
  </tbody>
</table>

 wandb.init（）を実行するスクリプトのコピーが自動的に保存されます。コード比較機能の詳細については、[コード比較](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MNTRmPsdM-fSSOyCL8w/v/japanese/app/features/panels/code)機能をご覧ください。この機能を無効にするには、環境変数WANDB\_DISABLE\_CODE=trueを設定してください。

## **よくある質問**

**1つのスクリプトから複数のrunを起動するにはどうすればよいですか？**

1つのスクリプトから複数のrunを開始しようとしている場合は、コードに次の2つを追加します。

1. run = wandb.init（**reinit=True**）：この設定を使用して、runの再初期化を許可します
2. **run.finish\(\)**：runの最後にこれを使用して、そのrunのログ記録を終了します

```python
import wandb
for x in range(10):
    run = wandb.init(project="runs-from-for-loop", reinit=True)
    for y in range (100):
        wandb.log({"metric": x+y})
    run.finish()
```

または、自動的にロギングを終了するPythonコンテキストマネージャーを使用することもできます。

```python
import wandb
for x in range(10):
    run = wandb.init(reinit=True)
    with run:
        for y in range(100):
            run.log({"metric": x+y})
```

### **LaunchError：アクセスが拒否されました**

**LaunchError：Launch exception：Permission denied**エラーが発生した場合、runを送信しようとしているプロジェクトにログを記録するためのアクセス許可がありません。これにはいくつかの様々な理由が考えられます。

1.  このマシンにログインしていません。コマンドラインでwandbloginを実行します。
2.  存在しないエンティティを設定しました。「エンティティ」は、ユーザー名または既存のチームの名前である必要があります。チームを作成する必要がある場合は、[サブスクリプションページ](https://app.wandb.ai/billing)にアクセスしてください。
3. プロジェクトの権限がありません。プロジェクトの作成者に、プライバシーを**Open**に設定して、このプロジェクトの実行をログに記録できるようにするよう依頼してください。

### **読み取り可能な実行名を取得します**

runに適した読みやすい名前を取得します。

```python
import wandb

wandb.init()
run_name = wandb.run.name
```

###  **実行名を生成されたrun IDに設定します**

実行名（snowy-owl-10など）をrun ID（qvlp96vkなど）で上書きする場合は、次のスニペットを使用できます。

```python
import wandb
wandb.init()
wandb.run.name = wandb.run.id
wandb.run.save()
```

### **git commitを保存します**

スクリプトでwandb.init（）が呼び出されると、git情報が自動的に検索され、最新のコミットのSHAのリポジトリへのリンクが保存されます。git情報が実行ページに表示されます。そこに表示されない場合は、wandb.init（）を呼び出すスクリプトがgit情報のあるフォルダーにあるかをご確認ください。

実験の実行に使用されるgitcommitとコマンドは表示されますが、外部ユーザーには表示されないため、パブリックプロジェクトがある場合、これらの詳細は公開されません。

###  **ログをオフラインで保存します**

デフォルトでは、wandb.init（）は、メトリックをクラウドでホストされているアプリにリアルタイムで同期するプロセスを開始します。マシンがオフラインであるか、またはインターネットにアクセスできない場合は、オフラインモードを使用してwandbを実行し、後で同期する方法を説明します。

2つの環境変数を設定します。

1. **WANDB\_MODE**: dryrun **WANDB\_API\_KEY**：[設定ページ](https://wandb.ai/settings)でこれをアカウントのAPIキーに設定します
2. **WANDB\_MODE**：dryrun

スクリプトでこれがどのように表示されるかのサンプルを次に示します。

```python
import wandb
import os

os.environ["WANDB_API_KEY"] = YOUR_KEY_HERE
os.environ["WANDB_MODE"] = "dryrun"

config = {
  "dataset": "CIFAR10",
  "machine": "offline cluster",
  "model": "CNN",
  "learning_rate": 0.01,
  "batch_size": 128,
}

wandb.init(project="offline-demo")

for i in range(100):
  wandb.log({"accuracy": i})
```

 ターミナル出力の例を次に示します。

![](../.gitbook/assets/image%20%2881%29.png)

インターネットがつながりますと、syncコマンドを実行してそのフォルダーをクラウドに送信します。

`wandb sync wandb/dryrun-folder-name`

![](../.gitbook/assets/image%20%2836%29.png)

