# Settings

## wandb.sdk.wandb\_settings

 [\[소스\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_settings.py#L2) 

이 모듈은 wandb 실행에 대한 설정을 구성합니다.

로딩 설정 순서: \(differs from priority\) defaults environment wandb.setup\(settings=\) system\_config workspace\_config wandb.init\(settings=\) network\_org network\_entity network\_project

설정 우선순위: "source" 변수를 참조하시기 바랍니다.

오버라이드\(override\)를 사용하는 경우, 오버라이드가 아닌\(non-override\) 설정보다 우선합니다

오버라이드 우선순위는 오버라이드가 아닌 설정의 역순입니다.

### Settings Objects

```python
class Settings(object)
```

 [\[소스\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_settings.py#L187)​

 설정 생성자\(Constructor\)

**전달인자**:

* `entity` - 실행을 위해 사용할 개인 사용자 또는 팀
* `project` - 실행에 대한 프로젝트 이름

 **발생\(Raises\)**:

* `Exception` - 문제인 경우.

**\_\_copy\_\_**

```python
 | __copy__()
```

 [\[소스\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_settings.py#L656)​

 복사 \(복사된 객체는 고정\(frozen\) 되지 않습니다\).

