# Api

[![https://www.tensorflow.org/images/GitHub-Mark-32px.png](https://lh5.googleusercontent.com/V6Zm47X7i7O4jtrIgFXxOhFQxqXkqNt-MItR8QbTgYna2yuXePfNEvsSIFlxq1y6SxSyLOjXAA9mB7T6sujetFbFQdM7mooxL8ZaAjTNtzZxxjuVl0JlNdrdMzD5DJbhSWavcaXa-dd33NlaAA)GitHub에서 소스 확인하기](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L182-L521)**​**

wandb 서버를 쿼리하는 데 사용됩니다.

```text
Api(
    overrides={}
)
```

#### **예시:**

가장 일반적인 초기화 방법입니다

```text
>>> wandb.Api()
```

| **전달인자** |  |
| :--- | :--- |
| `overrides` | \(dict\) [https://api.wandb.ai](https://api.wandb.ai/)가 아닌 wandb 서버를 사용하는 경우 \`base\_url\`을 설정할 수 있습니다. 또한 \`entity\`, \`project\`, \`run\`에 대한 기본값을 설정할 수 있습니다. |

| **속성** |  |
| :--- | :--- |
| `api_key` |  |
| `client` |  |
| `default_entity` |  |
| `user_agent` |  |

## **방법**

### `artifact` <a id="artifact"></a>

 [소스 보기](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L500-L521)**​**

```text
artifact(
    name, type=None
)
```

`entity/project/run_id` 형식으로 경로를 분석\(parse\)하여 단일 아티팩트를 반환합니다.

| **전달인자** |  |
| :--- | :--- |
| `name` | \(str\) 아티팩트 이름입니다. entity/project가 앞에 붙어있을 수 있습니다. 유효한 이름은 다음의 형식일 수 있습니다: name:version name:alias digest |
| `type` | \(str, optional\) 가져올 아티팩트의 유형입니다. |

| **반환** |
| :--- |
| \`Artifact\` 객체 |

### `artifact_type` <a id="artifact_type"></a>

 [소스 보기](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L489-L492)**​**

```text
artifact_type(
    type_name, project=None
)
```

### `artifact_types` <a id="artifact_types"></a>

[소스 보기](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L484-L487)**​**

```text
artifact_types(
    project=None
)
```

### `artifact_versions` <a id="artifact_versions"></a>

[소스 보기](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L494-L498)**​**

```text
artifact_versions(
    type_name, name, per_page=50
)
```

### `create_run` <a id="create_run"></a>

 [소스 보기](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L244-L247)**​**

```text
create_run(
    **kwargs
)
```

### `flush` <a id="flush"></a>

 [소스 보기](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L275-L281)

```text
flush()
```

api 객체는 실행의 로컬 캐시를 유지합니다. 따라서 스크립트를 실행하는 동안 실행 상태가 변경될 수 있는 경우, 반드시 실행과 관련된 최신 값을 가져오려면 `api.flush()`와 함께 로컬 캐시를 지워야 합니다.

### `projects` <a id="projects"></a>

 [소스 보기](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L338-L360)**​**

```text
projects(
    entity=None, per_page=200
)
```

지정된 개체\(entity\)에 대한 projects를 가져옵니다.

| **전달인자** |  |
| :--- | :--- |
| `entity` | 요청된 개체\(entity\)의 이름입니다. None인 경우, \`Api\`로 전달된 기본 개체로 폴백\(fallback\)됩니다. 기본 개체가 없는 경우, \`ValueError\`가 발생합니다. |
| `per_page` | **\(int\) 쿼리 페이지 지정에 대한 페이지 크기를 설정합니다. None일 경우 기본 크기를 사용합니다. 일반적으로 이것을 변경할 이유가 없습니다.**  |

| **반환** |
| :--- |
| 반복 가능한 \`Project\` 객체의 모음인 \`Projects\` 객체 |

### `reports` <a id="reports"></a>

 [소스 보기](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L362-L393)**​**

```text
reports(
    path='', name=None, per_page=50
)
```

지정된 프로젝트 경로에 대한 리포트를 가져옵니다.

경고: 이 api는 베타 버전이며 향후 출시에서 변경될 수 있습니다.

| **전달인자** |  |
| :--- | :--- |
| `path` | \(str\) 리포트가 존재하는 프로젝트 경로는 다음과 같아야 합니다: "entity/project" |
| `name` | \(str\) 요청된 리포트의 선택적 이름입니다. |
| `per_page` | \(int\) 쿼리 페이지 지정\(pagination\)의 페이지 크기를 설정합니다. 기본 사이즈를 사용하지 않습니다. 보통 이것을 바꿀 필요가 없습니다. |

| **반환** |
| :--- |
| 반복 가능한 \`BetaReport\` 객체의 모음인 \`Report\` 객체 |

### `run` <a id="run"></a>

```text
run(
    path=''
)
```

entity/project/run\_id 형식으로 경로를 분석\(parse\)하여 단일 실행을 반환합니다.

| **전달인자** |  |
| :--- | :--- |
| `path` | \(str\) \`entity/project/run\_id\` 형식의 실행 경로입니다. api.entity가 설정된 경우, \`project/run\_id\` 형식이 될 수 있으며, \`api.project\`가 설정된 경우 단지 run\_id일 수 있습니다. |

| **반환** |
| :--- |
| \`Run\` 객체 |

### `runs` <a id="runs"></a>

 [소스 보기](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L395-L446)**​**

```text
runs(
    path='', filters={}, order='-created_at', per_page=50
)
```

 제공된 필터와 일치하는 프로젝트에서 실행 세트를 반환합니다.

`config.*`, `summary.*`, `state`, `entity`, `createdAt` 등으로 필터링할 수 있습니다.  


####  **예시:**

my\_project config.experiment\_name에서 실행 찾기가 "foo"로 실행 찾기 설정

```text
api.runs(path="my_entity/my_project", {"config.experiment_name": "foo"})
```

my\_project config.experiment\_name에서 실행 찾기가 "foo" 또는 "bar"로 실행 찾기 설정

```text
api.runs(path="my_entity/my_project",
    {"$or": [{"config.experiment_name": "foo"}, {"config.experiment_name": "bar"}]})
```

my\_project에서 손실 오름차순으로 정렬된 실행 찾기

```text
api.runs(path="my_entity/my_project", {"order": "+summary_metrics.loss"})
```

| **전달인자** |  |
| :--- | :--- |
| `path` | \(str\) : 프로젝트에 대한 경로로 "entity/project"과 같은 형식이어야 합니다. |
| `filters` | MongoDB 쿼리 언어를 사용하여 특정 실행을 쿼리합니다. config.key, summary\_metrics.key, state, entity, createdAt과 같은 속성을 실행 별로 필터링할 수 있습니다. 예를 들어 {"config.experiment\_name": "foo"}은 실험 이름의 구성 엔트리\(config entry\)가 "foo"로 설정된 실행을 찾습니다. 작업을 구성하여 보다 복잡한 쿼리를 만들 수도 있습니다. [https://docs.mongodb.com/manual/reference/operator/query](https://docs.mongodb.com/manual/reference/operator/query)에서 이 언어\(language\)에 대한 참조를 살펴보시기 바랍니다. |
| `order` | Order는 \`created\_at\`, \`heartbeat\_at\`, \`config.\*.value\`, \`summary\_metrics.\*\`일 수 있습니다. Order 앞에 a +를 붙이는 경우 order는 오름차순입니다. Order 앞에 a –를 붙이는 경우 order는 내림차순\(기본값\) 입니다. order의 기본값은 최신순에서 오래된 순으로 run.created\_at입니다. |

| **반환** |
| :--- |
| 반복 가능한 \`Run\` 객체의 모음인 \`Runs\` 객체 |

### `sweep` <a id="sweep"></a>

 [소스 보기](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L466-L482)**​**

```text
sweep(
    path=''
)
```

`entity/project/sweep_id` 형식으로 경로를 분석\(parse\)하여 스윕을 반환합니다.

| **전달인자** |  |
| :--- | :--- |
| `path` | \(str, optional\) entity/project/sweep\_id 형식의 스윕 경로. api.entity가 설정된 경우, project/sweep\_id 형식일 수 있으며, \`api.project\`가 설정된 경우 단지 sweep\_id일 수 있습니다. |

| **반환** |
| :--- |
| \`Sweep\` 객체 |

| **클래스 변수** |  |
| :--- | :--- |
| VIEWER\_QUERY |  |

