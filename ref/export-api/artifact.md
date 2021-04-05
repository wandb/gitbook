# Artifact

  
​[​](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L2447-L3148)[![https://www.tensorflow.org/images/GitHub-Mark-32px.png](https://lh6.googleusercontent.com/SXVTjyUy1TYkFxx0zfwtaEyI1e-3GyFeuYZLFMinfY6JIEMyq94GMN-mVcyYy3M0mM0XYk9GO9EzUN4OWmT5HS1DzoL0W_USaH00lli5i76BqbNTEEbsJkF_6oNqa8Y4MQGn3dBS7J1cEailyA)**GitHub에서 소스 확인하기**](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L2447-L3148)**​**​

```text
Artifact(    client, entity, project, name, attrs=None)
```

| **속성** | ​ |
| :--- | :--- |
| `aliases` | ​ |
| `created_at` | ​ |
| `description` | ​ |
| `digest` | ​ |
| `id` | ​ |
| `manifest` | ​ |
| `metadata` | ​ |
| `name` | ​ |
| `size` | ​ |
| `state` | ​ |
| `type` | ​ |
| `updated_at` | ​ |

### **방법**

### `add_dir` <a id="add_dir"></a>

​ [소스 보기](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L2666-L2667)**​**​

```text
add_dir(    path, name=None)
```

### `add_file` <a id="add_file"></a>

​ [소스 보기](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L2663-L2664)**​**​

```text
add_file(    path, name=None)
```

### `add_reference` <a id="add_reference"></a>

​ [소스 보기](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L2669-L2670)**​**​

```text
add_reference(    path, name=None)
```

### `delete` <a id="delete"></a>

​ [소스 보기](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L2643-L2658)**​**​

```text
delete()
```

아티팩트 및 아티팩트 파일을 삭제합니다.

### `download` <a id="download"></a>

​ [소스 보기](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L2799-L2841)**​**​

```text
download(    root=None, recursive=False)
```

 the에 의해 지정된 dir로 단일 file 아티팩트를 다운로드합니다.

| **전달인자** |
| :--- |
| root \(str, optional\): 아티팩트를 다운로드할 티렉토리. None인 경우 아티팩트는 './artifacts//'에 다운로드됩니다.recursive \(bool, optional\): True로 설정된 경우, 모든 종속 아티팩트 또한 다운로드됩니다. False인 경우, 종속 아티팩트는 필요시에만 다운로드됩니다. |

| **반환** |
| :--- |
| 다운로드된 콘텐츠의 경로 |

### `expected_type` <a id="expected_type"></a>

​[소스 보기](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L2601-L2641)**​**​

```text
@staticmethodexpected_type(    client, name, entity_name, project_name)
```

 지정된 이름 및 프로젝트에 대한 예상 유형을 반환합니다

### `file` <a id="file"></a>

​ [소스 보기](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L2843-L2864)**​**​

```text
file(    root=None)
```

the에 의해 지정된 dir로 단일 file 아티팩트를 다운로드합니다.

| **전달인자** |
| :--- |
| root \(str, optional\): 아티팩트를 다운로드할 디렉토리. None인 경우 아티팩트는 './artifacts//'로 다운로드됩니다. |

| **반환** |
| :--- |
| 다운로드된 파일의 전체 경로 |

### `from_id` <a id="from_id"></a>

​ [소스 보기](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L2469-L2509)​

```text
@classmethodfrom_id(    artifact_id, client)
```

### `get` <a id="get"></a>

​ [소스 보기](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L2763-L2797)**​**​

```text
get(    name)
```

아티팩트에 저장된 wandb.Media 리소스를 반환합니다. Artifact\#add\(obj: wandbMedia, name: str\)\`를 통해 Media를 아티팩트에 저장할 수 있습니다.

| **반환** |
| :--- |
| \`name\`에 저장된 \`wandb.Media\` |

### `get_path` <a id="get_path"></a>

​[소스 보기](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L2692-L2761)**​**​

```text
get_path(    name)
```

### `logged_by` <a id="logged_by"></a>

​ [소스 보기](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L3115-L3148)**​**​

```text
logged_by()
```

이 아티팩트를 로그한 실행을 검색합니다.

| **반환** | ​ |
| :--- | :--- |
| `Run` | 이 아티팩트를 로그한 실행 객체 |

### `new_file` <a id="new_file"></a>

​ [소스 보기](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L2660-L2661)**​**​

```text
new_file(    name, mode=None)
```

### `save` <a id="save"></a>

​ [실행 객체](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L2877-L2915)**​**​

```text
save()
```

wandb 백엔드에 대한 아티팩트 변경사항을 지속합니다.

### `used_by` <a id="used_by"></a>

​ [소스 보기](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L3071-L3113)**​**​

```text
used_by()
```

이 아티팩트를 직접 사용하는 실행을 검색합니다.

| **반환** |
| :--- |
| \[Run\]: 이 아티팩트를 사용하는 실행 객체의 리스트 |

### `verify` <a id="verify"></a>

​[View source](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L2917-L2942)​

```text
verify(    root=None)
```

다운로드된 콘텐츠를 체크섬\(checksum\)하여 아티팩트를 확인합니다

확인에 실패하는 경우 ValueError가 발생합니다. 다운로드된 참조\(reference\) 파일을 확인하지 않습니다.

| **전달인자** |
| :--- |
| root \(str, optional\): 아티팩트를 다운로드할 디렉토리. None인 경우 아티팩트는 './artifacts//'로 다운로드됩니다. |

| **클래스 변수** | ​ |
| :--- | :--- |
| QUERY | ​ |

[  
](https://docs.wandb.ai/ref/public-api/projects)

