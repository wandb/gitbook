# Artifacts

## wandb.sdk.wandb\_artifacts

 [\[ソースを表示\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_artifacts.py#L2)

### アーティファクト

```python
class Artifact(object)
```

[ \[ソースを表示\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_artifacts.py#L69)

このアーティファクトをフェッチするために使用できる安定した名前。

**add**

```python
 | add(obj, name)
```

 [\[ソースを表示\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_artifacts.py#L229)

 `name`にあるアーティファクトにobjを追加します。アーティファクトをダウンロードした後、Artifact＃get`（name）`を使用してこのオブジェクトを取得できます。

**引数：**

* `obj` _wandb.Media_ - アーティファクトに保存するオブジェクト
* `name` _str_ -  保存するパス

**get\_added\_local\_path\_name**

```python
 | get_added_local_path_name(local_path)
```

 [\[ソースを表示\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_artifacts.py#L278)

local\_pathがすでにアーティファクトに追加されている場合は、その内部名を返します。

### ArtifactManifestV1 オブジェクト

```python
class ArtifactManifestV1(ArtifactManifest)
```

[ \[ソースを表示\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_artifacts.py#L328)

**to\_manifest\_json**

```python
 | to_manifest_json()
```

 [\[ソースを表示\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_artifacts.py#L368)

 これはwandb\_manifest.jsonに保存されているJSONです

include\_localがTrueの場合、ファイルへのローカルパスも含めます。これは、現在のシステムに保存されるのを待っているアーティファクトを表すために使用されます。アーティファクトマニフェストのコンテンツにローカルパスを含める必要はありません。

### TrackingHandler オブジェクト

```python
class TrackingHandler(StorageHandler)
```

 [\[ソースを表示\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_artifacts.py#L636)

**\_\_init\_\_**

```python
 | __init__(scheme=None)
```

 [\[ソースを表示\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_artifacts.py#L637)

変更や特別な処理を行わずに、パスをそのまま追跡します。追跡されるパスが標準化された場所にマウントされたファイルシステム上にある場合に役立ちます。

たとえば、追跡するデータが/dataにマウントされたNFS共有にある場合は、パスを追跡するだけで十分です。

### LocalFileHandler オブジェクト

```python
class LocalFileHandler(StorageHandler)
```

 [\[ソースを表示\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_artifacts.py#L682)

file：//参照を処理します

**\_\_init\_\_**

```python
 | __init__(scheme=None)
```

  [\[ソースを表示\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_artifacts.py#L686)

ローカルファイルシステム上のファイルまたはディレクトリを追跡します。ディレクトリが展開され、中に含まれる各ファイルのエントリが作成されます。

### WBArtifactHandler オブジェクト

```python
class WBArtifactHandler(StorageHandler)
```

  [\[ソースを表示\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_artifacts.py#L1172)

アーティファクト参照型ファイルのロードと保存を処理します

