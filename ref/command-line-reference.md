# Command Line Reference

wandb 

**사용법**

`wandb [OPTIONS] COMMAND [ARGS]...`

**옵션**

| **옵션** | **설명** |
| :--- | :--- |
| --version | 버전을 표시하고 종료합니다. |
| --help | 이 메시지를 표시하고 종료합니다. |

 **명령**

| **명령** | **설명** |
| :--- | :--- |
| agent | W&B 에이전트를 실행합니다. |
| artifact | 아티팩트와 상호작용하는 명령입니다. |
| controller | W&B 로컬 스윕 컨트롤러를 실행합니다 |
| disabled | W&B를 비활성화합니다. |
| docker | 도커\(docker\)를 사용하면 도커 이미지에 코드를 실행할 수 있습니다. |
| docker-run | W&B 환경을 설정하는 도커 실행에 대한 간단한 래퍼\(wrapper\)입니다. |
| enabled | W&B를 활성화합니다. |
| init | Weights & Biases를 통해 디렉토리를 구성합니다. |
| local | 로컬 W&B 컨테이너를 실행합니다. \(실험 중\) |
| login | Weights & Biases에 로그인합니다. |
| offline | W&B 동기화를 비활성화합니다. |
| online | W&B 동기화를 활성화합니다. |
| pull | Weights & Biases에서 파일을 가져옵니다. |
| restore | 실행에 대한 코드, 구성\(config\) 및 도커\(docker\) 상태를 복원합니다. |
| status | 구성\(configuration\) 설정을 표시합니다. |
| sweep | 스윕을 생성합니다. |
| sync | 오프라인 훈련 디렉토리를 W&B에 업로드합니다. |

## wandb agent

**사용법**

`wandb agent [OPTIONS] SWEEP_ID`

**요약**

W&B 에이전트를 실행합니다.

**옵션**

| **옵션** | **설명** |
| :--- | :--- |
| -p, --project | 스윕의 프로젝트입니다. |
| -e, --entity | 프로젝트에 대한 개체 범위\(entity scope\)입니다. |
| --count | 이 에이전트에 대한 실행의 최대 수입니다. |
| --help | 이 메시지를 표시하고 종료합니다. |

## wandb artifact

**사용법**

`wandb artifact [OPTIONS] COMMAND [ARGS]...`

**요약**

 아티팩트와 상호작용하는 명령

 **옵션**

| **옵션** | **설명** |
| :--- | :--- |
| --help | 이 메시지를 표시하고 종료합니다. |

### wandb artifact get

**사용법**

`wandb artifact get [OPTIONS] PATH`

**요약**

wandb에서 아티팩트를 다운로드합니다.

**옵션**

| **옵션** | **설명** |
| :--- | :--- |
| --root | 아티팩트를 다운로드할 디렉토리입니다. |
| --type | 다운로드하는 아티팩트의 유형입니다. |
| --help | 이 메시지를 표시하고 종료합니다. |

### wandb artifact ls

**사용법**

`wandb artifact ls [OPTIONS] PATH`

**요약**

wandb 프로젝트의 모든 아티팩트의 리스트를 작성합니다.

**옵션**

| **옵션** | **설명** |
| :--- | :--- |
| -t, --type | 리스트로 작성할 아티팩트의 유형입니다. |
| --help | 이 메시지를 표시하고 종료합니다. |

### wandb artifact put

**사용법**

`wandb artifact put [OPTIONS] PATH`

**요약**

아티팩트를 wandb에 업로드합니다.

 **옵션**

| **옵션** | **설명** |
| :--- | :--- |
| -n, --name | 푸시\(push\) 할 아티팩트의 이름: |
| -d, --description | 이 아티팩트에 대한 설명입니다. |
| -t, --type | 아티팩트의 유형입니다. |
| -a, --alias | 이 아티팩트에 적용할 별칭\(alias\)입니다. |
| --help | 이 메시지를 표시하고 종료합니다. |

## wandb controller

**사용법**

`wandb controller [OPTIONS] SWEEP_ID`

**요약**

W&B 로컬 스윕 컨트롤러를 실행합니다.

**옵션**

| **옵션** | **설명** |
| :--- | :--- |
| --verbose | 장황한\(Verbose\) 출력을 표시합니다. |
| --help | 이 메시지를 표시하고 종료합니다. |

## wandb disabled

#### **사용법**

`wandb disabled [OPTIONS]`

**요약**

W&B를 비활성화합니다.

**옵션**

| **옵션** | **설명** |
| :--- | :--- |
| --help | 이 메시지를 표시하고 종료합니다. |

## wandb docker

**사용법**

`wandb docker [OPTIONS] [DOCKER_RUN_ARGS]... [DOCKER_IMAGE]`

**요약**

W&B 도커를 사용하면 wandb가 구성되었는지를 확인하는 도커 이미지에서 코드를 실행할 수 있습니다. WANDB\_DOCKER and WANDB\_API\_KEY 환경변수를 컨테이너에 추가하고, 현재 디렉토리를 /app에 기본값으로 마운트 합니다. 이미지 이름이 선언\(declare\)되기 전에 docker run에 추가될 추가 args를 전달할 수 있으며, 전달되지 않은 경우, 저희는 기본 이미지를 선택합니다.

wandb docker -v /mnt/dataset:/app/data wandb docker gcr.io/kubeflow- images-public/tensorflow-1.12.0-notebook-cpu:v0.4.0 --jupyter wandb docker wandb/deepo:keras-gpu --no-tty --cmd "python train.py --epochs=5"

기본값으로, 진입점\(entrypoint\)을 오버라이드 하여 wandb의 존재를 확인하고 없는 경우 wandb를 설치합니다. --jupyter flag를 전달하는 경우, jupyter가 설치되었는지 확인하고 port 8888에 jupyter lab을 시작합니다. 시스템에서 nvidia-docker를 감지한 경우, nvidia 런타임을 사용합니다. wandb가 환경 변수를 기존의 도커 실행 명령으로 설정하도록 하려는 경우, wandb docker-run command를 참조하시기 바랍니다.

 **옵션**

| **옵션** | **설명** |
| :--- | :--- |
| --nvidia | / --no-nvidia nividia 런타임을 사용합니다. 다음의 경우 기본값은 nvidia입니다. |
| nvidia-docker | 존재하는 경우 |
| --digest | 이미지 다이제스트\(image digest\)를 출력하고 종료합니다. |
| --jupyter | / --no-jupyter 컨테이너에 jupyter lab을 실행합니다. |
| --dir | 컨테이너에 코드를 마운트할 디렉토리입니다. |
| --no-dir | 현재 디렉토리를 마운트하지 않습니다. |
| --shell | 컨테이너를 실행할 셸\(shell\)입니다. |
| --port | jupyter를 바인딩할 호스트 포트입니다. |
| --cmd | jupyter를 바인딩할 호스트 포트입니다. |
| --no-tty | tty 없이 명령을 실행합니다. |
| --help | 이 메시지를 표시하고 종료합니다. |

## wandb enabled

**사용법**

`wandb enabled [OPTIONS]`

 **요약**

W&B를 활성화합니다.

**옵션**

| **옵션** | **설명** |
| :--- | :--- |
| --help | 이 메시지를 표시하고 종료합니다. |

## wandb init

**사용법**

`wandb init [OPTIONS]`

 **요약**

Weights & Biases를 통해 디렉토리를 구성합니다.

**옵션**

| **옵션** | **설명** |
| :--- | :--- |
| -p, --project | 사용할 프로젝트입니다. |
| -e, --entity | 프로젝트의 범위를 지정할 개체입니다. |
| --reset | 설정을 재설정합니다. |
| -m, --mode | "online", "offline" 또는 "disabled". 기본값은 다음과 같습니다. |
| --help | 이 메시지를 표시하고 종료합니다. |

## wandb local

 **사용법**

`wandb local [OPTIONS]`

**요약**

 로컬 W&B 컨테이너를 실행합니다. \(실험 중\)

**옵션**

| **옵션** | **설명** |
| :--- | :--- |
| -p, --port | W&B로컬을 바인딩할 호스트 포트입니다. |
| -e, --env | wandb/local에 전달할 Env vars\(환경변수\)입니다. |
| --daemon | / --no-daemon 데몬 모드\(daemon mode\)에서 실행하거나 실행하지 않습니다. |
| --upgrade | 최신 버전으로 업그레이드합니다. |
| --help | 이 메시지를 표시하고 종료합니다. |

## wandb login

 **사용법**

`wandb login [OPTIONS] [KEY]...`

**요약**

 Weights & Biases로 로그인합니다

**옵션**

| **옵션** | **설명** |
| :--- | :--- |
| --cloud | 로컬 대신 클라우드로 로그인합니다. |
| --host | 지정된 W&B 인스턴스에 로그인합니다. |
| --relogin | 이미 로그인한 경우, 재로그인을 강제합니다. |
| --anonymously | 익명으로 로그인합니다. |
| --help | 이 메시지를 표시하고 종료합니다. |

## wandb offline

**사용법**

`wandb offline [OPTIONS]`

**요약**

 W&B 동기화를 비활성화합니다.

**옵션**

| **옵션**  | **설명** |
| :--- | :--- |
| --help | 이 메시지를 확인하고 종료합니다. |

## wandb online

**사용법**

`wandb online [OPTIONS]`

**요약**

W&B 동기화를 활성화합니다.

**옵션**

| **옵션** | **설명** |
| :--- | :--- |
| --help | 이 메시지를 표시하고 종료합니다. |

## wandb pull

**사용법**

`wandb pull [OPTIONS] RUN`

**요약**

Weights & Biases에서 파일을 가져옵니다.

**옵션**

| **옵션** | **설명** |
| :--- | :--- |
| -p, --project | 다운로드할 프로젝트입니다. |
| -e, --entity | 리스팅\(listing\)의 범위를 지정할 개체입니다. |
| --help | 이 메시지를 표시하고 종료합니다. |

## wandb restore

 **사용법**

`wandb restore [OPTIONS] RUN`

 **요약**

 실행에 대한 코드, 구성\(config\) 및 도커\(docker\) 상태를 복원합니다.

 **옵션**

| **옵션** | **설명** |
| :--- | :--- |
| --no-git | Skupp |
| --branch | / --no-branch 브랜치\(branch\) 또는 분리된 체크아웃\(checkout detached\) 생성 여부입니다. |
| -p, --project | 업로드할 프로젝트입니다. |
| -e, --entity | 리스팅\(listing\)의 범위를 지정할 개체입니다. |
| --help | 이 메시지를 표시하고 종료합니다. |

## wandb status

**사용법**

`wandb status [OPTIONS]`

**요약**

 구성\(configuration\) 설정을 표시합니다.

**옵션**

| **옵션** | **설명** |
| :--- | :--- |
| --settings | / --no-settings 현재 설정을 표시합니다. |
| --help | 이 메시지를 표시하고 종료합니다. |

## wandb sweep

 **사용법**

`wandb sweep [OPTIONS] CONFIG_YAML`

 **요약**

 스윕을 생성합니다.

**옵션**

| **옵션** | **설명** |
| :--- | :--- |
| -p, --project | **스윕의 프로젝트입니다.** |
| -e, --entity | **프로젝트에 대한 개체 범위입니다.** |
| --controller | **로컬 컨트롤러를 실행합니다.** |
| --verbose | **verbose 출력을 표시합니다.** |
| --name | **스윕 이름을 설정합니다.** |
| --program | **스윕 프로그램을 설정합니다.** |
| --update | **보류 중인 스윕\(pending sweep\)을 업데이트합니다.** |
| --help | **이 메시지를 표시하고 종료합니다.** |

## wandb sync

**사용법**

`wandb sync [OPTIONS] [PATH]...`

 **요약**

 오프라인 훈련 디렉토리를 W&B로 업로드합니다.

**옵션**

| **옵션** | 설명 |
| :--- | :--- |
| --id | 업로드할 실행입니다. |
| -p, --project | 업로드할 프로젝트입니다. |
| -e, --entity | 범위를 지정할 개체입니다. |
| --include-globs | 포함할 콤마로 구분된 globs의 리스트입니다. |
| --exclude-globs | 제외할 콤마로 구분된 globs의 리스트입니다. |
| --include-online | / --no-include-online |
| Include | 온라인 실행입니다. |
| --include-offline | / --no-include-offline |
| Include | 오프라인 실행입니다. |
| --include-synced | / --no-include-synced |
| Include | 동기화된 실행입니다. |
| --mark-synced | / --no-mark-synced |
| Mark | 동기화 바와 같은 실행입니다. |
| --sync-all | 모든 실행을 동기화합니다. |
| --clean | 동기화된 실행을 삭제합니다. |
| --clean-old-hours | 오랜 시간 전에 생성된 실행을 삭제합니다. |
| To | --clean flag와 함께 사용합니다. |
| --clean-force | 확인 프롬프트 없이 청소\(clean\)합니다. |
| --show | 표시할 실행의 수입니다. |
| --help | 이 메시지를 표시하고 종료합니다. |

