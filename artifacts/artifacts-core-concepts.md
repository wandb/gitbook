# Artifacts Core Concepts

 이번 가이드에서는 W&B 아티팩트를 잘 사용하기 위해 필요한 모든 것에 대해서 학습해보겠습니다. 자, 이제 시작해보겠습니다!

##  **조망도**

W&B 아티팩트는 W&B를 통한 파일 저장 여부 또는 추적할 버킷이 이미 있는지와는 관계없이, 여러분의 데이터세트 및 모델을 손쉽게 버저닝할 수 있게 설계되었습니다. 데이터 세트 또는 모델 파일을 추적한 경우, W&B는 자동으로 각/모든 수정 사항을 자동으로 로그하여, 완전한 감사 가능 파일 변경 사항 히스토리를 제공합니다. 이를 통해 사용자는 데이터세트 개선 및 모델 훈련의 중요하고 재미있는 부분에 집중할 수 있으며, W&B가 지루한 모든 세부 사항 추적 프로세스를 수행합니다.

##  **용어**

몇 가지 정의로 우선 시작해보겠습니다. 먼저, 우리가 말하는 “artifact”\(아티팩트\)란 정확히 무엇을 의미할까요?

개념상으로, 아티팩트는 단순히 이미지, HTML, 코드, 오디오, 원시 이진 데이터\(raw binary data\) 등 여러분이 원하는 것을 저장하는 디렉토리를 뜻합니다. 여러분께서는 이를 S3 또는 Google Cloud Storage 버킷과 동일하게 사용하실 수 있습니다. W&B는 이전 콘텐츠를 덮어쓰는 것 대신 새 버전의 아티팩트를 생성합니다.

 다음의 디렉토리 구조로 되어 있다고 가정해보겠습니다:

```text
images
|-- cat.png (2MB)
|-- dog.png (1MB)
```

새 아티팩트의 첫 번째 버전인, `animals`로 이를 로그합니다:

```text
#!/usr/bin/env python
#log_artifact.py
import wandb

run = wandb.init()
artifact = wandb.Artifact('animals', type='dataset')
artifact.add_dir('images')
run.log_artifact(artifact) # Creates `animals:v0`
```

 W&B 용어로, 이 버전에는 인덱스\(index\)`v0`이 있습니다. 모든 새 버전의 아티팩트는 인덱스를 하나씩 범프\(bump\)합니다. 수백 개의 버전이 있는 경우, 특정 버전을 인덱스로 참조하면 혼란스러우며 오류가 발생하기 쉽습니다. 이때 별칭\(aliases\)이 유용합니다. 별칭을 통해 사람이 읽을 수 있는 이름을 지정된 버전에 적용할 수 있습니다.

이것을 보다 구체적으로 나타내기 위해, 새로운 이미지로 데이터세트를 업데이트하고 새 버전을 `latest` 이미지로 표시한다고 해보겠습니다. 다음은 새 디렉토리 구조입니다:

```text
images
|-- cat.png (2MB)
|-- dog.png (1MB)
|-- rat.png (3MB)
```

 이제, `log_artifact.py`를 재실행하여 `animals:v1`을 생성할 수 있습니다. W&B는 자동으로 최신 버전에 별칭 latest를 할당하므로, 버전 인덱스를 사용하는 대신, `animals:latest`를 사용하여 이를 참조할 수도 있습니다. `aliases=['my-cool-alias']`를 `log_artifact`로 전달하여 버전에 적용할 별칭을 사용자 지정할 수 있습니다.

아티팩트를 참조하는 것은 간단합니다. 저희 훈련 스크립트에서 최신 버전의 데이터세트를 가져오려면 다음을 수행하시면 됩니다.

```text
import wandb

run = wandb.init()
animals = run.use_artifact('animals:latest')
directory = animals.download()

# Train on our image dataset...
```

 여기까지입니다! 이제 데이터세트를 수정하고 싶을 때마다 `log_artifact.py`를 다시 실행하실 수 있으며 나머지는 W&B가 처리합니다. 그러면 훈련 스크립트는 자동으로 `latest` 버전을 가져옵니다.

## **저장소 레이아웃**

아티팩트가 시간이 지남에 따라 개선되고 버전이 누적되기 시작하면 데이터세트의 모든 반복 사항을 저장하는데 필요한 공간 소요량을 걱정하실 수도 있습니다. 다행히 W&B는 두 버전 간에 변경된 파일에만 저장 비용이 발생하도록 아티팩트 파일을 저장합니다.

 다음 콘텐츠를 추적하는 `animals:v0`와 `animals:v1`을 다시 참조해보겠습니다.

```text
images
|-- cat.png (2MB) # Added in `v0`
|-- dog.png (1MB) # Added in `v0`
|-- rat.png (3MB) # Added in `v1`
```

`v1`이 총 6MB의 파일을 추적하는 동안 3MB의 공간만 차지합니다. 이는 나머지 3MB의 공간을 `v0`와 공동으로 공유하기 때문입니다. `v1`을 삭제하려면 `rat.png`와 연관된 3MB를 회수해야 할 수 있습니다. 반면에 `v0`를 삭제하려는 경우, `v1`은 해당 크기를 6MB로 가져오는 `cat.png`와 `dog.png`의 저장 비용을 물려받아야 합니다.

##  **데이터 프라이버시 및 규정 준수**

아티팩트를 로그할 때, 파일은 W&B가 관리하는 Google Cloud 버킷에 업로드됩니다. 버킷의 콘텐츠는 유휴 상태 및 전송 중 모두의 경우에 암호화됩니다. 게다가 아티팩트 파일은 해당 프로젝트에 액세스할 수 있는 사용자들만 볼 수 있습니다.

![](../.gitbook/assets/image%20%2817%29.png)

아티팩트 버전을 삭제하는 경우, 안전하게 삭제할 수 있는 모든 파일은 \(이전 또는 후속 버전에서 사용되지 않은 파일\) 즉시 버킷에서 제거됩니다. 이와 마찬가지로 전체 아티팩트를 삭제하는 경우, 해당 아티팩트의 모든 콘텐츠는 버킷에서 제거됩니다.

 여러 사용자가 사용하는 환경\(multi-tenant environment\)에 두면 안 되는 민감한 데이터세트의 경우, 여러분의 cloud 버킷에 연결된 개인 W&B 서버 또는 참조 아티팩트\(reference artifacts\)를 사용하실 수 있습니다. 참조 아티팩트는 파일의 링크를 버킷 또는 서버에 유지합니다. 즉, W&B는 파일 자체가 아닌 파일과 연관된 메타데이터만 추적함을 의미합니다.

![](../.gitbook/assets/image%20%284%29.png)

참조 아티팩트 구축은 일반 아티팩트와 동일하게 작동합니다.

```text
import wandb

run = wandb.init()
artifact = wandb.Artifact('animals', type='dataset')
artifact.add_reference('s3://my-bucket/animals')
```

사용자는 버킷 및 버킷의 파일에 대한 제어를 할 수 있지만, W&B는 사용자를 대신하여 모든 메타데이터만을 추적합니다.

