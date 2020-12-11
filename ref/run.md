# Run

## wandb.sdk.wandb\_run

​[\[소스\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L4)​

### Run Objects

```python
class Run(object)
```

​[\[소스\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L131)​

 실행 객체\(run object\)는 단일 스크립트 수행에 해당하며, 일반적으로 ML 실험입니다. wandb.init\(\)로 실행을 생성합니다.

실행 객체\(run object\)는 단일 스크립트 수행에 해당하며, 일반적으로 ML 실험입니다. wandb.init\(\)로 실행을 생성합니다.

분산 훈련에서 wandb.init\(\)를 사용하여 각 프로세스에 대한 실행을 생성하고, 실행을 더 큰 실험으로 구성하기 위해 그룹 전달인자를 설정합니다.

**Attributes**:

* `history` _`History`_ - – 시계열 값으로, wandb.log\(\)와 함께 생성됩니다. 히스토리는 여러 단계에 걸친 스칼라 값, 리치 미디어\(rich media\) 및 사용자 정의 플롯이 포함될 수 있습니다.
* `summary` _`Summary`_ - 각 wandb.log\(\) 키에 대해 설정된 단일 값. 기본값으로 요약\(summary\)은 마지막으로 로그된 값으로 설정됩니다. 요약을 수동으로 최종 값 대신 최대 정확도와 같은 최적 값으로 설정할 수 있습니다.

**dir**

```python
 | @property
 | dir()
```

[\[view\_source\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_run.py#L334)

str: 실행과 연관된 모든 파일이 위치하는 디렉토리

**config**

```python
 | @property
 | config()
```

[\[view\_source\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_run.py#L341)

\(`Config`\): 실행의 초매개변수와 연관된 키 값 쌍의 실행 객체\(config object\) \(중첩 dict와 유사\)

**name**

```python
 | @property
 | name()
```

[\[소스\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L351)​

str: 실행의 표시이름. 고유한 이름일 필요 없으며, 이상적으로 서술적입니다

**notes**

```python
 | @property
 | notes()
```

[\[view\_source\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_run.py#L368)

str: 실행과 연관된 노트. 노트는 여러 줄의 스트링 일 수 있으며 또한 ${x}와 같은 $$내의 마크다운 및 라텍스 방정식을 사용할 수 있습니다.

**tags**

```python
 | @property
 | tags() -> Optional[Tuple]
```

[\[view\_source\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_run.py#L384)

Tuple\[str\]: 실행과 연관된 태그

**id**

```python
 | @property
 | id()
```

[\[view\_source\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_run.py#L398)

str: 실행과 연관된 run\_id

**sweep\_id**

```python
 | @property
 | sweep_id()
```

​[\[소스\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L402)

\(str, optional\): 실행과 연관된 스윕 id 또는 None

**path**

```python
 | @property
 | path()
```

 ​[\[소스\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L409)​

str: 실행 \[entity\]/\[project\]/\[run\_id\] 으로의 경로

**start\_time**

```python
 | @property
 | start_time()
```

[\[view\_source\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_run.py#L419)

int: the unix time stamp in seconds when the run started

**starting\_step**

```python
 | @property
 | starting_step()
```

 [\[소스\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L418)​

실행이 시작되었을 때 유닉스 시간 스탬프 \(초\)

**resumed**

```python
 | @property
 | resumed()
```

[\[소스\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L426)​

bool: 실행이 재개되었는지 여부

**step**

```python
 | @property
 | step()
```

​[\[소스\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L442)​

int: 스탭 카운터

wandb.log\(\)를 호출할 때마다, 기본값으로, 스텝 카운터를 증가합니다.

**mode**

```python
 | @property
 | mode()
```

[\[소스\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L455)

0.9x 및 이전 버전과의 호환을 위해, 결국 더 이상 사용되지 않습니다.

**group**

```python
 | @property
 | group()
```

 [\[소스\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L468)​

str: 실행과 연관된 W&B 그룹 이름

그룹을 설정하면 W&B UI가 합리적인 방식으로 정리하는 데 도움이 됩니다.

 분산 훈련을 수행하는 경우, 훈련의 모든 실행에 동일 그룹을 제공해야 합니다. 교차 검증을 수행하는 경우, 모든 교차 검증 겹\(corssvalidation folds\)에 동일 그룹을 제공해야 합니다.

**project**

```python
 | @property
 | project()
```

[\[view\_source\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_run.py#L484)

str: 실행과 연관된 W&B 프로젝트 이름.

**get\_url**

```python
 | get_url()
```

[\[view\_source\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_run.py#L488)

Returns: \(str, optional\): W&B 실행 대한 URL 또는 실행이 오프라인인 경우 None

**get\_project\_url**

```python
 | get_project_url()
```

 [\[소스\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L499)​

Returns: \(str, optional\): 실행과 연관된 W&B 프로젝트에 대한 URL 또는 실행이 오프라인인 경우 None

**get\_sweep\_url**

```python
 | get_sweep_url()
```

 ​[\[소스\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L507)​

Returns: \(str, optional\): 실행과 연관된 스윕에 대한 URL 또는 연관된 스윕이 없거나 실행이 오프라인인 경우 None.

**url**

```python
 | @property
 | url()
```

[\[view\_source\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_run.py#L513)

str: 실행과 연관된 W&B URL의 이름.

**entity**

```python
 | @property
 | entity()
```

​[\[소스\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L521)​

str: 실행과 연관된 W&B 개체의 이름. 개체는 사용자 이름 또는 기관 이름입니다.

**log**

```python
 | log(data, step=None, commit=None, sync=None)
```

[\[소스\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L675)​

글로벌 실행 히스토리에 dict를 로그합니다.

스칼라, 히스토그램, 미디어 및 matplotlib 플롯에 이르는 모든 것을 기록하기 위해 wandb.log를 사용할 수 있습니다.

가장 기본적인 용도는 wandb.log\({'train-loss': 0.5, 'accuracy': 0.9}\)입니다. train-loss=0.5 및accuracy=0.9로 실행과 연관된 히스토리 행에 저장됩니다. 히스토리 값은 app.wandb.ai 또는 로컬 서버에 작성될 수 있으며, 또한 wandb API를 통해서도 히스토리 값을 다운로드할 수 있습니다.

값을 로깅하면 로그된 메트릭에 대한 요약 값이 업데이트됩니다. 요약 값은 app.wandb.ai 또는 로컬 서버의 실행 테이블에 나타납니다. 요약 값은 수동으로 wandb.run.summary\["accuracy"\] = 0.9 와 같이 설정되는 경우, wandb.log는 더 이상 자동으로 실행의 정확도를 업데이트하지 않습니다.

로깅 값은 스칼라일 필요가 없습니다. 모든 wandb 객체 로깅이 지원됩니다.예컨대, wandb.log\({"example": wandb.Image\("myimage.jpg"\)}\)는 wandb UI에 깔끔하게 표시되는 예시 이미지를 로그합니다. 다른 모든 지원되는 유형의 경우 [https://docs.wandb.com/library/reference/data\_types](https://docs.wandb.com/library/reference/data_types)를 참조하시기 바랍니다.

 중첩된 메트릭 로깅을 권장하며, wandb API에 지원됩니다. 따라서 wandb.log\({'dataset-1': {'acc': 0.9, 'loss': 0.3} ,'dataset-2': {'acc': 0.8, 'loss': 0.2}}\)를 통해서 많은 정확도 값을 로그할 수 있으며, 메트릭은 wandb UI에 구성됩니다.

W&B는 글로벌 단계를 추적하므로 관련 메트릭을 함께 로그하는 것이 권장됩니다. 따라서, 기본값으로 wandb.log가 호출될 때마다, 글로벌 단계가 증가됩니다.

wandb.log를 초당 몇 번 이상 호출할 수 없습니다. 보다 자주 로그 하려는 경우, 클라이언트 측에서 데이터를 종합하는 것이 좋으며, 그렇지 않은 경우 퍼포먼스가 저하될 수 있습니다.

 **전달인자**

* `row` _dict, optional_ - 직렬화 가능 Python 객체의 dict. 즉, str,

  ints, floats, Tensors, dicts, 또는 wandb.data\_types

* `commit` _boolean, optional_ - wandb 서버에 메트릭 dict를 저장하고 단계를 증가시킵니다. false wandb.log가 현재 메트릭 dict을 행 전달인자와 함께 업데이트하는 경우, wandb.log를 commit=True와 함께 호출할 때까지 메트릭은 저장되지 않습니다.
* `step` _integer, optional_ - 프로세싱의 글로벌 단계. 이는 커밋되지 않은 이전 단계를 유지하지만 기본값으로 지정된 단계를 커밋하지 않습니다.
* `sync` _boolean, True_ - 이 전달인자는 사용되지 않으며, 현재 wandb.log의 행위를 변경하지 않습니다.

**예시**:

 기본 사용법

```text
- `wandb.log({'accuracy'` - 0.9, 'epoch': 5})
```

점진적 로깅

```text
- `wandb.log({'loss'` - 0.2}, commit=False)
# Somewhere else when I'm ready to report this step:
- `wandb.log({'accuracy'` - 0.8})
```

 히스토그램

```text
- `wandb.log({"gradients"` - wandb.Histogram(numpy_array_or_sequence)})
```

 이미지

```text
- `wandb.log({"examples"` - [wandb.Image(numpy_array_or_pil, caption="Label")]})
```

비디오

```text
- `wandb.log({"video"` - wandb.Video(numpy_array_or_video_path, fps=4,
format="gif")})
```

Matplotlib 플롯

```text
- `wandb.log({"chart"` - plt})
```

PR 곡선

```text
- `wandb.log({'pr'` - wandb.plots.precision_recall(y_test, y_probas, labels)})
```

3D 객체

```text
wandb.log({"generated_samples":
[wandb.Object3D(open("sample.obj")),
wandb.Object3D(open("sample.gltf")),
wandb.Object3D(open("sample.glb"))]})
```

더 자세한 예시는, [https://docs.wandb.com/library/log](https://docs.wandb.com/library/log)를 참조하시기 바랍니다

**발생\(Raises\)**:

wandb.Error - wandb.init이전에 호출된 경우 ValueError – 유효하지 않은 데이터가 전달된 경우

 **저장**

```python
 | save(glob_str: Optional[str] = None, base_path: Optional[str] = None, policy: str = "live")
```

 [\[소스\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L810)​

glob\_str와 일치하는 모든 파일이 wandb에 지정된 정책과 함께 동기화 되는지 확인합니다.

**전달인자**:

* `glob_str` _string_ - unix glob의 상대 또는 절대 경로 또는 일반 경로. 지정되지 않은 경우, 이 방법은 noop입니다.
* `base_path` _string_ - glob를 실행하는 기본 경로
* `policy` _string_ - “live”, “now”, 또는 “end” 중 하나
* `live` - 파일이 변경 될 때 업로드하고, 이전 파일을 덮어씁니다: 파일을 지금 한 번만 업로드합니다.
* `end` - 실행이 종료될 때만 파일을 업로드 합니다.

**finish**

```python
 | finish(exit_code=None)
```

 ​[\[소스\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L906)​

실행을 완료로 표시하고, 모든 데이터의 업로드를 완료합니다. 동일 프로세스에서 여러 실행을 생성할 때 사용됩니다. 스크립트가 종료되면 자동으로 이 방법을 호출합니다.

**join**

```python
 | join(exit_code=None)
```

​[\[소스\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L920)​

finish\(\)를 위해서 사용되지 않는 별칭\(alias\) – finish를 사용하십시오

**plot\_table**

```python
 | plot_table(vega_spec_name, data_table, fields, string_fields=None)
```

 ​[\[소스\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L924)​

 테이블에 사용자 정의 플롯을 생성합니다.

 **전달인자**:

* `vega_spec_name` - 플롯에 대한 스펙\(spec\)의 이름
* `table_key` - 데이터 테이블 로그에 사용되는 키
* `data_table` - 데이터를 포함하는 wandb.Table 객체로 시각화에 사용됨
* `fields` - 테이블 키에서 사용자 정의 시각화가 필요한 영역에 이르는 dict 매핑
* `string_fields` - 사용자 지정 시각화에 필요한 모든 문자열 상수에 대한 값을 제공하는 dict

**use\_artifact**

```python
 | use_artifact(artifact_or_name, type=None, aliases=None)
```

 [\[소스\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L1566)​

 아티팩트를 실행에 대한 입력으로 선언\(declare\)하고 반환된 객체에 대한 다운로드 또는 파일을 호출하여 로컬로 컨텐츠를 가져옵니다.

 **전달인자**:

* `artifact_or_name` _str or Artifact_ - 아티팩트 이름.

  entity/project를 앞에 붙일 수 있습니다. 유효한 이름은 다음과 같은 형식입니다:

  can be in the following forms:

  name:version

  name:alias

  digest

  또한 wandb.Artifact를 호출하여 생성된 아티팩트 객체를 전달할 수도 있습니다.

* `type` _str, optional_ - 사용할 아티팩트의 유형.
* `aliases` _list, optional_ - 이 아티팩트에 적용할 별칭

**Returns**:

`Artifact` 객체.

**log\_artifact**

```python
 | log_artifact(artifact_or_path, name=None, type=None, aliases=None)
```

​[\[소스\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L1621)​

 실행의 출력으로 아티팩트를 선언\(declare\)합니다.

**Arguments**:

* `artifact_or_path` _str or Artifact_ - 이 아티팩트의 콘텐츠에 대한 경로.

  다음과 같은 형식입니다:

  /local/directory

  /local/directory/file.txt

  s3://bucket/path

  `wandb.Artifact`를 호출하여 생성된 아티팩트 객체를 전달할 수도 있습니다.

* `name` _str, optional_ - 아티팩트 이름. entity/project를 앞에 붙일 수 있습니다.

  유효한 이름은 다음과 같은 형식입니다:

  name:version

  name:alias

  digest

  이 값이 지정되지 않은 경우 현재 실행 ID를 경로의 기본이름 앞에 추가하도록 기본 값으로 설정됩니다.

* `type` _str_ - 로그할 아티팩트의 유형. 예를 들어 “dataset”, “model”를 포합합니다.
* `aliases` _list, optional_ - 이 아티팩트에 적용할 별칭. 기본값은 \["latest"\]입니다.

**Returns**:

Artifact 객체

**alert**

```python
 | alert(title, text, level=None, wait_duration=None)
```

​[\[소스\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L1675)​

 지정된 제목과 텍스트로 경보를 실행합니다.

 **전달인자:**

* `title` _str_ - 경보의 제목으로, 64자 미만이어야 합니다
* `text` _str_ - 경보의 텍스트 본문
* `level` _str or wandb.AlertLevel, optional_ - 사용할 경보의 수준으로, "INFO", "WARN", 또는 "ERROR" 중 하나입니다
* `wait_duration` _int, float, or timedelta, optional_ - 이 제목의 다른 경보를 보내기 전 대기 시간 \(초\).

