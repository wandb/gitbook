# Sweep

​​[**​**![https://www.tensorflow.org/images/GitHub-Mark-32px.png](https://lh6.googleusercontent.com/o62r1EeGIEi77XXwlFRrmYcn-V_qfN0nERIzZ-_IqfJAXt5KnQca7S1ChgQWoTBKd8SIB4lUmYn7tnJt0E19TRGeJaWgkeUx49CFXt13qw-XD2fua0EQOcNE14wjpJXYurwvshmeOMahgLzoIg)GitHub에서 소스 확인하기](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L1356-L1534)**​**​

스윕\(sweep\)과 관련된 일련의 실행\(runs\)

```text
Sweep(    client, entity, project, sweep_id, attrs={})
```

#### **다음과 함께 인스턴스화 하십시오:** <a id="instantiate-with"></a>

api.sweep\(sweep\_path\)

| **특성** | ​ |
| :--- | :--- |
| `config` | ​ |
| `entity` | ​ |
| `order` | ​ |
| `path` | ​ |
| `url` | ​ |
| `username` | ​ |

##  **방법** <a id="methods"></a>

### `best_run` <a id="best_run"></a>

​ [소스 보기](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L1442-L1465)**​**​

```text
best_run(    order=None)
```

config에 정의된 메트릭 또는 전달된 순서에 따라 정렬된 최적의 실행을 반환합니다.

### `get` <a id="get"></a>

​ [소스 보기](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L1481-L1531)**​**​

```text
@classmethodget(    client, entity=None, project=None, sid=None, withRuns=True, order=None,    query=None, **kwargs)
```

클라우드 백엔드에 대하여 쿼리를 실행합니다

### `load` <a id="load"></a>

​ [소스 보기](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L1422-L1431)**​**​

```text
load(    force=False)
```

### `snake_to_camel` <a id="snake_to_camel"></a>

​ [소스 보기](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L528-L530)**​**​

```text
snake_to_camel(    string)
```

| **클래스 변수** | ​ |
| :--- | :--- |
| QUERY | ​ |

[  
](https://docs.wandb.ai/ref/public-api/runs)

