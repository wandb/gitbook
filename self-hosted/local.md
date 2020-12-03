---
description: Dockerを使用して自分の機器でWeights and Biasesを実行します
---

# Local

##  **サーバーの開始**

 W＆Bサーバーをローカルで実行するには、[Docker](https://www.docker.com/products/docker-desktop)をインストールしてください。そうすると、簡単に実行されます。

```text
wandb local
```

裏では、wandbクライアントライブラリが[_wandb/local_](https://hub.docker.com/repository/docker/wandb/local) dockerイメージを実行し、ポート8080をホストに転送し、ホストされたクラウドではなくローカルインスタンスにメトリックを送信するように機器を設定しています。ローカルコンテナを手動で実行する場合は、次のdockerコマンドを実行できます。

```text
docker run --rm -d -v wandb:/vol -p 8080:8080 --name wandb-local wandb/local
```

### Centralized Hosting

一元化されたホスティングローカルホストでwandbを実行することは初期テストには最適ですが、_wandb/local_のコラボレーション機能を活用するには、中央サーバーでサービスをホストする必要があります。さまざまなプラットフォームで集中型サーバーをセットアップする手順については、「セットアップ」セクションを参照してください。

### 基本構成

wandb localを実行すると、メトリックを[http://localhost:8080](http://localhost:8080/)にプッシュするようにあなたの機器が構成されます。別のポートでローカルをホストする場合は、-port引数をwandb localに渡すことができます。ローカルインスタンスでDNSを構成している場合は、メトリックをレポートする任意の機器で`wandb login --host=http://wandb.myhost.com`のコマンドを実行できます。また、WANDB\_BASE\_URL環境変数を、ローカルインスタンスに報告する任意の機器のホストまたはIPに設定することもできます。自動化された環境では、設定ページのAPIキー内に`WANDB_API_KEY`環境変数を設定することもできます。機器をクラウドホストソリューションへのレポートメトリックに復元するには、`wandb login --host=https://api.wandb.ai`を実行します。

### 認証

_wandb/local_の基本インストールは、デフォルトユーザーlocal@wandb.comから始まります。デフォルトのパスワードは**perceptron**です。フロントエンドはこのユーザーで自動的にログインを試み、パスワードをリセットするように求めます。ライセンスのないバージョンのwandbでは、最大4人のユーザーを作成できます。[`http://localhost:8080/admin/users`](http://localhost:8080/admin/users)にある_wandb/local_のユーザー管理ページでユーザーを構成できます。

### 統一性

W＆Bに送信されるすべてのメタデータとファイルは/volディレクトリに保存されます。この場所に統一されたボリュームをマウントしないと、Dockerプロセスが停止したときにすべてのデータが失われます。wandb/localのライセンスを購入すると、メタデータを外部MySQLデータベースに保存し、ファイルを外部ストレージバケットに保存して、ステートフルコンテナを不要にすることができます。

### アップグレード

新しいバージョンの_wandb/local_を定期的にdockerhubにプッシュしています。アップグレードするには、以下を実行します。

```text
$ wandb local --upgrade
```

 インスタンスを手動でアップグレードするには、次のコマンドを実行します

```text
$ docker pull wandb/local
$ docker stop wandb-local
$ docker run --rm -d -v wandb:/vol -p 8080:8080 --name wandb-local wandb/local
```

### ライセンスの取得

チームの構成、外部ストレージの使用、またはwandb/localのKubernetesクラスターへのデプロイに関心がある場合は、[contact@wandb.com](mailto:contact@wandb.com)までメールを送信してください

