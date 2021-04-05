# Artifacts

## wandb.sdk.wandb\_artifacts

 [\[소스\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_artifacts.py#L2)​

### Artifact Objects

```python
class Artifact(object)
```

 ​[\[소스\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_artifacts.py#L69)​

파일 작성 및 log\_artifact에 전달할 수 있는 아티팩트 객체.

**add**

```python
 | add(obj, name)
```

 [\[소스\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_artifacts.py#L229)​

 `name`에 위치한 아티팩트에 `obj`를 추가합니다. 아티팩트를 다운로드한 후 Artifact\#get\(name\)을 사용하여 이 객체를 검색할 수 있습니다.

 **전달인자**:

* `obj` _wandb.Media_ - 아티팩트에 저장할 객체
* `name` _str_ - 저장할 경로

**get\_added\_local\_path\_name**

```python
 | get_added_local_path_name(local_path)
```

 ​[\[소스\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_artifacts.py#L278)​

 local\_path가 이미 아티팩트에 추가된 경우, 아티팩트의 내부 이름을 반환합니다.

### ArtifactManifestV1 Objects

```python
class ArtifactManifestV1(ArtifactManifest)
```

 ​[\[소스\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_artifacts.py#L328)​

**to\_manifest\_json**

```python
 | to_manifest_json()
```

 [\[소스\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_artifacts.py#L368)​

 wandb\_manifest.json에 저장된 JSON입니다.

include\_local이 True인 경우, 파일에 대한 로컬 경로 또한 포함합니다. 이것은 현재 시스템에 저장 대기 중인 이티팩트를 나타내는 데 사용됩니다. 아티팩트 매니페스트 콘텐츠\(artifact manifest contents\)에 로컬 경로를 포함할 필요가 없습니다.

### TrackingHandler Objects

```python
class TrackingHandler(StorageHandler)
```

 ​[\[소스\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_artifacts.py#L636)​

**\_\_init\_\_**

```python
 | __init__(scheme=None)
```

 [\[소스\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_artifacts.py#L637)​

 수정 또는 특별한 프로세싱 없이 경로를 있는 그대로 추적합니다. 추적중인 경로가 표준화된 위치에 마운트된 파일 시스템에 있는 경우에 유용합니다.

예를 들어, 추적할 데이터가 /data 에 마운트 된 NFS share에 있는 경우, 경로만 추적하는 것으로 충분합니다.

### LocalFileHandler Objects

```python
class LocalFileHandler(StorageHandler)
```

 ​[\[소스\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_artifacts.py#L682)​

Handles file:// references

**\_\_init\_\_**

```python
 | __init__(scheme=None)
```

 ​[\[소스\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_artifacts.py#L686)​ 

로컬 파일시스템의 파일 또는 디렉토리를 추적합니다. 디렉토리는 내부에 포함된 각 파일에 대한 엔트리 생성을 위해 확장됩니다.

### WBArtifactHandler Objects

```python
class WBArtifactHandler(StorageHandler)
```

​[\[소스\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_artifacts.py#L1172)​

아티팩트 참조 유형 파일 로딩 및 저장을 처리합니다

