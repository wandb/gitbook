# Init

​[​](https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_init.py#L526-L754)[**​**![https://www.tensorflow.org/images/GitHub-Mark-32px.png](https://lh4.googleusercontent.com/n_8hpOXv4v_e8Y7DfB246JG7v7BcJjI1DstK-HqE_jJoyk7z9f9ocbJdlc3AXzjrULo1jHxas1XDR-XeduBzO48im2-5gqgHt9KV3r-CJYbIkDaKwuk7PoB9ZeecLjae1Q1hasJXRljLDL31uw)GitHub에서 Source 확인하기](https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_init.py#L526-L754)**​**​

Start a new tracked run with `wandb.init()`.

```text
init(    job_type: Optional[str] = None,    dir=None,    config: Union[Dict, str, None] = None,    project: Optional[str] = None,    entity: Optional[str] = None,    reinit: bool = None,    tags: Optional[Sequence] = None,    group: Optional[str] = None,    name: Optional[str] = None,    notes: Optional[str] = None,    magic: Union[dict, str, bool] = None,    config_exclude_keys=None,    config_include_keys=None,    anonymous: Optional[str] = None,    mode: Optional[str] = None,    allow_val_change: Optional[bool] = None,    resume: Optional[Union[bool, str]] = None,    force: Optional[bool] = None,    tensorboard=None,    sync_tensorboard=None,    monitor_gym=None,    save_code=None,    id=None,    settings: Union[run.settings, Dict[str, Any], None] = None) -> Union[Run, Dummy, None]
```

`wandb.init()`를 사용하여 추적된 새로운 실행을 시작합니다. 

머신러닝 훈련 파이프라인에서 훈련 스크립트 및 평가 스크립트의 시작 부분에 `wandb.init()`를 추가할 수 있으며, 각 부분은 W&B에서 실행\(run\)으로 추적됩니다.

wandb.init\(\)는 새로운 백그라운드 프로세스를 생성하여 데이터를 실행에 로그하며, 또한 기본값으로 데이터를 wandb.ai에 동기화합니다. 따라서 `wandb.log()`로 데이터를 로그하기 전에 `wandb.init()`를 호출하여 실행을 시작합니다.

`wandb.init()`는 실행 객체\(run object\)를 반환하며, `wandb.run`을 사용해 실행 객체에 액세스할 수도 있습니다.  
****

| **전달인자** | ​ |
| :--- | :--- |
|  `project` | \(str,optional\) – 새 실행을 전송할 프로젝트의 이름. 프로젝트가 지정되지 않은 경우, 실행은 “Uncategorized”\(분류되지 않은\) 프로젝트에 위치합니다. |
|  `entity` |  entity\(개체\)는 실행을 전송할 팀 또는 사용자 이름입니다. 이 entity는 반드시 그곳에 실행을 전송하기 전에 존재해야 하며, 따라서 실행 로그를 시작하기 전에 UI에 계정 또는 팀을 생성하여야 합니다. entity를 지정하지 않은 경우, 실행은 default entity\(기본값 개체\)로 전송되며, 이는 일반적으로 사용자 이름입니다. “default location to create new projects”\(새로운 프로젝트를 생성할 기본 위치\)에서 \[Settings\]\(wandb.ai/settings\)의 default entity를 변경합니다. |
|  `config` |  \(dict, argparse, absl.flags, str, optional\) config는 wandb.config를 설정하며, 즉, 여러분의 작업에 입력을 저장하는 사전과 유사한 객체\(dictionary-like object\) 또는 데이터 처리 작업을 위한 모델 또는 설정에 대한 초매개변수를 설정합니다. 이 config는 실행을 그룹화, 필터링, 정렬할 수 있는 UI의 테이블에 나타납니다. 키의 이름에는 \`.\`이 포함되어서는 안되며, 값은 10MB 미만이어야 합니다. dict, argparse or absl.flags: 인 경우, 키 값 쌍을 wandb.config 객체에 로드합니다. str:인 경우, yaml 파일을 해당 이름 별로 검색하여 config를 해당 파일에서 wandb.config 객체로 로드합니다. |
|  `save_code` | \(bool,optional\) 메인 스크립트 또는 notebook을 W&B에 저장하려면 save\_code를 작동하시기 바랍니다. 실험 재현성 및 UI에서 실험 간 코드를 디핑\(diff\) 하는 데 유용합니다. 기본값으로 off이며, \[Settings\]\(wandb.ai/settings\)에서 기본 동작을 “on”으로 변경하실 수 있습니다. |
|  `group` |  \(str,optional\) 그룹을 지정하여 더 큰 실험으로 개별 실행을 구성합니다. 예를 들어, 교차 검증\(cross validation\) 수행 중이거나, 여러 실험 세트에 대하여 모델을 훈련 및 평가하는 여러 작업이 있을 수 있습니다. Group을 통해 실행을 하나의 전체로 구성할 수 있으며, UI에서 토글링하여 켜거나 끌 수 있습니다. 자세한 사향은 \[Grouping\]\(docs.wandb.com/library/grouping\)을 참조하시기 바랍니다. |
|  `job_type` | \(str,optional\) 실행의 유형을 지정하며, 이는 group을 사용하여 실행을 하나의 더 큰 실험으로 그룹화할 때 유용합니다. 예를 들어, tran\(훈련\) alc eval\(평가\)와 같은 작업 유형을 포함한 한 그룹에 다양한 작업이 있을 수 있습니다. 이를 설정하면 UI에서 유사한 실행을 간단하게 필터링 및 그룹화할 수 있음으로, 같은 유형을 비교할 수 있습니다. |
|  `tags` |  \(list,optional\) 스트링\(strings\)의 리스트로, UI에서 이 실행에 대한 태그 리스트를 덧붙입니다. Tags는 실행을 함께 구성하거나 “baseline” 또는 “production”과 같은 임시 레이블을 적용할 때 유용합니다. UI에서 쉽게 태그를 추가 및 제거할 수 있으며, 특정 태그와 함께 실행만 필터링할 수 있습니다. |
|  `name` |  \(str,optional\)이 실행에 대한 짧은 표시 이름으로, 이를 통해 UI에서 이 실행을 식별할 수 있습니다. 기본값으로 테이블에서부터 차트에 이르기까지 쉽게 실행을 상호참조 할 수 있도록 임의의 2단어 이름을 생성합니다. 이러한 실행 이름을 짧게 유지하면 차트 범례 및 테이블을 더욱 쉽게 판별할 수 있습니다. 초매개변수를 저장할 위치를 찾는 경우, config에 이를 저장하시기 바랍니다. |
|  `notes` | \(str,optional\) git의 -m commit\(-m 커밋\) 메시지와 같은 좀 더 상세한 실행의 설명입니다. 이는 이 실행을 실행했을 때 어떠한 작업을 수행하고 있었는지 기억하는 데 도움이 됩니다. |
|  `dir` |  ****\(str,optional\) 메타데이터가 저장될 디렉토리의 절대 경로입니다. 아티펙트에 download\(\)를 호출하는 경우, 다운로드된 파일이 저장되는 디렉토리입니다. 기본값은 ./wandb directory입니다. |
|  `sync_tensorboard` |  \(bool,optional\) 모든 TensorBoard 로그를 W&B에 복사할지 여부를 나타냅니다 \(기본값: False\). \[Tensorboard\]\([https://docs.wandb.com/integrations/tensorboard](https://docs.wandb.com/integrations/tensorboard)\) resume \(bool, str, optional\): 재개\(resuming\) 방식을 설정합니다. 옵션: "allow"\(허용\), "must"\(반드시 허용\), "never"\(허용 안 함\), "auto"\(자동\) 또는 None\(없음\). 기본값은 None입니다. Cases: - None \(default\): 새 실행이 이전 실행과 같은 ID를 갖는 경우, 이 실행은 해당 데이터를 덮어씁니다. - "auto" \(or True\): 이 머신의 이전 실행이 충돌한 경우, 자동으로 다시 시작합니다. 그렇지 않은 경우, 새 실행을 시작합니다. - "allow": ID가 init\(id="UNIQUE\_ID"\) 또는 WANDB\_RUN\_ID="UNIQUE\_ID"과 함께 설정되어 있고 이전 실행과 동일한 경우, wandb는 자동으로 해당 id와 함께 실행을 다시 시작합니다. 그렇지 않은 경우 wandb는 새 실행을 시작합니다. - "never": id가 init\(id="UNIQUE\_ID"\) 또는 WANDB\_RUN\_ID="UNIQUE\_ID"와 함께 설정되어 있고 이전 실행과 동일한 경우, wandb는 충돌하게 됩니다. - "must": id가 init\(id="UNIQUE\_ID"\) 또는 WANDB\_RUN\_ID="UNIQUE\_ID"와 함께 설정되어 있고 이전 실행과 동일한 경우, wandb는 자동으로 해당 id와 함께 실행을 다시 시작합니다. 그렇지 않은 경우 wandb는 충돌하게 됩니다. 더 자세한 사항은 [https://docs.wandb.com/library/advanced/resuming](https://docs.wandb.com/library/advanced/resuming)을 참조하시기 바랍니다.  |
|  `reinit` | \(bool,optional\) 동일 프로세스에서 여러 wandb.init\(\)를 허용합니다. \(기본값: False\) |
|  `magic` | \(bool, dict, or str, optional\)이 bool\(불\)은 추가적인 wandb code를 더할 필요 없이 실행의 기본 사항을 캡처하여, 스크립트의 자동 조직\(auto-instrument\)여부를 제어합니다. \(기본값: False\) 또한, dict, Jason 스트링 또는 yaml 파일 이름을 전달할 수도 있습니다. |
|  `config_exclude_keys` |  \(list,optional\) \`wandb.config\`에서 제외할 스트링 키 |
|  `config_include_keys` |  \(list,optional\) wandb.config에 포함할 스트링 키 |
|  `anonymous` | \(str,optional\) 익명 데이터 로깅을 제어합니다. 옵션: "never" \(default\): 실수로 익명 실행을 생성하지 않으시려면 실행을 추적하기 전에 W&B 계정을 연동하셔야 합니다. - "allow": 로그인한 사용자가 자신의 계정으로 실행을 추적할 수 있지만, W&B 계정 없이 스크립트를 실행한 사용자의 경우 UI에서 차트를 볼 수 있습니다. - "must": 가입한 사용자 계정 대신 익명 계정으로 실행을 전송합니다. |
|  `mode` | \(str,optional\) " online"\(온라인\), "offline"\(오프라인\) 또는 "disabled"\(비활성화\)이며, 기본값은 online입니다. |
|  `allow_val_change` | \(bool,optional\) 키를 한 번 설정한 후 config 값을 변경할 수 있는지 여부이며, 기본값으로 config 값을 덮어쓴 경우 예외가 발생합니다. 훈련 중에 여러 번 변화하는 learning\_rate와 같은 것을 추적하려는 경우, wandb.log\(\)를 대신 사용합니다. \(기본값: False \(스크립트\), True \(Jupyter\)\) |
|  `force` |  \(bool,optional\) True인 경우, W&B에 사용자가 로그인하지 않았다면, force는 스크립트를 충돌시킵니다. False인 경우 W&B에 사용자가 로그인하지 않았다면 스크립트를 오프라인 모드에서 실행되도록 허용합니다. \(기본값: False\) |
|  `sync_tensorboard` | \(bool,optional\) Tensorboard 또는 TensorboardX에서의 wandb 로그를 동기화하고 관련 이벤트 파일을 저장합니다. 기본값은 false입니다. |
|  `monitor_gym` |  \(bool,optional\) OpenAI Gym을 사용할 때 자동으로 환경의 비디오를 로그합니다. [https://docs.wandb.com/library/integrations/openai-gym](https://docs.wandb.com/library/integrations/openai-gym)을 참조하시기 바랍니다. |
|  `id` | 이 실행에 대한 고유한 ID로 재개\(resuming\)에 사용됩니다. 이는 프로젝트에서 고유한 것이야 하며, 실행을 삭제한 경우 ID를 재사용할 수 없습니다. 짧은 설명 이름의 경우 이름 영역을 사용하거나 여러 실행을 비교하기 위해 초매개변수를 저장하도록 구성\(config\)해야 합니다. ID에 특수 문자를 사용할 수 없습니다. [https://docs.wandb.com/library/resuming](https://docs.wandb.com/library/resuming)를 참조하시기 바랍니다. |

### **예시:**

**기본 사용법**

```text
wandb.init()
```

동일 스크립트에서 여러 실행 시작

```text
for x in range(10):    with wandb.init(project="my-projo") as run:        for y in range(100):            run.log({"metric": x+y})
```

| **발생\(Raises\)** | ​ |
| :--- | :--- |
|  `Exception` | 문제가 발생한 경우 |

| **반환** |
| :--- |
|   \`Run\` **객체**  |

