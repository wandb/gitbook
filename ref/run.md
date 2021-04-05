# Run

## wandb.sdk.wandb\_run

[ \[ソースを表示\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_run.py#L4)

###  試行オブジェクト

```python
class Run(object)
```

## 実行

[ソース](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_run.py#L132) \[ソースを表示\]

エンティティとプロジェクトに関連付けられた単一の実行。

## **属性：**

•`tags` _\[str\]_‐実行に関連付けられたタグのリスト

•`url` _str_‐この実行のURL

•`id` _str_‐実行の一意の識別子（デフォルトは8文字）

•`name` _str_‐実行の名前

•`state` _str_‐実行中、終了、クラッシュ、中止のいずれか

•`config` _dict_‐実行に関連付けられたハイパーパラメータの辞書

•`created_at` _str_‐実行が開始されたときのISOタイムスタンプ

•`system_metrics` _dict_‐実行のために記録された最新のシステムメトリック

•`summary` _dict_‐現在の要約を保持する可変の辞書のようなプロパティ。updateを呼び出すと、変更が保持されます。

•`project` _str_‐実行に関連付けられたプロジェクト

•`entity` _str_‐実行に関連付けられたエンティティの名前

•`user` _str_‐実行を作成したユーザーの名前

•`path` _str_‐ユニークな識別子\[entity\]/\[project\]/\[run\_id\]

•`notes` _str_‐実行に関するメモ

•`read_only` _boolean_‐実行が編集可能の如何

•`history_keys` _str_‐`wandb.log({key: value})`でログに記録された履歴メトリックのキー

**dir**

```python
 | @property
 | dir()
```

 [\[ソースを表示\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_run.py#L334)

str：試行の表示名。ユニークである必要はなく、説明的なのが理想的です。

**config**

```python
 | @property
 | config()
```

 [\[ソースを表示\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_run.py#L341)

\(`Config`\): 試行のハイパーパラメーターに関連付けられたキーと値のペアの構成オブジェクト（ネストされた辞書に類似）。

 **構成**

```python
 | @property
 | name()
```

 [\[ソースを表示\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_run.py#L352)

str：試行の表示名。ユニークである必要はなく、説明的なのが理想的です。

 **名前**

```python
 | @property
 | notes()
```

[\[ソースを表示\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_run.py#L368)

str：試行に関連するメモ。メモは複数行の文字列にすることができ、${x}のように$$内でマークダウンとラテックスの方程式を使用することもできます。

**ノート**

```python
 | @property
 | tags() -> Optional[Tuple]
```

 [\[ソースを表示\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_run.py#L384)

Tuple\[str\]：試行に関連付けられたタグ

**id**

```python
 | @property
 | id()
```

 [\[ソースを表示\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_run.py#L398)

str：試行に関連付けられたrun\_id

**sweep\_id**

```python
 | @property
 | sweep_id()
```

 [\[ソースを表示\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_run.py#L403)

（str, optional）：試行に関連付けられたスイープIDまたはなし

**path**

```python
 | @property
 | path()
```

 [\[ソースを表示\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_run.py#L410)

 str：試行\[entity\]/\[project\]/\[run\_id\]へのパス

**start\_time**

```python
 | @property
 | start_time()
```

 [\[ソースを表示\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_run.py#L419)

int：試行が開始されたときのunixタイムスタンプ（秒単位）

**starting\_step**

```python
 | @property
 | starting_step()
```

 [\[ソースを表示\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_run.py#L427)

int：試行の最初のステップ

 **再開**

```python
 | @property
 | resumed()
```

 [\[ソースを表示\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_run.py#L435)

bool: 試行の再開如何

**ステップ**

```python
 | @property
 | step()
```

 [\[ソースを表示\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_run.py#L443)

int: ステップカウンター

wandb.log（）を呼び出すたびに、デフォルトでステップカウンターがインクリメントされます。

 **モード**

```python
 | @property
 | mode()
```

 [\[ソースを表示\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_run.py#L465)

0.9.x以前との互換性のため、最終的には非推奨になります。

**グループ**

```python
 | @property
 | group()
```

[\[view\_source\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_run.py#L465) \[ソースを表示\]

str: 試行に関連付けられたW＆Bグループの名前。

グループを設定すると、W＆B UIが適切な方法で試行を整理するのに役立ちます。

分散トレーニングを行っている場合は、トレーニングのすべての試行を同じグループに行う必要があります。交差検定を行う場合は、すべての交差検定フォールドに同じグループを与える必要があります。

**プロジェクト**

```python
 | @property
 | project()
```

 [\[ソースを表示\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_run.py#L484)

str：試行に関連付けられたW＆Bプロジェクトの名前。

**get\_url**

```python
 | get_url()
```

 [\[ソースを表示\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_run.py#L488)

戻り値：（str, optional）：W＆B試行のURL、または試行がオフラインの場合はNone

**get\_project\_url**

```python
 | get_project_url()
```

 [\[ソースを表示\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_run.py#L496)

戻り値：（str, optional）：試行に関連付けられたW＆BプロジェクトのURL、または試行がオフラインの場合はNone

**get\_sweep\_url**

```python
 | get_sweep_url()
```

 [\[ソースを表示\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_run.py#L504)

戻り値：（str, optional）：試行に関連付けられたスイープのURL。関連付けられたスイープがない場合、または試行がオフラインの場合はNone。

**url**

```python
 | @property
 | url()
```

 [\[ソースを表示\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_run.py#L518)

str: 試行に関連付けられたW＆B URLの名前。

 **エンティティ**

```python
 | @property
 | entity()
```

  [\[ソースを表示\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_run.py#L518)

str：試行に関連付けられたW＆Bエンティティの名前。エンティティは、ユーザー名または組織名のいずれかです。

 **ログ**

```python
 | log(data, step=None, commit=None, sync=None)
```

 [\[ソースを表示\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_run.py#L672)

 グローバル試行の履歴にdictを記録します。

wandb.logを使用して、スカラーからヒストグラム、メディア、matplotlibプロットまですべてをログに記録できます。

最も基本的な使用法はwandb.log\({'train-loss': 0.5, 'accuracy': 0.9}\)です。これにより、train-loss=0.5およびaccuracy=0.9の試行に関連付けられた履歴行が保存されます。履歴値は、app.wandb.aiまたはローカルサーバーにプロットできます。履歴値は、wandb APIを介してダウンロードすることもできます。

値をログに記録すると、ログに記録されたすべてのメトリックの要約値が更新されます。 サマリー値は、app.wandb.aiまたはローカルサーバーの試行テーブルに表示されます。たとえば、要約値がwandb.run.summary\["accuracy"\]=0.9で手動で設定されている場合、wandb.logは試行の精度を自動的に更新しなくなりました。

ロギング値はスカラーである必要はありません。wandbオブジェクトのロギングがサポートされています。たとえば、wandb.log\({"example": wandb.Image\("myimage.jpg"\)}\)は、wandb UIに適切に表示されるサンプル画像をログに記録します。サポートされているさまざまなタイプのすべてについては、[https://docs.wandb.com/library/reference/data\_types](https://docs.wandb.com/library/reference/data_types)を参照してください。

 ネストされたメトリックのログ記録が推奨され、wandb APIでサポートされているため、wandb.log\({'dataset-1': {'acc': 0.9, 'loss': 0.3} ,'dataset-2': {'acc': 0.8, 'loss': 0.2}}\)を使用して複数の精度値をログに記録できます。また、メトリックはwandb UIで編成されます。

W＆Bはグローバルステップを追跡するため、関連するメトリックを一緒にログに記録することが推奨されます。したがって、デフォルトでは、wandb.logが呼び出されるたびにグローバルステップがインクリメントされます。wandb.log（\({'train-loss': 0.5, commit=False}\)を呼び出して関連するメトリックを一緒にログに記録するのが不便な場合、wandb.log\({'accuracy': 0.9}\)がwandb.log\({'train-loss': 0.5, 'accuracy': 0.9}\)を呼び出すことと同等です。

wandb.logは、1秒間に数回以上呼び出されることを意図していません。それよりも頻繁にログを記録する場合は、クライアント側でデータを集約することをお勧めします。そうしないと、パフォーマンスが低下する可能性があります。

 **引数：**

* `row` dict、オプション-シリアル化可能なPythonオブジェクトのdict、つまりstr、int、floats、Tensors、dicts、またはwandb.data\_types
* `commit`、オプション–メトリックディクテーションをwandbサーバーに保存し、ステップをインクリメントします。falseの場合、wandb.logは現在のメトリック辞書をrow引数で更新するだけであり、コミット=Trueでwandb.logが呼び出されるまでメトリックは保存されません。
* `step` _integer_、オプション–処理のグローバルステップ。これにより、コミットされていない以前のステップは保持されますが、デフォルトでは、指定されたステップはコミットされません。
* `sync` _boolean_、True-この引数は非推奨であり、現在wandb.logの動作は変更されていません。

 **例：**

基本的な使い方

```text
- `wandb.log({'accuracy'` - 0.9, 'epoch': 5})
```

インクリメンタルロギング

```text
- `wandb.log({'loss'` - 0.2}, commit=False)
# Somewhere else when I'm ready to report this step:
- `wandb.log({'accuracy'` - 0.8})
```

 ヒストグラム

```text
- `wandb.log({"gradients"` - wandb.Histogram(numpy_array_or_sequence)})
```

 画像

```text
- `wandb.log({"examples"` - [wandb.Image(numpy_array_or_pil, caption="Label")]})
```

ビデオ

```text
- `wandb.log({"video"` - wandb.Video(numpy_array_or_video_path, fps=4,
format="gif")})
```

 Matplotlibプロット

```text
- `wandb.log({"chart"` - plt})
```

PR曲線

```text
- `wandb.log({'pr'` - wandb.plots.precision_recall(y_test, y_probas, labels)})
```

3Dオブジェクト

```text
wandb.log({"generated_samples":
[wandb.Object3D(open("sample.obj")),
wandb.Object3D(open("sample.gltf")),
wandb.Object3D(open("sample.glb"))]})
```

その他の例については、[https://docs.wandb.com/library/log](https://docs.wandb.com/library/log)を参照してください。

**レイズ：**

wandb.Error-wandb.initの前に呼び出された場合ValueError‐無効なデータが渡された場合

 **保存**

```python
 | save(glob_str: Optional[str] = None, base_path: Optional[str] = None, policy: str = "live")
```

 [\[ソースを表示\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_run.py#L807)

glob\_strに一致するすべてのファイルが、指定されたポリシーでwandbに同期されていることを確認します。

**引数：**

* `glob_str` _string_ - 文字列‐UNIXグロブまたは通常のパスへの相対パスまたは絶対パス。これが指定されていない場合、メソッドはnoopです。
* `base_path` _string_ - 文字列‐グロブを試行するためのベースパス
* `policy` 文字列‐「live」、「now」、または「end」のオン
* `live` - 変更されたファイルをアップロードし、以前のバージョンを上書きします
* `now` -ファイルを今すぐアップロードします
* `end` - 試行が終了したときにのみファイルをアップロードします

 **完了**

```python
 | finish(exit_code=None)
```

 [\[ソースを表示\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_run.py#L902)

試行を終了としてマークし、すべてのデータのアップロードを終了します。これは、同じプロセスで複数の試行を作成するときに使用されます。スクリプトが終了すると、このメソッドが自動的に呼び出されます。

 **参加**

```python
 | join(exit_code=None)
```

 [\[ソースを表示\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_run.py#L916)

finish（）の非推奨のエイリアス‐finishを使用してください

**plot\_table**

```python
 | plot_table(vega_spec_name, data_table, fields, string_fields=None)
```

[\[ソースを表示\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_run.py#L920)

テーブルにカスタムプロットを作成します。

 **引数：**

* `vega_spec_name` - プロットの仕様の名前
* `table_key` - データテーブルをログに記録するために使用されるキー
* `data_table` - ビジュアライゼーションで使用されるデータを含むwandb.Tableオブジェクト
* `fields` - テーブルキーからカスタムビジュアライゼーションに必要なフィールドへのdictマッピング
* `string_fields` - カスタム視覚化に必要な文字列定数の値を提供するdict

**use\_artifact**

```python
 | use_artifact(artifact_or_name, type=None, aliases=None)
```

 [\[ソースを表示\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_run.py#L1552)

アーティファクトを試行への入力として宣言し、返されたオブジェクトでdownloadまたはfileを呼び出して、コンテンツをローカルで取得します。

**引数：**

* 接頭辞としてエンティティ/プロジェクトを付けることができます。有効な名前は次の形式になります。

  name: ダイジェスト

  name:alias

  digest

   `wandb.Artifact` を呼び出して作成したArtifactオブジェクトを渡すこともできます。

* `type` _str_、オプション-使用するアーティファクトのタイプ。
* `aliases` エイリアスリスト、オプション‐このアーティファクトに適用するエイリアス

**戻り値：**

アーティファクトオブジェクト。

**log\_artifact**

```python
 | log_artifact(artifact_or_path, name=None, type=None, aliases=None)
```

 [\[ソースを表示\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_run.py#L1607)

アーティファクトを試行の出力として宣言します。

**引数：**

* `artifact_or_path` _str_  またはArtifact-このアーティファクトのコンテンツへのパスは、次の形式にすることができます。

  /local/directory

  /local/directory/file.txt

  s3://bucket/path

  `wandb.Artifact` wandb.Artifactを呼び出して作成したArtifactオブジェクトを渡すこともできます。

* `name` _str_、オプション-アーティファクト名。接頭辞としてエンティティ/プロジェクトを付けることができます。

  名前：バージョン

  名前：エイリアス

  ダイジェスト

  指定されていない場合、これはデフォルトで現在の試行IDが付加されたパスのベース名になります。

* `type` _str_ - ログに記録するアーティファクトのタイプ。例には「dataset」、「model」が含まれます
* エイリアスリスト、オプション-このアーティファクトに適用するエイリアス。デフォルトは\["latest"\]です。

**戻り値：**

アーティファクトオブジェクト。

**アラート**

```python
 | alert(title, text, level=None, wait_duration=None)
```

 [\[ソースを表示\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_run.py#L1661)

指定されたタイトルとテキストでアラートを起動します。

 **引数：**

* `title` _str_ - アラートのタイトル。長さは64文字未満である必要があります。
* `text` _str_ - アラートのテキスト本文
* `level` _str_  またはwandb.AlertLevel、オプション-使用するアラートレベル：「INFO」、「WARN」、または「ERROR」のいずれか
* `wait_duration` _int, float,_またはtimedelta、オプション-このタイトルの別のアラートを送信する前に待機する時間（秒単位）

