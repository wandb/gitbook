---
description: wandb.apis.public
---

# Public API Reference

## wandb.apis.public

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1)

### APIオブジェクト

```python
class Api(object)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L181)

wandbサーバーのクエリに使用されます。

 **例：**

 初期化する最も一般的な方法

```text
wandb.Api()
```

 **引数：**

* `overrides` -[https://api.wandb.ai](https://api.wandb.ai/)以外のwandbサーバーを使用している場合は、base\_urlを設定できます。

  エンティティ、プロジェクト、および試行のデフォルトを設定することもできます。

 **完了**

```python
 | flush()
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L276)

apiオブジェクトは試行のローカルキャッシュを保持するため、スクリプトの試行中に試行の状態が変更される可能性がある場合は、`api.flush（）`を使用してローカルキャッシュをクリアし、試行に関連付けられた最新の値を取得する必要があります。

 **プロジェクト**

```python
 | projects(entity=None, per_page=200)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L338)

 特定のエンティティのプロジェクトを取得します。

 **引数：**

* `entity` _str_ -–要求されたエンティティの名前。Noneの場合、Apiに渡されたデフォルトエンティティにフォールバックします。デフォルトのエンティティがない場合、ValueErrorが発生します。
* `per_page` _int_ - クエリページネーションのページサイズを設定します。デフォルトのサイズを使用するものはありません。通常、これを変更する理由はありません。

**戻り値：**

`Project`オブジェクトの反復可能なコレクションである`Projects`オブジェクト。

**レポート**

```python
 | reports(path="", name=None, per_page=50)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L360)

特定のプロジェクトパスのレポートを取得します。

警告：このAPIはベータ版であり、将来のリリースで変更される可能性があります

 **引数：**

* `path` _str_ - レポートが存在するプロジェクトへのパス。「エンティティ/プロジェクト」の形式である必要があります。
* `name` _str_ - 要求されたレポートのオプションの名前。
* `per_page` _int_ - クエリページネーションのページサイズを設定します。 デフォルトのサイズを使用するものはありません。

  通常、これを変更する理由はありません。

**戻り値：**

`BetaReport`オブジェクトの反復可能なコレクションである`Reports`オブジェクト。

試行

```python
 | runs(path="", filters={}, order="-created_at", per_page=50)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L393)

提供されたフィルターに一致するプロジェクトからの一連の試行を返します。`config.*`, `summary.*`, `state`, `entity`, `createdAt`などでフィルタリングできます。

**例**:

my\_projectでの検索の試行config.experiment\_nameが「foo」に設定されている

```text
api.runs(path="my_entity/my_project", {"config.experiment_name": "foo"})
```

my\_projectでの検索の試行config.experiment\_nameが「foo」または「bar」に設定されている

```text
api.runs(path="my_entity/my_project",
- `{"$or"` - [{"config.experiment_name": "foo"}, {"config.experiment_name": "bar"}]})
```

損失の昇順でソートされたmy\_projectの試行を検索

```text
api.runs(path="my_entity/my_project", {"order": "+summary_metrics.loss"})
```

**引数：**

* `path`  str-プロジェクトへのパス。「entity/project」の形式である必要があります。
* `filters` dict-MongoDBクエリ言語を使用した特定の試行のクエリ。

  config.key、summary\_metrics.key、state、entity、createdAtなどの試行プロパティでフィルタリングできます。

  例：{"config.experiment\_name"："foo"}は、構成エントリを含む試行を検索します

  実験名を「foo」に設定

  より複雑なクエリを作成するための操作を作成できます。

  言語のリファレンスは[https://docs.mongodb.com/manual/reference/operator/query](https://docs.mongodb.com/manual/reference/operator/query)にありますを参照してください。

* order str-順序はcreated\_at、heartbeat\_at、config。_。value、またはsummary\_metrics。_にすることができます。注文の前に+を付けると、注文が昇順になります。

  順序の前に-を付けると、順序は降順になります（デフォルト）。

  デフォルトの順序は、新しいものから古いものへのrun.created\_atです。

 **戻り値：**

Runsオブジェクト。これは、`Run`オブジェクトの反復可能なコレクションです。

 **試行**

```python
 | @normalize_exceptions
 | run(path="")
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L445)

entity/project/run\_idの形式でパスを解析することにより、単一の試行を返します。

 **引数：**

* `path` str-entity/project/run\_idの形式で試行するパス。api.entityが設定されている場合、これはproject/run\_idの形式にすることができ、api.projectが設定されている場合、これは単にrun\_idにすることができます。.

**戻り値：**

 `Run`オブジェクト。

 **スイープ**

```python
 | @normalize_exceptions
 | sweep(path="")
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L462)

entity/project/sweep\_idの形式でパスを解析してスイープを返します。

 **引数：**

* `path` _str_、オプション-entity / project/sweep\_idの形式でスイープするパス。api.entityが設定されている場合、これはproject/swap\_idの形式にすることができ、api.projectが設定されている場合、これは単にsweep\_idにすることができます。

 **戻り値：**

スイープオブジェクト。

**アーティファクト**

```python
 | @normalize_exceptions
 | artifact(name, type=None)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L496)

entity/project/run\_idの形式でパスを解析することにより、単一のアーティファクトを返します。

**引数：**

* name str -アーティファクト名。接頭辞としてエンティティ/プロジェクトを付けることができます。有効な名前は次の形式にすることができます。
* 名前：バージョン
* 名前：エイリアス
* ダイジェスト
* type str、オプション-フェッチするアーティファクトのタイプ。

 **戻り値：**

 アーティファクトオブジェクト。

###  プロジェクトオブジェクト

```python
class Projects(Paginator)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L616)

 `Project`オブジェクトの反復可能なコレクション。

###  プロジェクトオブジェクト

```python
class Project(Attrs)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L678)

プロジェクトは試行用の名前空間です

###  オブジェクトを試行します

```python
class Runs(Paginator)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L699)

 プロジェクトとオプションのフィルターに関連付けられた試行の反復可能なコレクション。これは通常、`Api.`runsメソッドを介して間接的に使用されます

###  オブジェクトの試行

```python
class Run(Attrs)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L804)

エンティティとプロジェクトに関連付けられた単一の試行。

 **属性：**

* `tags` _\[str\]_ - 試行に関連付けられたタグのリスト
* `url` _str_ - この試行のURL
* `id` _str_ - 試行のユニークの識別子（デフォルトは8文字）
* `name` _str_ - 試行の名前
* `state` _str_ - 試行中、終了、クラッシュ、中止のいずれか
* `config` _dict_ - 試行に関連付けられたハイパーパラメータのdict
* `created_at` _str_ - 試行が開始されたときのISOタイムスタンプ
* `system_metrics` _dict_ - 試行のために記録された最新のシステムメトリック
* `summary` _dict_ - 現在の要約を保持する可変のdictのようなプロパティ。

  updateを呼び出すと、変更が保持されます。

* `project` _str_ - 試行に関連付けられたプロジェクト
* `entity` _str_ - 試行に関連付けられたエンティティの名前
* `user` _str_ - 試行を作成したユーザーの名前
* `path` _str_ - ユニークの識別子\[entity\]/\[project\]/\[run\_id\]
* `notes` _str_ - 試行に関するメモ
* `read_only` _boolean_ - 試行が編集可能かどうか
* `history_keys` _str_ - ログに記録された履歴メトリックのキー

  wandb.log（{key：value}）を使用  

**\_\_init\_\_**

```python
 | __init__(client, entity, project, run_id, attrs={})
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L829)

試行は常にapi.runs（）を呼び出すことによって初期化されます。ここで、apiはwandb.Apiのインスタンスです。

**作成する**

```python
 | @classmethod
 | create(cls, api, run_id=None, project=None, entity=None)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L887)

 指定されたプロジェクトの試行を作成します

 **更新**

```python
 | @normalize_exceptions
 | update()
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L993)

試行オブジェクトへの変更をwandbバックエンドに永続化します。

**ファイル**

```python
 | @normalize_exceptions
 | files(names=[], per_page=50)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1070)

 **引数：**

* `names` _list_ - 空の場合はすべてのファイルを返す場合、要求されたファイルの名前
* `per_page` ページあたりの結果の数

 **戻り値：**

`File`オブジェクトのイテレータである`Files`オブジェクト。

 **ファイル**

```python
 | @normalize_exceptions
 | file(name)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1082)

 **引数：**

* `name` _str_ - 要求されたファイルの名前。

 **戻り値：**

name引数に一致するファイル。

**upload\_file**

```python
 | @normalize_exceptions
 | upload_file(path, root=".")
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1093)

**引数：**

* `path` _str_ - アップロードするファイルの名前。
* `root` _str_ - ファイルを保存するためのルートパス。つまり試行時にファイルを「my\_dir/file.txt」として保存し、現在「my\_dir」にいる場合は、rootを「../」に設定します。

**戻り値：**

A `File` matching the name argument. 

**history**

```python
 | @normalize_exceptions
 | history(samples=500, keys=None, x_axis="_step", pandas=True, stream="default")
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1116)

Returns sampled history metrics for a run. This is simpler and faster if you are ok with the history records being sampled.

**Arguments**:

* `samples` _int, optional_ - The number of samples to return
* `pandas` _bool, optional_ - Return a pandas dataframe
* `keys` _list, optional_ - Only return metrics for specific keys
* `x_axis` _str, optional_ - Use this metric as the xAxis defaults to \_step
* `stream` _str, optional_ - "default" for metrics, "system" for machine metrics

**Returns**:

If pandas=True returns a `pandas.DataFrame` of history metrics. If pandas=False returns a list of dicts of history metrics.

**scan\_history**

```python
 | @normalize_exceptions
 | scan_history(keys=None, page_size=1000, min_step=None, max_step=None)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1150)

Returns an iterable collection of all history records for a run.

**Example**:

Export all the loss values for an example run

```python
run = api.run("l2k2/examples-numpy-boston/i0wt6xua")
history = run.scan_history(keys=["Loss"])
losses = [row["Loss"] for row in history]
```

**Arguments**:

* `keys` _\[str\], optional_ - only fetch these keys, and only fetch rows that have all of keys defined.
* `page_size` _int, optional_ - size of pages to fetch from the api

**Returns**:

An iterable collection over history records \(dict\).

**use\_artifact**

```python
 | @normalize_exceptions
 | use_artifact(artifact)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1207)

Declare an artifact as an input to a run.

**Arguments**:

* `artifact` _`Artifact`_ - An artifact returned from

  `wandb.Api().artifact(name)`

**Returns**:

A `Artifact` object.

**log\_artifact**

```python
 | @normalize_exceptions
 | log_artifact(artifact, aliases=None)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1234)

Declare an artifact as output of a run.

**Arguments**:

* `artifact` _`Artifact`_ - An artifact returned from

  `wandb.Api().artifact(name)`

* `aliases` _list, optional_ - Aliases to apply to this artifact

**Returns**:

A `Artifact` object.

### Sweep Objects

```python
class Sweep(Attrs)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1314)

A set of runs associated with a sweep Instantiate with: api.sweep\(sweep\_path\)

**Attributes**:

* `runs` _`Runs`_ - list of runs
* `id` _str_ - sweep id
* `project` _str_ - name of project
* `config` _str_ - dictionary of sweep configuration

**best\_run**

```python
 | best_run(order=None)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1400)

Returns the best run sorted by the metric defined in config or the order passed in

**get**

```python
 | @classmethod
 | get(cls, client, entity=None, project=None, sid=None, withRuns=True, order=None, query=None, **kwargs)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1440)

Execute a query against the cloud backend

### Files Objects

```python
class Files(Paginator)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1495)

Files is an iterable collection of `File` objects.

### File Objects

```python
class File(object)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1561)

File is a class associated with a file saved by wandb.

**Attributes**:

* `name` _string_ - filename
* `url` _string_ - path to file
* `md5` _string_ - md5 of file
* `mimetype` _string_ - mimetype of file
* `updated_at` _string_ - timestamp of last update
* `size` _int_ - size of file in bytes

**download**

```python
 | @normalize_exceptions
 | @retriable(
 |         retry_timedelta=RETRY_TIMEDELTA,
 |         check_retry_fn=util.no_retry_auth,
 |         retryable_exceptions=(RetryError, requests.RequestException),
 |     )
 | download(root=".", replace=False)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1618)

Downloads a file previously saved by a run from the wandb server.

**Arguments**:

* `replace` _boolean_ - If `True`, download will overwrite a local file

  if it exists. Defaults to `False`.

* `root` _str_ - Local directory to save the file.  Defaults to ".".

**Raises**:

`ValueError` if file already exists and replace=False

### Reports Objects

```python
class Reports(Paginator)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1641)

Reports is an iterable collection of `BetaReport` objects.

### QueryGenerator Objects

```python
class QueryGenerator(object)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1721)

QueryGenerator is a helper object to write filters for runs

### BetaReport Objects

```python
class BetaReport(Attrs)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1818)

BetaReport is a class associated with reports created in wandb.

WARNING: this API will likely change in a future release

**Attributes**:

* `name` _string_ - report name
* `description` _string_ - report descirpiton;
* `user` _User_ - the user that created the report
* `spec` _dict_ - the spec off the report;
* `updated_at` _string_ - timestamp of last update

### ArtifactType Objects

```python
class ArtifactType(object)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L2282)

**collections**

```python
 | @normalize_exceptions
 | collections(per_page=50)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L2337)

Artifact collections

### ArtifactCollection Objects

```python
class ArtifactCollection(object)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L2352)

**versions**

```python
 | @normalize_exceptions
 | versions(per_page=50)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L2366)

Artifact versions

### Artifact Objects

```python
class Artifact(object)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L2381)

**delete**

```python
 | delete()
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L2534)

Delete artifact and it's files.

**get**

```python
 | get(name)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L2628)

Returns the wandb.Media resource stored in the artifact. Media can be stored in the artifact via Artifact\#add\(obj: wandbMedia, name: str\)\`

**Arguments**:

* `name` _str_ - name of resource.

**Returns**:

A `wandb.Media` which has been stored at `name`

**download**

```python
 | download(root=None)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L2663)

Download the artifact to dir specified by the 

**Arguments**:

* `root` _str, optional_ - directory to download artifact to. If None

  artifact will be downloaded to './artifacts//'

**Returns**:

The path to the downloaded contents.

**file**

```python
 | file(root=None)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L2702)

Download a single file artifact to dir specified by the 

**Arguments**:

* `root` _str, optional_ - directory to download artifact to. If None

  artifact will be downloaded to './artifacts//'

**Returns**:

The full path of the downloaded file

**save**

```python
 | @normalize_exceptions
 | save()
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L2737)

Persists artifact changes to the wandb backend.

**verify**

```python
 | verify(root=None)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L2776)

Verify an artifact by checksumming its downloaded contents.

Raises a ValueError if the verification fails. Does not verify downloaded reference files.

**Arguments**:

* `root` _str, optional_ - directory to download artifact to. If None

  artifact will be downloaded to './artifacts//'

### ArtifactVersions Objects

```python
class ArtifactVersions(Paginator)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L2930)

An iterable collection of artifact versions associated with a project and optional filter. This is generally used indirectly via the `Api`.artifact\_versions method

