---
description: 모델 체크포인트 등의 파일을 스크립트 내에서 액세스 할 로컬 실행 폴더에 복원합니다.
---

# wandb.restore\(\)

### **개요**

 ****`wandb.restore`\(파일 이름\)을 호출하시면 파일을 여러분의 로컬 실행 디렉토리에 복원합니다. 일반적으로 `filename`\(파일 이름\)은 이전 실험 실행에서 생성되어 클라우드에 업로드 된 파일을 말합니다. 이 요청은 파일의 로컬 본사본을 만들고 읽기 위해 열려진 로컬 파일 스트림을 반환합니다.

 `wandb.restore`는 다음의 몇 가지 선택적 키워드 전달인자를 허용합니다.

* **run\_path** — $ENTITY\_NAME/$PROJECT\_NAME/$RUN\_ID' 또는 '$PROJECT\_NAME/$RUN\_ID'으로 포맷된, 파일을 가져 올 이전 실행을 참조하는 스트링 \(기본값: 현재 개체\(current entity\), 프로젝트 이름\(project name\), 실행 id\(run id\)\)
* **replace** — 로컬 복사본을 사용할 수 있는 경우 \(기본값: False\) 로컬 복사본의 파일명을 클라우드 복사본으로 덮어쓸 지 여부를 지정하는 불린\(boolean\)
* **root** — 파일의 로컬 복사본을 저장할 디렉토리를 지정하는 스트링. 기본값은 현재 작업 디렉토리로 지정되어 있으며, 또는 wandb.init 가 호출 된 경우 \(기본값: "."\), `wandb.run.dir` 입니다.

 공통 사용 케이스

* 과거 실행으로 생성된 모델 아키텍처 또는 가중치를 복원합니다
* 실행이 실패한 경우. 마지막 체크포인트에서부터 훈련을 재개합니다 \(중요 세부 사항은 [재개\(resuming\)](https://docs.wandb.ai/v/ko/library/resuming)에 관한 섹션 참조\)

##  **예시**

 **완성된**업 예제는 [이 리포트](https://app.wandb.ai/lavanyashukla/save_and_restore/reports/Saving-and-Restoring-Models-with-W%26B--Vmlldzo3MDQ3Mw)를 참조하십시오. 

```python
# restore a model file from a specific run by user "vanpelt" in "my-project"
best_model = wandb.restore('model-best.h5', run_path="vanpelt/my-project/a1b2c3d")

# restore a weights file from a checkpoint
# (NOTE: resuming must be configured if run_path is not provided)
weights_file = wandb.restore('weights.h5')
# use the "name" attribute of the returned object
# if your framework expects a filename, e.g. as in Keras
my_predefined_model.load_weights(weights_file.name)
```

> **run\_path를 지정하지 않은 경우, 실행을 위해** [**재개\(resuming\)**](https://docs.wandb.ai/v/ko/library/resuming)**를 구성하셔야 합니다. 프로그래밍 방식으로 훈련 밖의 파일에 액세스 하고 싶으시다면 을 사용하시기 바랍니다.**

