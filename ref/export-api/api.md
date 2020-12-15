---
description: wandb.apis.public
---

# Public API Reference

## wandb.apis.public

​[\[소스\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1)​

### Api Objects

```python
class Api(object)
```

​[\[소스\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L181)​

wandb 서버의 쿼리\(querying\)에 사용됩니다.

 **예시**:

가장 일반적인 초기화 방법입니다.

```text
wandb.Api()
```

 **전달인자**:

* `overrides` _dict_ -  [https://api.wandb.ai](https://api.wandb.ai/) 이외의 wandb server를 사용하는 경우 `base_url`을 설정할 수 있습니다. 또한 `entity`, `project`, 및 `run`에 대한 기본값을 설정할 수 있습니다.

**flush**

```python
 | flush()
```

[\[소스\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L276)​​

 api 객체는 실행의 로컬 캐시를 유지합니다. 따라서 실행 상태가 스크립트를 수행하는 동안 변경될 수 있는 경우, `api.flush()`를 통해 로컬 캐시를 지워 실행과 연관된 최신 값을 얻을 수 있습니다.

**projects**

```python
 | projects(entity=None, per_page=200)
```

 ​[\[소스\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L338)​

주어진 개체\(entity\)에 대한 프로젝트를 가져옵니다.

 **전달인자**:

* `entity` _str_ - 요청된 개체의 이름. None인 경우, Api로 전달된 기본값 개체\(default entity\)로 돌아갑니다. 기본값 개체가 없는 경우, `ValueError`가 발생합니다..
* `per_page` _int_ - 쿼리 페이지네이션\(query pagination\)에 대한 페이지 사이즈를 설정합니다. None은 기본 사이즈를 사용합니다. 일반적으로 이를 변경할 필요가 없습니다.

 **반환**:

반복 가능한\(iterable\) `Project` 객체의 컬렉션인 `Project` 객체.

**reports**

```python
 | reports(path="", name=None, per_page=50)
```

[\[소스\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L360)​

주어진 프로젝트 경로에 대한 리포트를 가져옵니다.

경고: 이 api는 베타 버전이며, 향후 출시 시에 변경될 수 있습니다

**전달인자**:

* `path` _str_ - 리포트가 위치한 프로젝트 경로로, 다음의 형식이어야 합니다: "entity/project"
* `name` _str_ - 요청된 리포트의 선택적 이름
* `per_page` _int_ - 쿼리 페이지네이션\(query pagination\)에 대한 페이지 사이즈를 설정합니다. None은 기본 사이즈를 사용합니다. 일반적으로 이를 변경할 필요가 없습니다.

 **반환**:

반복 가능한\(iterable\) `BetaReport` 객체의 컬렉션인 `Reports` 객체.

**runs**

```python
 | runs(path="", filters={}, order="-created_at", per_page=50)
```

 [\[소스\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L393)​

`state`, `entity`, `createdAt`, etc. 제공된 필터와 일치하는 프로젝트에서 실행 세트를 반환합니다. `config.,` _`summary.`_, `entity`, `createdAt`, 등을 기준으로 필터링할 수 있습니다.

 **예시**:

“foo”로 설정된 my\_project config.experiment\_name에서 실행을 찾기

```text
api.runs(path="my_entity/my_project", {"config.experiment_name": "foo"})
```

“foo” 또는 “bar”로 설정된 my\_project config.experiment\_name에서 실행을 찾기

```text
api.runs(path="my_entity/my_project",
- `{"$or"` - [{"config.experiment_name": "foo"}, {"config.experiment_name": "bar"}]})
```

오름차순 손실별로 정렬된 my\_project에서 실행을 찾기

```text
api.runs(path="my_entity/my_project", {"order": "+summary_metrics.loss"})
```

**전달인자**:

* 리포트가 위치한 프로젝트 경로로, 다음의 형식이어야 합니다: "entity/project"
* `filters` _dict_ - MongoDB를 사용하는 특정 실행에 대한 쿼리. config.key, summary\_metrics.key, state, entity, createdAt, 등과 같은 실행 특성 별로 필터링 할 수 있습니다. 예: : {"config.experiment\_name": "foo"}은 실험 이름의 구성 엔트리\(config entry\)가 “foo”로 설정된 실행을 찾습니다. 연산\(operations\)을 구성하여 더 복잡한 쿼리를 생성할 수 있습니다. 언어에 대한 참조 사항은 다음에서 확인할 수 있습니다:[https://docs.mongodb.com/manual/reference/operator/query](https://docs.mongodb.com/manual/reference/operator/query)​
* `order` _str_ - 순서는 `created_at`, `heartbeat_at`, `config.*.value`, 또는summary\_metrics.\* 입니다.

  +를 순서에 접두어로 붙이는 경우, 오름차순입니다.

  +를 순서에 접두어로 붙이는 경우, 내림차순입니다 \(기본값\).

  기본 순서는 최신에서 오래된 순서로 run.created\_at입니다.

**반환**:

A `Runs` object, which is an iterable collection of `Run` objects. 반복 가능한\(iterable\) Runs 객체의 컬렉션인 Runs 객체.

**run**

```python
 | @normalize_exceptions
 | run(path="")
```

 [\[소스\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L445)​

entity/project/run\_id 형식의 경로를 파싱\(parsing\)하여 단일 실행을 반환합니다.

 **전달인자**:

* `path` _str_ - path str - entity/project/run\_id 형식의 실행 경로. api.entity가 설정된 경우, project/run\_id의 형식일 수 있으며, api.project가 설정된 경우 run\_id 일 수 있습니다.

**반환**:

Run 객체

**sweep**

```python
 | @normalize_exceptions
 | sweep(path="")
```

​[\[소스\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L462)​

entity/project/run\_id 형식의 경로를 파싱\(parsing\)하여 스윕을 반환합니다.

 **전달인자**:

* `path` _str, optional_ - entity/project/run\_id 형식의 경로. api.entity가 설정된 경우, project/sweep\_id의 형식일 수 있으며, api.project가 설정된 경우 sweep\_id 일 수 있습니다.

 **반환**:

 `Sweep` 객체

**artifact**

```python
 | @normalize_exceptions
 | artifact(name, type=None)
```

 ​[\[소스\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L496)​

entity/project/run\_id 형식의 경로를 파싱\(parsing\)하여 단일 아티팩트를 반환합니다.

 **전달인자**:

* `name` _str_ - 아티팩트 이름. 앞에 entity/project를 붙일 수 있습니다. 유효한 이름의 다음의 형식입니다: name:version, name:alias, digest
* `type` _str, optional_ - 가져올\(fetch\) 아티팩트 유형.

 **전달인자**:

`Artifact` 객체

### Projects Objects

```python
class Projects(Paginator)
```

 ​[\[소스\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L616)​

An iterable collection of `Project` objects. 반복 가능한\(iterable\) `Project` 객체의 컬렉션

### Project Objects

```python
class Project(Attrs)
```

 ​[\[소스\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L678)​

 프로젝트는 실행에 대한 이름 공간\(namespace\)입니다.

### Runs Objects

```python
class Runs(Paginator)
```

 ​[\[소스\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L699)​

 프로젝트 및 선택적 필터와 관련된 반복 가능한\(iterable\) 실행의 컬렉션. 일반적으로 Api.runs 방법을 통해 간접적으로 사용됩니다.

### Run Objects

```python
class Run(Attrs)
```

​[\[소스\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L804)​

개체 및 프로젝트와 연관된 단일 실행

 **속성**:

* `tags` _\[str\]_ - 실행과 연관된 태그 리스트
* `url` _str_ - 이 실행의 url
* `id` _str_ - 실행에 대한 고유 식별자\(identifier\) \(기본값으로 8자까지\)
* `name` _str_ - 실행 이름
* `state` _str_ - 다음 중 하나입니다: running\(실행 중\), finished\(완료됨\), crashed\(완료됨\), aborted\(중단됨\)
* `config` _dict_ - 실행과 연관된 초매개변수의 dict
* `created_at` _str_ - 실행이 시작 됐을때 ISO 타임스탬프
* `system_metrics` _dict_ - 실행에 대해 기록된 최신 시스템 메트릭
* `summary` _dict_ - 현재 요약을 포함하는 변경 가능한\(mutable\) dict-like 속성\(property\). 업데이트 호출은 변경 사항을 지속합니다.
* `project` _str_ - 실행과 연관된 프로젝트
* `entity` _str_ - 실행과 연관된 개체의 이름
* `user` _str_ - 실행을 생성한 유저의 이름
* `path` _str_ - 고유 식별자\(identifier\) \[entity\]/\[project\]/\[run\_id\]
* `notes` _str_ - 실행에 대한 메모
* `read_only` _boolean_ - 실행의 편집 가능 여부
* `history_keys` _str_ -  `wandb.log({key: value})`와 함께 로그된 히스토리 메트릭의 키

**\_\_init\_\_**

```python
 | __init__(client, entity, project, run_id, attrs={})
```

​[\[소스\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L829)​ 

실행은 항상 api.runs\(\)을 호출하여 초기화되며, 여기서 api는 wandb.Api의 인스턴스입니다.

**create**

```python
 | @classmethod
 | create(cls, api, run_id=None, project=None, entity=None)
```

 [\[소스\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L887)​

지정된 프로젝트에 대한 실행을 생성합니다

**update**

```python
 | @normalize_exceptions
 | update()
```

[\[소스\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L993)​

 wandb 백엔드에 대한 실행 객체의 변경 사항을 지속합니다.

**files**

```python
 | @normalize_exceptions
 | files(names=[], per_page=50)
```

  ​[\[소스\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1070)​

**전달인자**:

* `names` _list_ - 요청된 파일의 이름이며, 비어있는 경우 모든 파일을 반환합니다.
* `per_page` _int_ - 페이지 당 결과 개수

 **반환**:

`Files` 객체이며, `File` 객체에 대한 이터레이터\(iterator\)입니다.

**file**

```python
 | @normalize_exceptions
 | file(name)
```

[\[소스\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1082)

 **전달인자**:

* `name` _str_ - 요청된 파일의 이름.

 **반환**:

이름 전달인자와 일치하는 `File`

**upload\_file**

```python
 | @normalize_exceptions
 | upload_file(path, root=".")
```

 [\[소스\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1093)​

 **전달인자**:

* `path` _str_ - 업로드할 파일 이름
* `root` _str_ - 관련된 파일을 저장할 루트 경로. 즉, "my\_dir/file.txt"로 실행에 파일을 저장하고 현재 "my\_dir"에 있는 경우, 루트를 "../"로 설정할 수 있습니다.

 **반환**:

 이름 전달인자와 일치하는 `File`

**history**

```python
 | @normalize_exceptions
 | history(samples=500, keys=None, x_axis="_step", pandas=True, stream="default")
```

 [\[소스\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1116)​

 실행에 대하여 샘플링된 히스토리 메트릭을 반환합니다. 히스토리 기록이 샘플링돼도 괜찮은 경우 이는 더 간단하고 빠른 방법입니다.

 **전달인자**:

* `samples` _int, optional_ - 반환할 샘플 개수
* `pandas` _bool, optional_ - 판다스\(pandas\) 데이터프레임을 반환합니다
* `keys` _list, optional_ - 특정 키에 대한 메트릭만 반환합니다
* `x_axis` _str, optional_ - 이 메트릭을 xAxis로 사용합니다. 기본값은 \_step입니다.
* `stream` _str, optional_ - 메트릭의 경우 “default”, 머신 메트릭의 경우 “system”

**반환**:

 pandas=True가 히스토리 메트릭의 `pandas.DataFrame`를 반환하는 경우. pandas=False가 히스토리 메트릭의 dicts 리스트를 반환하는 경우.

**scan\_history**

```python
 | @normalize_exceptions
 | scan_history(keys=None, page_size=1000, min_step=None, max_step=None)
```

 ​[\[소스\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1150)​

실행에 대한 반복 가능한\(iterable\) 모든 히스토리 기록의 컬렉션을 반환합니다.

 **예시**:

예시 실행에 대한 모든 손실 값을 내보냅니다

```python
run = api.run("l2k2/examples-numpy-boston/i0wt6xua")
history = run.scan_history(keys=["Loss"])
losses = [row["Loss"] for row in history]
```

**전달인자**:

* `keys` _\[str\], optional_ - 이 키들만 가져오며, 모든 키가 정의된 행만 가져옵니다.
* `page_size` _int, optional_ - api에서 가져올 페이지의 사이즈

 **반환**:

A히스토리 기록 \(dict\)에 대한 반복 가능한 컬렉션

**use\_artifact**

```python
 | @normalize_exceptions
 | use_artifact(artifact)
```

 ​[\[소스\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1207)​

실행에 대한 입력으로 아티팩트를 선언\(declare\) 합니다.

 **전달인자**:

* `artifact` _`Artifact`_ - `wandb.Api().artifact(name)` 에서 반환된 아티팩트

 **반환**:

 `Artifact` 객체.

**log\_artifact**

```python
 | @normalize_exceptions
 | log_artifact(artifact, aliases=None)
```

​[\[소스\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1234)​ 

실행의 출력으로 아티팩트를 선언합니다.

 **전달인자**:

* `artifact` _`Artifact`_ - `wandb.Api().artifact(name)`에서 반환된 아티팩트
* `aliases` _list, optional_ - 이 아티팩트에 적용할 별칭\(aliases\)

**반환**:

 `Artifact` 객체

### Sweep Objects

```python
class Sweep(Attrs)
```

[\[소스\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1314)​

스윕과 연관된 일련의 실행으로 다음과 함께 인스턴트화합니다: api.sweep\(sweep\_path\)

 **속성**:

* `runs` _`Runs`_ - 실행의 리스트
* `id` _str_ - 스윕 id
* `project` _str_ - 프로젝트 이름
* `config` _str_ - 스윕 구성 사전

**best\_run**

```python
 | best_run(order=None)
```

[\[소스\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1400)​

성에 정의된 메트릭 또는 전달된 순서로 정렬된 최적의 실행을 반환합니다

**get**

```python
 | @classmethod
 | get(cls, client, entity=None, project=None, sid=None, withRuns=True, order=None, query=None, **kwargs)
```

[\[소스\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1440)​

클라우드 백엔드에 대한 쿼리를 수행합니다

### Files Objects

```python
class Files(Paginator)
```

[\[소스\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1495)

파일은 반복 가능한 `File` 객체의 컬렉션입니다.

### File Objects

```python
class File(object)
```

[\[소스\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1561)

파일은 wandb에 의해 저장된 파일과 연관된 클래스입니다.

 **속성**:

* `name` _string_ - 파일 이름
* `url` _string_ - 파일 경로
* `md5` _string_ - md5 of file
* `mimetype` _string_ - 파일의 mime 타입
* `updated_at` _string_ - 마지막 업데이트의 타임스탬프
* `size` _int_ - 파일 사이즈 \(바이트\)

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

[\[소스\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1618)

wandb 서버에서 실행에 의해 이전에 저장된 파일을 다운로드합니다.

 **전달인자**:

* `replace` _boolean_ - `True`인 경우, 다운로드는 존재하는 경우 로컬파일을 덮어 씁니다. 기본값은 `False`입니다.
* `root` _str_ - 파일을 저장할 로컬 디렉토리. 기본값은 “.”입니다.

**발생\(Raises\)**:

파일이 이미 존재하고 replace=False인 경우 `ValuError`

### Reports Objects

```python
class Reports(Paginator)
```

​[\[소스\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1641)​

리포트는 반복 가능한 `BetaRepor`t 객체의 컬렉션

### QueryGenerator Objects

```python
class QueryGenerator(object)
```

 ​[\[소스\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1721)​

QueryGenerator는 실행에 대한 필터를 작성하는 헬퍼\(helper\) 객체입니다.

### BetaReport Objects

```python
class BetaReport(Attrs)
```

​[\[소스\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1818)​

BetaReport는 wandb에 생성된 리포트와 연관된 클래스입니다.

경고: 이 API는 향후 출시 시에 변경될 수 있습니다

 **속성**:

* `name` _string_ - 리포트 이름
* `description` _string_ - 리포트 설명
* `user` _User_ - 리포트를 생성한 사용자
* `spec` _dict_ - 리포트의 스펙\(spec\)
* `updated_at` _string_ - 마지막 업데이트의 타임스탬프

### ArtifactType Objects

```python
class ArtifactType(object)
```

 [\[소스\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L2282)​

**collections**

```python
 | @normalize_exceptions
 | collections(per_page=50)
```

 [\[소스\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L2337) 

아티팩트 컬렉션

### ArtifactCollection Objects

```python
class ArtifactCollection(object)
```

[\[소스\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L2352)​

**versions**

```python
 | @normalize_exceptions
 | versions(per_page=50)
```

​[\[소스\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L2366)​

아티팩트 버전

### Artifact Objects

```python
class Artifact(object)
```

​[\[소스\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L2381)​

**delete**

```python
 | delete()
```

[\[소스\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L2534)​

아티팩트를 삭제하며 파일입니다.

**get**

```python
 | get(name)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L2628)

아티팩트에 저장된 wandb.Media 리소스를 반환합니다. Media을 Artifact\#add\(obj: wandbMedia, name: str\)\`를 통해 아티팩트에 저장할 수 있습니다.

**전달인자**:

* `name` _str_ - 리소스의 이름

 **반환**:

name에 저장되어 있는 `wandb.Media`

**download**

```python
 | download(root=None)
```

 ​[\[소스\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L2663)​

아티팩트를 the로 지정된 dir에 다운로드합니다.

 **전달인자**:

* `root` _str, optional_ - 아티팩트를 다운로드할 디렉토리. None인 경우, 아티팩트는 './artifacts//'로 다운로드됩니다.

 **반환**:

다운로드된 콘텐츠의 경로

**file**

```python
 | file(root=None)
```

[\[소스\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L2702)​

단일 파일 아티팩트를 the로 지정된 dir에 다운로드합니다.

 **전달인자**:

* `root` _str, optional_ - 아티팩트를 다운로드할 디렉토리. None인 경우, 아티팩트는 './artifacts//'로 다운로드됩니다

**반환**:

다운로드한 파일의 전체 경로

**save**

```python
 | @normalize_exceptions
 | save()
```

[\[소스\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L2737)​

wandb 백엔드에 대한 아티팩트 변경 사항을 지속합니다.

**verify**

```python
 | verify(root=None)
```

[\[소스\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L2776)​

아티팩트의 다운로드된 콘텐츠를 체크섬\(checksumming\)하여 아티팩트를 인증합니다.

인증에 실패한 경우 ValueError가 발생합니다. 다운로드된 참조 파일을 인증하지 않습니다.

 **전달인자**:

* `root` _str, optional_ - 아티팩트를 다운로드할 디렉토리. None인 경우, 아티팩트는 './artifacts//'로 다운로드됩니다

### ArtifactVersions Objects

```python
class ArtifactVersions(Paginator)
```

[\[소스\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L2930)

프로젝트 및 선택적 필터와 연관된 반복 가능한 아티팩트 버전의 컬렉션. 일반적으로 Api.artifact\_versions 방식을 통해 간접적으로 사용됩니다.

