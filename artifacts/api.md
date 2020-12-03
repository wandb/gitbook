# Artifacts API

データセットの追跡とモデルのバージョン管理にW＆Bアーティファクトを使用します。実行を初期化し、アーティファクトを作成してから、ワークフローの別の部分で使用します。アーティファクトを使用して、ファイルを追跡および保存したり、外部URIを追跡したりできます。

この機能は、`wandb`バージョン0.9.0以降のクライアントで使用できます。

## 1. **1.実行の初期化**

パイプラインのステップを追跡するには、スクリプトで実行を初期化します。job\_typeの文字列を指定して、前処理、トレーニング、評価などのさまざまなパイプラインステップを区別します。W＆Bで実行をインストルメント化したことがない場合は、[Pythonライブ](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MNdlQobOrN8f63KfkRZ/v/japanese/library)ラリのドキュメントに実験追跡の詳細なガイダンスがあります。

```python
run = wandb.init(job_type='train')
```

## 2. **2.アーティファクトの作成**

アーティファクトとはデータのフォルダのようなもので、コンテンツはアーティファクトに保存されている実際のファイルまたは外部URIへの参照です。アーティファクトを作成するには、実行の出力としてログに記録します。さまざまなアーティファクト（データセット、モデル、結果など）を区別するために、タイプの文字列を指定します。アーティファクトの内部にあるものを思い出しやすいように、このアーティファクトにbike-datasetなどの名前を付けます。パイプラインの後のステップで、この名前をbike-dataset：v1のようなバージョンと一緒に使用して、このアーティファクトをダウンロードできます。log\_artifactを呼び出すと、アーティファクトの内容が変更されているかどうかが確認され、変更されている場合は、アーティファクトの新しいバージョン（v0、v1、v2など）が自動的に作成されます。

* **wandb.Artifact\(\)type \(str\)**：組織的な目的で使用されるアーティファクトの種類を区別します。「データセット」、「モデル」、「結果」に沿って行うことをお勧めします。
* **name \(str\)**：アーティファクトにユニークな名前を付けます。これは、アーティファクトを他の場所で参照するときに使用されます。名前には、数字、文字、アンダースコア、ハイフン、およびドットを使用できます。
* **description \(str, optional\)**：UIのアーティファクトバージョンの横に表示されるフリーテキスト
* **metadata \(dict, optional\)**：データセットのクラス分布など、アーティファクトに関連付けられた構造化データ。Webインターフェースを構築すると、このデータを使用してクエリを実行し、プロットを作成できるようになります。

```python
artifact = wandb.Artifact('bike-dataset', type='dataset')

# Add a file to the artifact's contents
artifact.add_file('bicycle-data.h5')

# Save the artifact version to W&B and mark it as the output of this run
run.log_artifact(artifact)
```

## **3.アーティファクトの使用**

アーティファクトを実行への入力として使用できます。たとえば、`bike-dataset`の最初のバージョンである`bike-dataset:v0`を取得し、パイプラインの次のスクリプトで使用できます。**use\_artifact**を呼び出すと、スクリプトはW＆Bにクエリを実行して、その名前付きアーティファクトを見つけ、実行への入力としてマークします。

```python
# Query W&B for an artifact and mark it as input to this run
artifact = run.use_artifact('bike-dataset:v0')

# Download the artifact's contents
artifact_dir = artifact.download()
```

**別のプロジェクトのアーティファクトの使用**　

アーティファクトの名前をプロジェクト名で修飾することにより、アクセスできる任意のプロジェクトのアーティファクトを自由に参照できます。アーティファクトの名前をエンティティ名でさらに修飾することにより、エンティティ間でアーティファクトを参照することもできます。

```python
# Query W&B for an artifact from another project and mark it
# as an input to this run.
artifact = run.use_artifact('my-project/bike-model:v0')

# Use an artifact from another entity and mark it as an input
# to this run.
artifact = run.use_artifact('my-entity/my-project/bike-model:v0')
```

**ログに記録されていないアーティファクトの使用**　

アーティファクトオブジェクトを作成して**use\_artifact**に渡すこともできます。アーティファクトがすでにW＆Bに存在するかどうかを確認し、存在しない場合は新しいアーティファクトを作成します。これが冪（べき）等性です。アーティファクトをuse\_artifactに何度でも渡すことができるし、内容が同じである限り重複排除します。

```python
artifact = wandb.Artifact('bike-model', type='model')
artifact.add_file('model.h5')
run.use_artifact(artifact)
```

## **バージョンとエイリアス**

 アーティファクトを初めてログに記録するときに、バージョン**v0**を作成します。同じアーティファクトに再度ログインすると、内容がチェックサムされ、アーティファクトが変更されている場合は、新しいバージョン**v1**が保存されます。

特定のバージョンへのポインタとしてエイリアスを使用できます。デフォルトでは、run.log\_artifactはログに記録されたバージョンに**最新**エイリアスを追加します

エイリアスを使用してアーティファクトをフェッチできます。たとえば、トレーニングスクリプトで常に最新バージョンのデータセットを取得する場合は、そのアーティファクトを使用するときに**最新バージョン**を指定します。

```python
artifact = run.use_artifact('bike-dataset:latest')
```

アーティファクトバージョンにカスタムエイリアスを適用することもできます。たとえば、メトリックAP-50でどのモデルチェックポイントが最適であるかをマークする場合は、モデルアーティファクトをログに記録するときに、エイリアスとして文字列**best-ap50**を追加できます。

```python
artifact = wandb.Artifact('run-3nq3ctyy-bike-model', type='model')
artifact.add_file('model.h5')
run.log_artifact(artifact, aliases=['latest','best-ap50'])
```

## **アーティファクトの構築**

アーティファクトはデータのフォルダのようなものです。各エントリは、アーティファクトに保存されている実際のファイル、または外部URIへの参照のいずれかです。通常のファイルシステムと同じように、アーティファクト内にフォルダをネストできます。`wandb.Artifact()`クラスを初期化して、新しいアーティファクトを作成します。

次のフィールドを`Artifact()`コンストラクターに渡すか、アーティファクトオブジェクトに直接設定できます。

* **タイプ**：「データセット」、「モデル」、または「結果」である必要があります•**説明**：UIに表示される自由形式のテキスト。
* •**メタデータ**：任意の構造化データを含めることができる辞書。このデータを使用して、クエリを実行したり、プロットを作成したりできます。例えば、データセットアーティファクトのクラス分布をメタデータとして保存することを選択できます。
* **name**を使用してオプションのファイル名を指定するか、ディレクトリを追加する場合はファイルパスプレフィックスを指定します。

```python
artifact = wandb.Artifact('bike-dataset', type='dataset')
```

**name**を使用してオプションのファイル名を指定するか、ディレクトリを追加する場合はファイルパスプレフィックスを指定します

```python
# Add a single file
artifact.add_file(path, name='optional-name')

# Recursively add a directory
artifact.add_dir(path, name='optional-prefix')

# Return a writeable file-like object, stored as <name> in the artifact
with artifact.new_file(name) as f:
    ...  # Write contents into the file 

# Add a URI reference
artifact.add_reference(uri, name='optional-name')
```

###  ファイルおよびディレクトリの追加

次の例では、このようなファイルを含むプロジェクトディレクトリがあると想定します。

```text
project-directory
|-- images
|   |-- cat.png
|   +-- dog.png
|-- checkpoints
|   +-- model.h5
+-- model.h5
```

<table>
  <thead>
    <tr>
      <th style="text-align:left">API call</th>
      <th style="text-align:left">Resulting artifact contents</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">artifact.add_file(&apos;model.h5&apos;)</td>
      <td style="text-align:left">model.h5</td>
    </tr>
    <tr>
      <td style="text-align:left">artifact.add_file(&apos;checkpoints/model.h5&apos;)</td>
      <td style="text-align:left">model.h5</td>
    </tr>
    <tr>
      <td style="text-align:left">artifact.add_file(&apos;model.h5&apos;, name=&apos;models/mymodel.h5&apos;)</td>
      <td
      style="text-align:left">models/mymodel.h5</td>
    </tr>
    <tr>
      <td style="text-align:left">artifact.add_dir(&apos;images&apos;)</td>
      <td style="text-align:left">
        <p>cat.png</p>
        <p>dog.png</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">artifact.add_dir(&apos;images&apos;, name=&apos;images&apos;)</td>
      <td
      style="text-align:left">
        <p>images/cat.png</p>
        <p>images/dog.png</p>
        </td>
    </tr>
    <tr>
      <td style="text-align:left">artifact.new_file(&apos;hello.txt&apos;)</td>
      <td style="text-align:left">hello.txt</td>
    </tr>
  </tbody>
</table>

###  レファレンスの追加

```python
artifact.add_reference(uri, name=None, checksum=True)
```

* **uri \(string\):** 追跡するレファレンスURI。
* **name \(string\):** オプションの名前オーバーライド。指定しない場合、名前は**uri**から推測されます
* **checksum \(bool\):** trueの場合、レファレンスは検証の目的で**uri**からチェックサム情報とメタデータを収集します。

実際のファイルの代わりに、外部URIへの参照をアーティファクトに追加できます。wandbが処理方法を知っているスキームがURIにある場合、アーティファクトは再現性のためにチェックサムやその他の情報を追跡します。アーティファクトは現在、次のURIスキームをサポートしています。

* `http(s)://`: HTTP経由でアクセス可能なファイルへのパス。HTTPサーバーがETagおよびContent-Length応答ヘッダーをサポートしている場合、アーティファクトはetagおよびサイズメタデータの形式でチェックサムを追跡します。
* `s3://`: S3のオブジェクトまたはオブジェクトプレフィックスへのパス。アーティファクトは、参照されたオブジェクトのチェックサムとバージョン情報（バケットでオブジェクトのバージョン管理が有効になっている場合）を追跡します。オブジェクトプレフィックスは、最大10,000オブジェクトまで、プレフィックスの下のオブジェクトを含むように拡張されます。
* `gs://`: GCSのオブジェクトまたはオブジェクトプレフィックスへのパス。アーティファクトは、参照されたオブジェクトのチェックサムとバージョン情報（バケットでオブジェクトのバージョン管理が有効になっている場合）を追跡します。オブジェクトプレフィックスは、最大10,000オブジェクトまで、プレフィックスの下のオブジェクトを含むように拡張されます。

次の例では、これらのファイルを含むS3バケットがあると想定します。

```text
s3://my-bucket
|-- images
|   |-- cat.png
|   +-- dog.png
|-- checkpoints
|   +-- model.h5
+-- model.h5
```

<table>
  <thead>
    <tr>
      <th style="text-align:left">API call</th>
      <th style="text-align:left">Resulting artifact contents</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">artifact.add_reference(&apos;s3://my-bucket/model.h5&apos;)</td>
      <td style="text-align:left">model.h5</td>
    </tr>
    <tr>
      <td style="text-align:left">artifact.add_reference(&apos;s3://my-bucket/checkpoints/model.h5&apos;)</td>
      <td
      style="text-align:left">model.h5</td>
    </tr>
    <tr>
      <td style="text-align:left">artifact.add_reference(&apos;s3://my-bucket/model.h5&apos;, name=&apos;models/mymodel.h5&apos;)</td>
      <td
      style="text-align:left">models/mymodel.h5</td>
    </tr>
    <tr>
      <td style="text-align:left">artifact.add_reference(&apos;s3://my-bucket/images&apos;)</td>
      <td style="text-align:left">
        <p>cat.png</p>
        <p>dog.png</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">artifact.add_reference(&apos;s3://my-bucket/images&apos;, name=&apos;images&apos;)</td>
      <td
      style="text-align:left">
        <p>images/cat.png</p>
        <p>images/dog.png</p>
        </td>
    </tr>
  </tbody>
</table>

##  **アーティファクトの使用とダウンロード**

```python
run.use_artifact(artifact=None)
```

* 実行への入力としてアーティファクトをマークします。

アーティファクトの使用には2つのパターンがあります。W＆Bに明示的に格納されているアーティファクト名を使用することも、アーティファクトオブジェクトを作成して渡し、必要に応じて重複排除することもできます。

### W＆Bに保存されているアーティファクトを使用します

```python
artifact = run.use_artifact('bike-dataset:latest')
```

 返されたアーティファクトに対して次のメソッドを呼び出すことができます。

```python
datadir = artifact.download(root=None)
```

* 現在存在しないアーティファクトのコンテンツをすべてダウンロードします。これにより、アーティファクトのコンテンツを含むディレクトリへのパスが返されます。**root**を設定することで、ダウンロード先を明示的に指定できます。

```python
path = artifact.get_path(name)
```

* パス名のファイルのみを取得します。次のメソッドを使用して`Entry`オブジェクトを返します。
  * **Entry.download\(\)**: Downloads file from the artifact at path `name`
  *  **Entry.ref（）**：エントリが`add_reference`を使用して参照として保存された場合、URIを返します

W＆Bが処理方法を知っているスキームを持つ参照は、アーティファクトファイルと同じようにダウンロードできます。コンシューマーAPIも同様です。



アーティファクトオブジェクトを作成して**use\_artifact**に渡すこともできます。これにより、アーティファクトがまだ存在しない場合、W＆Bにアーティファクトが作成されます。これは冪（べき）等性であるため、何度でも実行できます。model.h5の内容が同じである限り、アーティファクトは1回だけ作成されます。

実行外でアーティファクトをダウンロードします

```python
artifact = wandb.Artifact('reference model')
artifact.add_file('model.h5')
run.use_artifact(artifact)
```

###  アーティファクトを作成して使用します

```python
api = wandb.Api()
artifact = api.artifact('entity/project/artifact:alias')
artifact.download()
```

## アーティファクトの更新

アーティファクトの説明、メタデータ、およびエイリアスを目的の値に設定してからsave（）を呼び出して、それらを更新できます。

```python
api = wandb.Api()
artifact = api.artifact('bike-dataset:latest')

# Update the description
artifact.description = "My new description"

# Selectively update metadata keys
artifact.metadata["oldKey"] = "new value"

# Replace the metadata entirely
artifact.metadata = {"newKey": "new value"}

# Add an alias
artifact.aliases.append('best')

# Remove an alias
artifact.aliases.remove('latest')

# Completely replace the aliases
artifact.aliases = ['replaced']

# Persist all artifact modifications
artifact.save()
```

## データのプライバシー

アーティファクトは、安全なAPIレベルのアクセス制御を使用します。ファイルは保存時と転送中に暗号化されます。アーティファクトは、ファイルの内容をW＆Bに送信せずに、プライベートバケットへの参照を追跡することもできます。別の方法については、[contact@wandb.com](mailto:contact@wandb.com)まで連絡して、プライベートクラウドとオンプレミスのインストールについてお問い合わせください。

