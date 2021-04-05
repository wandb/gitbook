# Login

[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)](https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_login.py#L22-L43)[GitHub에서 소스 확인하기](https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_login.py#L22-L43)​

 W&B로 로그인합니다.

```text
login(
    anonymous=None, key=None, relogin=None, host=None, force=None
)
```

| 전달인자 |  |
| :--- | :--- |
|  `anonymous` | \(string, optional\) “must”\(반드시 허용\), “allow”\(허용\), “never”\(허용 안 함\)이며, “must”로 설정된 경우 항상 익명으로 로그인하며, “allow”인 경우, 사용자가 아직 로그인하지 않은 경우에만 익명 사용자를 생성합니다. |
|  `key` |  \(string, optional\) 인증 키 |
|  `relogin` |  \(bool, optional\) true인 경우, API 키에 대하여 재안내\(re-prompt\)합니다 |
|  `host` | \(string, optional\) 연결할 호스트 |

| 반환 |  |
| :--- | :--- |
|  `bool` | 키가 구성된 경우 |

| 발생\(Raises\) |
| :--- |
| UsageError – api\_key를 구성할 수 없으며 tty가 없는 경우 |

