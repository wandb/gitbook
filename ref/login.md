# Login

## wandb.sdk.wandb\_login

 [\[소스\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_login.py#L3) 

Weights & Biases에 로그인 하여 사용자 계정에 로그 하도록 여러분의 머신을 인증합니다.

**login**

```python
login(anonymous=None, key=None, relogin=None, host=None, force=None)
```

 [\[소스\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_login.py#L22)

 W&B에 로그인합니다.

 **전달인자**:

* `anonymous` string, optional - "must"\(반드시 허용\), "allow"\(허용\), or "never"\(허용 안 함\). “must”로 설정된 경우, 사용자가 아직 로그인하지 않은 경우에만 익명 사용자를 생성합니다.
* `key` _string, optional_ - 인증 키.
* `relogin` _bool, optional_ - True인 경우 API 키를 확인하는 다시 메시지가 표시됩니다.
* `host` _string, optional_ - 연결할 호스트

 **반환:**

* `bool` - 키가 구성된 경우

 **발생\(Raises\)**:

UsageError - api\_key를 구성할 수 없고, tty가 없는 경우

