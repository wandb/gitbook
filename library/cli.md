---
description: '명령 줄 인터페이스로 로그인 및 코드 상태 복원, 로컬 디렉토리를 서버에 동기화, 그리고 초매개변수 스윕을 실행하실 수 있습니다'
---

# Command Line Interface

`pip install wandb`를 실행하신 후, **wandb**라는 새 명령어를 사용하실 수 있어야 합니다.  


다음의 하위 명령어를 사용하실 수 있습니다”

| 하위 명령어 |  설명 |
| :--- | :--- |
| docs | 브라우저에서 문서를 엽니다. |
| init | W&B로 디렉토리를 구성합니다 |
| login | W&B에 로그인합니다 |
| offline | W&B를 이 디렉토리에서 비활성화 합니다. 테스트에 유용합니다 |
| online | 이 디렉토리에서 W&B가 활성화 되어 있는지 확인합니다 |
| disabled | Disables all API calls, useful for testing |
| enabled | Same as `online`, resumes normal W&B logging, once you've finished testing |
| docker | 도커 이미지\(docker image\) 실행, cwd 마운트 및 wandb가설치되었는지 확인합니다 |
| docker-run | W&B 환경 변수를 도커 실행 명령\(docker run command\)에 추가합니다 |
| projects |  프로젝트의 리스트를 작성합니다 |
| pull | W&B에서 실행을 위한 파일을 가져옵니다 |
| restore | 실행에 대한 코드 및 구성 상태\(config state\)를 복원합니다 |
| run | python프로그램이 아닌 다른 프로그램을 실행합니다. 파이선의 경우, wandb.init\(\)을 사용합니다 |
| runs | 프로젝트의 실행의 리스트를 작성합니다 |
| sync | tfevents 또는 이전 실행파일을 포함한 로컬 디렉토리를 동기화합니다 |
| status | 현재 디렉토리 상태의 리스트를 작성합니다 |
| sweep | TAML 정의\(definition\)가 지정된 새로운 스윕\(sweep\)을 생성합니다 |
| agent | 스윕에서 프로그램을 실행하도록 에이전트를 시작합니다 |

##  **코스 상태 복원하기**

 지정된 실행을 실행할 때, `restore`를 사용하셔서 코드 상태로 돌아갑니다.

###  **예시**

```python
# creates a branch and restores the code to the state it was in when run $RUN_ID was executed
wandb restore $RUN_ID
```

 **코드 상태를 어떻게 캡쳐하나요?**

 스크립트에서 `wandb.init`가 요청 될 때, 코드가 git repository에 있는 경우 링크가 마지막 git 커밋\(commit\)에 저장됩니다. 또한, 원격 상태에서 동기화 되지 않은 변경 사항이나 커밋 되지 않은 변경사항이 있는 경우 diff 패치\(patch\) 또한 생성됩니다.

