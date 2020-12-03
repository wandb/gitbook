---
description: W＆Bローカルサーバーのインストールを構成する方法
---

# Advanced Configuration

W＆Bローカルサーバーは、起動時にすぐに使用できるようになります。ただし、サーバーが起動して実行されると、サーバーの`/system-admin`ページでいくつかの詳細な構成オプションを使用できます。[contact@wandb.com](mailto:contact@wandb.com)に電子メールを送信して、より多くのユーザーとチームを有効にするための試用ライセンスを要求できます。

##  コードとしての構成

すべての構成設定はUIを介して設定できますが、コードを介してこれらの構成オプションを管理する場合は、次の環境変数を設定できます。

* **LICENSE‐**wandb/localライセンス
* **MYSQL**‐MySQL接続文字列
* **BUCKET**‐データを保存するためのS3 / GCSバケット
* **BUCKET\_QUEUE**‐オブジェクト作成イベント用のSQS / Google PubSubキュー
* **NOTIFICATIONS\_QUEUE**‐実行イベントを公開するSQSキュー
* **AWS\_REGION**‐AWSバケットが存在するリージョン
* **HOST**‐インスタンスのFQD、つまり[https://my.domain.net](https://my.domain.net/)\*\*\*\*
* **AUTH0\_DOMAIN**‐テナントのAuth0ドメイン
* **AUTH0\_CLIENT\_ID**‐アプリケーションのAuth0クライアントID

##  認証

デフォルトでは、W＆Bローカルサーバーは手動のユーザー管理で実行され、最大4人のユーザーが使用できます。ライセンスバージョンの_wandb/local_も、Auth0を使用してSSOのロックを解除します。

 サーバーは、[Auth0](https://auth0.com/)でサポートされているすべての認証プロバイダーをサポートしています。チームの管理下にある独自のAuth0ドメインとアプリケーションを設定する必要があります。

Auth0アプリを作成したら、W＆BサーバーのホストへのAuth0コールバックを構成する必要があります。デフォルトでは、サーバーはホストによって提供されたパブリックまたはプライベートIPアドレスからのhttpをサポートします。必要に応じて、DNSホスト名とSSL証明書を構成することもできます。

* コールバックURLを`http(s)://YOUR-W&B-SERVER-HOST`に設定します
* 許可されたWebオリジンを`http(s)://YOUR-W&B-SERVER-HOST`に設定します
* ログアウトURLを`http(s)://YOUR-W&B-SERVER-HOST/logout`に設定します

![Auth0 Settings](../.gitbook/assets/auth0-1.png)

 Auth0アプリからクライアントIDとドメインを保存します。

![Auth0 Settings](../.gitbook/assets/auth0-2.png)

次に、`http(s)://YOUR-W&B-SERVER-HOST/admin-settings`のW＆B設定ページに移動します。\[Auth0で認証をカスタマイズする\]オプションを有効にし、Auth0アプリからクライアントIDとドメインを入力します。

![&#x30A8;&#x30F3;&#x30BF;&#x30FC;&#x30D7;&#x30E9;&#x30A4;&#x30BA;&#x8A8D;&#x8A3C;&#x8A2D;&#x5B9A;](../.gitbook/assets/enterprise-auth.png)

最後に、「設定を更新してW＆Bを再起動」を押します。

##  ファイルストレージ

デフォルトでは、W＆B Enterprise Serverは、インスタンスのプロビジョニング時に設定した容量でファイルをローカルデータディスクに保存します。無制限のファイルストレージをサポートするために、S3互換APIで外部クラウドファイルストレージバケットを使用するようにサーバーを構成できます。

###  アマゾンウェブサービス

AWS S3バケットをW＆Bのファイルストレージバックエンドとして使用するには、バケットを作成する必要があります。また、そのバケットからオブジェクト作成通知を受信するように設定されたSQSキューも作成する必要があります。このキューから読み取るには、インスタンスに権限が必要です。

 **SQSキューの作成**

まず、SQS標準キューを作成します。SendMessageアクションとReceiveMessageアクション、およびGetQueueUrlのすべてのプリンシパルに対するアクセス許可を追加します。（必要に応じて

![Enterprise file storage settings](../.gitbook/assets/sqs-perms.png)

**S3バケットとバケット通知の作成**

次に、S3バケットを作成します。コンソールのバケットプロパティページの\[詳細設定\]の\[イベント\]セクションで、\[通知の追加\]をクリックし、前に設定したSQSキューに送信されるようにすべてのオブジェクト作成イベントを設定します。

![Enterprise file storage settings](../.gitbook/assets/s3-notification.png)

Enable CORS access: your CORS configuration should look like the following:

```markup
<?xml version="1.0" encoding="UTF-8"?>
<CORSConfiguration xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
<CORSRule>
    <AllowedOrigin>http://YOUR-W&B-SERVER-IP</AllowedOrigin>
    <AllowedMethod>GET</AllowedMethod>
    <AllowedMethod>PUT</AllowedMethod>
    <AllowedHeader>*</AllowedHeader>
</CORSRule>
</CORSConfiguration>
```

**Configure W&B Server**

Finally, navigate to the W&B settings page at `http(s)://YOUR-W&B-SERVER-HOST/admin-settings`. Enable the "Use an external file storage backend" option, and fill in the s3 bucket, region, and SQS queue in the following format:

* **File Storage Bucket**: `s3://<bucket-name>`
* **File Storage Region**: `<region>`
* **Notification Subscription**: `sqs://<queue-name>`

![AWS file storage settings](../.gitbook/assets/aws-filestore.png)

Press "update settings and restart W&B" to apply the new settings.

### Google Cloud Platform

To use a GCP Storage bucket as a file storage backend for W&B, you'll need to create a bucket, along with a pubsub topic and subscription configured to receive object creation messages from that bucket.

**Create Pubsub Topic and Subscription**

Navigate to Pub/Sub &gt; Topics in the GCP Console, and click "Create topic". Choose a name and create a topic.

Then click "Create subscription" in the subscriptions table at the bottom of the page. Choose a name, and make sure Delivery Type is set to "Pull". Click "Create".

Make sure the service account or account that your instance is running as has access to this subscription.

**Create Storage Bucket**

Navigate to Storage &gt; Browser in the GCP Console, and click "Create bucket". Make sure to choose "Standard" storage class.

Make sure the service account or account that your instance is running as has access to this bucket.

**Create Pubsub Notification**

Creating a notification stream from the Storage Bucket to the Pubsub Topic can unfortunately only be done in the console. Make sure you have `gsutil` installed, and logged into the correct GCP Project, then run the following:

```bash
gcloud pubsub topics list  # list names of topics for reference
gsutil ls                  # list names of buckets for reference

# create bucket notification
gsutil notification create -t <TOPIC-NAME> -f json gs://<BUCKET-NAME>
```

[Further reference is available on the Cloud Storage website.](https://cloud.google.com/storage/docs/reporting-changes)

**Add Signing Permissions**

To create signed file URLs, your W&B instance also needs the `iam.serviceAccounts.signBlob` permission in GCP. You can add it by adding the `Service Account Token Creator` role to the service account or IAM member that your instance is running as.

**Configure W&B Server**

Finally, navigate to the W&B settings page at `http(s)://YOUR-W&B-SERVER-HOST/admin-settings`. Enable the "Use an external file storage backend" option, and fill in the s3 bucket, region, and SQS queue in the following format:

* **File Storage Bucket**: `gs://<bucket-name>`
* **File Storage Region**: blank
* **Notification Subscription**: `pubsub:/<project-name>/<topic-name>/<subscription-name>`

![GCP file storage settings](../.gitbook/assets/gcloud-filestore.png)

Press "update settings and restart W&B" to apply the new settings.

