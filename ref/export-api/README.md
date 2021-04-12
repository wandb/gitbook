# Public API

[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)](https://www.github.com/wandb/client/tree/master/wandb/__init__.py)[GitHub에서 소스 확인하기](https://www.github.com/wandb/client/tree/master/wandb/__init__.py)​

Wandb는 머신러닝 실험 추적을 돕는 라이브러리입니다.

Wandb에 대한 자세한 정보는 [https://docs.wandb.com](https://docs.wandb.com/)을 참조하시기 바랍니다.

가장 일반적으로 사용되는 함수/객체\(object\)는 다음과 같습니다:

* wandb.init — 훈련 스크립트의 상단에 새로운 실행을 초기화합니다.
* wandb.config — 초매개변수를 추적합니다.
* wandb.log — 훈련 루프 내에서 시간의 경과에 따라 메트릭을 로그합니다.
* wandb.save — 모델 가중치\(weights\)와 같은 실행과 관련된 파일을 저장합니다.
* wandb.restore — 지정된 실행을 실행했을 때의 코드 상태를 복원합니다.

사용 예시는 github.com/wandb/examples를 참조하시기 바랍니다

## **클래스**

[`class Api`](): wandb 서버를 쿼리하는 데 사용됩니다.

[`class Artifact`]()

[`class File`](): File은 wandb가 저장한 파일과 관련된 클래스입니다.

[`class Files`](): Files는 반복 가능한\(iterable\) `File` 객체의 모음입니다.

[`class Project`](): project는 실행에 대한 이름 영역입니다.

[`class Projects`](): 반복 가능한 `Project` 객체의 모음입니다.

[`class Run`](): 개체\(entity\) 및 프로젝트와 관련된 단일 실행입니다.

[`class Runs`](): 프로젝트 및 선택적 필터와 관련된 반복 가능한 실행의 모음입니다.

[`class Sweep`](): 스윕과 관련된 실행 집합입니다.

