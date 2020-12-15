# Init

## wandb.sdk.wandb\_init

[\[view\_source\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_init.py#L3)

wandb.init\(\)를 통해 새롭게 추적된 실행을 시작합니다. ML 훈련 파이프라인에서 훈련 스크립트 및 평가 스크립트의 시작 부분에 wandb.init\(\)을 추가할 수 있으며, 각 부분은 W&B에서 실행으로 추적됩니다.

### \_WandbInit Objects

```python
class _WandbInit(object)
```

 ​[\[소스\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_init.py#L35)​

**setup**

```python
 | setup(kwargs)
```

[\[view\_source\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_init.py#L62)

wandb.init\(\)은 새로운 백그라운드 프로세스를 생성하여 데이터를 실행에 로그하며, 또한 기본값으로 데이터를 wandb.ai에 동기화합니다. 따라서 라이브 시각화를 확인하실 수 있습니다. wandb.log\(\)로 데이터를 로그하기 전에 wandb.init\(\)를 호출하여 실행을 시작합니다.

**init**

```python
init(job_type: Optional[str] = None, dir=None, config: Union[Dict, str, None] = None, project: Optional[str] = None, entity: Optional[str] = None, reinit: bool = None, tags: Optional[Sequence] = None, group: Optional[str] = None, name: Optional[str] = None, notes: Optional[str] = None, magic: Union[dict, str, bool] = None, config_exclude_keys=None, config_include_keys=None, anonymous: Optional[str] = None, mode: Optional[str] = None, allow_val_change: Optional[bool] = None, resume: Optional[Union[bool, str]] = None, force: Optional[bool] = None, tensorboard=None, sync_tensorboard=None, monitor_gym=None, save_code=None, id=None, settings: Union[Settings, Dict[str, Any], None] = None) -> Union[Run, Dummy]
```



 **전달인자**:

* `job_type` _str, optional_ - 새 실행을 보낼 프로젝트의 이름. 프로젝트가 지정되지 않은 경우, 실행은 “Uncategorized”\(분류되지 않은\) 프로젝트에 위치합니다.
* `magic` _bool, dict, or str, optional_ - 이 불\(bool\)은 더 많은 wandb 코드를 추가할 필요 없이 실행의 기본 사항을 캡처하여, 스크립트를 자동 조직\(auto-instrument\) 할지 여부를 제어합니다. \(기본값: False\) dict, json 스트링 또는 yaml 파일 이름을 전달할 수도 있습니다.
* `config_exclude_keys` _list, optional_ - wandb.config에서 제외할 스트링 키
* `config_include_keys` _list, optional_ - wandb.config에 포함할 스트링 키
* `anonymous` _str, optional_ - 익명 데이터 로깅을 제어합니다. 옵션:
* `mode` _str, optional_ - "online"\(온라인\), "offline"\(오프라인\) 또는 "disabled"\(비활성화\)입니다. 기본값은 online입니다.
* `allow_val_change` 로그인한 사용자가 자신의 계정으로 실행을 추적하게 허용하지만, W&B 계정 없이 스크립트를 실행한 사용자는 UI의 차트를 확인할 수 있습니다.
* `resume` _bool, str, optional_ - Sets the resuming behavior. Should be one of:

  "allow", "must", "never", "auto" or None. Defaults to None.

  Cases:

* "auto" \(or True\): automatically resume the previous run on the same machine.

  if the previous run crashed, otherwise starts a new run.

* "allow": 키를 한 번 설정한 후 config 값을 변경할 수 있는지 여부. 기본값으로, config 값을 덮어쓴 경우 예외가 발생합니다. 훈련 중에 여러 번 변화하는 learning\_rate와 같은 내용을 추적하려는 경우, wandb.log\(\)를 대신 사용합니다. \(기본값: False \(스크립트에서\), True \(Jupyter에서\)\)
* "never":  실행을 추적하기 전에 W&B 계정을 연동하여야 실수로 익명 실행을 생성하지 않습니다.
* "must": 가입한 사용자 계정 대신 익명 계정으로 실행을 전송합니다.
* None: never resumes - if a run has a duplicate run\_id the previous run is

  overwritten.

  See [https://docs.wandb.com/library/advanced/resuming](https://docs.wandb.com/library/advanced/resuming) for more detail.

* `force` _bool, optional_ -True인 경우, W&B에 사용자가 로그인하지 않은 경우 스크립트를 충돌시킵니다. False인 경우 W&B에 사용자가 로그인하지 않은 경우 스크립트를 오프라인 모드에서 실행되도록 허용합니다. \(기본값: False\)
* `sync_tensorboard` _bool, optional_ - Tensorboard 또는 TensorboardX에서의 wandb 로그를 동기화하고 관련 이벤트 파일을 저장합니다. 기본값은 false입니다.
* `monitor_gym` - OpenAI Gym을 사용할 때 자동으로 환경의 비디오를 로그합니다. [https://docs.wandb.com/library/integrations/openai-gym](https://docs.wandb.com/library/integrations/openai-gym)를 참조하시기 바랍니다.
* `save_code` _bool, optional_ - Save the entrypoint or jupyter session history

  source code.

* `id` _str, optional_ - 이 실행에 대한 고유한 ID로 재개\(resuming\)에 사용됩니다. 이는 프로젝트에서 고유한 것이야 하며, 실행을 삭제한 경우 ID를 재사용할 수 없습니다. 짧은 설명 이름의 경우 이름 영역을 사용하거나 여러 실행을 비교하기 위해 초매개변수를 저장하는 구성을 사용해야 합니다. ID에 특수 문자를 사용할 수 없습니다

 **예시**:

 기본 사용법

```text
wandb.init()
```

동일 스크립트에서 여러 실행 시작

```text
for x in range(10):
with wandb.init(project="my-projo") as run:
for y in range(100):
- `run.log({"metric"` - x+y})
```

**발생\(Raises\)**:

* `Exception` - 문제인 경우.

 **반환**:

 `Run` 객체

