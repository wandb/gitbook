# Security

간단하게, W&B는 API에 액세스할 때 인증에 API 키를 사용합니다. 사용자는 사용자의 [설정](https://app.wandb.ai/settings)에서 API 키를 찾으실 수 있습니다. API 키를 안전하게 보관하셔야 하며 버전 제어\(version control\)로 체크인해서는 안 됩니다. 개인 API 키 외에도, Service Account\(서비스 계정\) 사용자를 여러분의 팀에 추가하실 수 있습니다.

## **키 순환\(Key Rotation\)**

 개인 계정 및 서비스 계정 모두 순환 또는 폐기하실 수 있습니다. 새 API 키 또는 서비스 계정 사용자를 생성하고 새로운 키를 사용하도록 스크립트를 재구성하시기만 하면 됩니다. 모든 프로세스가 재구성되면, 프로필 또는 팀에서 오래된 API 키를 제거하실 수 있습니다.

##  **계정 간 전환**

동일한 머신에서 작동하는 2개의 W&B 계정을 가지고 계신 경우, 서로 다른 API 키를 전환하는 좋은 방법이 필요합니다. API 키를 모두 머신의 파일에 저장하신 후, 다음과 같은 코드를 여러분의 repo에 추가하실 수 있습니다. 이는 잠재적으로 위험할 수 있는 소스 제어\(source control\) 시스템으로 여러분의 비밀 키를 확인하는 것을 방지하기 위한 조치입니다.

```text
if os.path.exists("~/keys.json"):
   os.environ["WANDB_API_KEY"] = json.loads("~/keys.json")["work_account"]
```

