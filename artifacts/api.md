# Artifacts API

데이터세트 추적 및 모델 버저닝을 위해 W&B 아티팩트를 사용하세요. 실행을 초기화하고, 아티팩트를 생성한 후, 워크플로의 다른 부분에서 사용하세요. 아티팩트를 파일 추적 및 저장 또는 외부 URL 추적에 사용하실 수 있습니다.

이 기능은 `wandb` 버전 0.9.9 부터 시작하는 클라이언트에서 사용하실 수 있습니다

## 1. **실행 초기화**

파이프라인 단계를 추적하시려면, 스크립트의 실행을 초기화합니다. **job\_type**에 대한 스트링을 명시하여 다른 파이프라인 단계 \(사전처리\(preprocessing\), 훈련\(training\), 평가\(evaluation\) 등\)를 구분합니다. W&B로 실행을 조직한 적이 없는 경우, 저의 [Python 라이브러리](https://docs.wandb.com/library) 문서에서 실험 추적에 대한 보다 자세한 가이드라인을 제공하고 있습니다.  


```python
run = wandb.init(job_type='train')
```

## 2. Create an artifact

아티팩트는 아티팩트에 저장된 실제 파일 또는 외부 URL에 대한 참조인 콘텐츠를 포함한 데이터 폴더와 같습니다. **유형\(type\)**에 대한 스트링을 명시화해서 다른 아티팩트\(데이터세트, 모델, 결과, 등\)를 구별하실 수 있습니다. 아티팩트에 `bike-dataset`과 같은 **이름**을 지정해서 아티팩트 내에 무엇이 있는지 쉽게 기억하실 수 있습니다. 파이프라인의 이 후 단계에서, 이 아티팩트를 다운로그 하기 위해 `bike-dataset:v1`와 같은 버전을 함께 이 이름을 사용하실 수 있습니다.

**log\_artifact**를 호출할 때, 저희는 아티팩트의 콘텐츠가 변경되었는지 확인하고, 변경이 확인 되었다면, 자동으로 새로운 버전의 아티펙트\(v0, v1, v2 등\)을 생성합니다.

**wandb.Artifact\(\)**

* **type \(str\)**: 조직 목적으로 사용되는 아티팩트의 유형을 구분합니다. "dataset", "model", "result”를 계속 사용하시는 것을 추천합니다.
* **name \(str\)**: 아티팩트를 참조할 때 사용되는 고유한 이름을 아티팩트에 지정하세요. 숫자, 문자, 밑줄\(underscore\), 하이픈, 점을 이름에 사용하실 수 있습니다.
* **description \(str, optional\)**: UI에서 아티팩트 버전 옆에 보이는 자유 텍스트
* **metadata \(dict, optional\)**: 데이터세트의 클래스 분포와 같은 아티펙트와 관련된 구조화된 데이터. 저희가 웹 인터페이스를 확장 중이므로, 플롯 쿼리\(query\) 및 생성에 이 데이터를 사용하실 수 있습니다.

```python
artifact = wandb.Artifact('bike-dataset', type='dataset')

# Add a file to the artifact's contents
artifact.add_file('bicycle-data.h5')

# Save the artifact version to W&B and mark it as the output of this run
run.log_artifact(artifact)
```

## 3. **아티팩트 사용하기**

실행에의 입력으로 아티팩트를 사용하실 수 있습니다. 예를 들어, `bike-dataset`의 첫 번째 버전인 `bike-dataset:v0`를 가지고 우리의 파이프라인에 다음 스크립트에서 사용할 수 있습니다. **use\_artifact** 요청하실 때, 지명된 아티팩트 찾기 및 실행으로의 입력으로 표시하기 위해 여러분의 스크립트는 W&B를 쿼리합니다.

```python
# Query W&B for an artifact and mark it as input to this run
artifact = run.use_artifact('bike-dataset:v0')

# Download the artifact's contents
artifact_dir = artifact.download()
```

 **다른 프로젝트의 아티팩트 사용하기**  
아티펙트의 이름을 프로젝트의 이름을 통해 자격을 부여하여 엑세스할 수 있는 모든 프로젝트의 아티팩트를 자유롭게 참조하실 수 있습니다. 또한, 아티팩트 이름을 개체의 이름으로 추가로 자격을 부여하여 객체에 걸쳐서 아티팩트를 참조하실 수 있습니다

```python
# Query W&B for an artifact from another project and mark it
# as an input to this run.
artifact = run.use_artifact('my-project/bike-model:v0')

# Use an artifact from another entity and mark it as an input
# to this run.
artifact = run.use_artifact('my-entity/my-project/bike-model:v0')
```

 **로그 되지 않은 아티팩트 사용하기**  
아티팩트 객체를 생성하고 **use\_artifact**로 전달할 수 있습니다. 저희는 아티팩트가 이미 W&B에 존재하는 지 확인하고, 그렇지 않다면, 새로운 아티팩트를 생성합니다. 이는 멱등성\(idempotent\)입니다. 즉, 아티팩트를 원하시는 횟수 만큼 use\_artifact에 전달하실 수 있으며, 저희는 콘텐츠가 그대로 유지되는 한 중복 제거해 드립니다.

```python
artifact = wandb.Artifact('bike-model', type='model')
artifact.add_file('model.h5')
run.use_artifact(artifact)
```

## **버전 및 앨리어스 \(Versions and aliases\)**

아티팩를 처음으로 로그할 때, 저희는 버전 **v0**을 생성합니다. 동일한 아티팩트에 다시 로그하면, 저희는 콘텐츠를 체크섬하고, 아티팩트가 변경된 경우, 새 버전 **v1**을 저장합니다.

 앨리어스를 특정 버전에 대한 포인터로 사용하실 수 있습니다. 기본값으로, run.log\_artifact는 **최신** 앨리어스를 로그된 버전에 추가합니다.

앨리어스를 사용해서 아티팩트를 가져올 수 있습니다. 예를 들어, 훈련 스크립트에 항상 초시ㅣㄴ 버전의 데이터세트를 끌어오려면, 해당 아티팩트를 사용하실 때 **최신\(latest\)**를 명시하십시오.

```python
artifact = run.use_artifact('bike-dataset:latest')
```

 또한 사용자 지정 앨리어스를 아티팩트 버전에 적용할 수 있습니다. 예를 들어, 어떤 모델 체크포인트가 메트릭 AP-50에 적합한지 표시하려면, 모델 아티팩트를 로그하실 때, 스트링 **best-ap50**을 앨리어스로 추가하실 수 있습니다.

```python
artifact = wandb.Artifact('run-3nq3ctyy-bike-model', type='model')
artifact.add_file('model.h5')
run.log_artifact(artifact, aliases=['latest','best-ap50'])
```

##  **아티팩트 구축하기**

아티팩트는 데이터 폴더와 같습니다. 각 엔트리는 아티팩트 내에 저장된 실제 파일이거나 외부 URL에 대한 참조입니다. 정규 파일시스템과 마찬가지로 아티팩트 내에 폴더를 중첩할 수 있습니다. `wandb.Artifact()` 클래스를 초기화 하여 새 아티팩트를 구축하세요.

다음 필드를 `Artifact()` 구축자에게 전달하거나, 아티팩트 객체에 직접 설정하실 수 있습니다.

* **type:** ‘dataset’, ‘model’, ‘result’ 이어야 합니다.
* **description**: UI에 표시될 자유형식 텍스트.
* **metadata**: 구조화된 데이터를 포함할 수 있는 사전. 플롯 쿼링 및 생성에 이 데이터를 사용하실 수 있습니다. 예를 들면, 메타데이터로써 데이터세트에 대한 클래스 분포를 저장하도록 선택하실 수 있습니다.

```python
artifact = wandb.Artifact('bike-dataset', type='dataset')
```

디렉토리를 추가하신다면, 선택적 파일명 또는 파일 경로 접두사를 명시하여**이름**을 사용하십시오.

```python
# Add a single file
artifact.add_file(path, name='optional-name')

# Recursively add a directory
artifact.add_dir(path, name='optional-prefix')

# Return a writeable file-like object, stored as <name> in the artifact
with artifact.new_file(name) as f:
    ...  # Write contents into the file 

# Add a URI reference
artifact.add_reference(uri, name='optional-name')
```

###  **파일 및 디렉토리 추가하기**

다음의 예에서, 이러한 파일을 포함하는 프로젝트 디렉토리가 있다고 가정해봅시다:

```text
project-directory
|-- images
|   |-- cat.png
|   +-- dog.png
|-- checkpoints
|   +-- model.h5
+-- model.h5
```

<table>
  <thead>
    <tr>
      <th style="text-align:left">API call</th>
      <th style="text-align:left">Resulting artifact contents</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">artifact.add_file(&apos;model.h5&apos;)</td>
      <td style="text-align:left">model.h5</td>
    </tr>
    <tr>
      <td style="text-align:left">artifact.add_file(&apos;checkpoints/model.h5&apos;)</td>
      <td style="text-align:left">model.h5</td>
    </tr>
    <tr>
      <td style="text-align:left">artifact.add_file(&apos;model.h5&apos;, name=&apos;models/mymodel.h5&apos;)</td>
      <td
      style="text-align:left">models/mymodel.h5</td>
    </tr>
    <tr>
      <td style="text-align:left">artifact.add_dir(&apos;images&apos;)</td>
      <td style="text-align:left">
        <p>cat.png</p>
        <p>dog.png</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">artifact.add_dir(&apos;images&apos;, name=&apos;images&apos;)</td>
      <td
      style="text-align:left">
        <p>images/cat.png</p>
        <p>images/dog.png</p>
        </td>
    </tr>
    <tr>
      <td style="text-align:left">artifact.new_file(&apos;hello.txt&apos;)</td>
      <td style="text-align:left">hello.txt</td>
    </tr>
  </tbody>
</table>

###  **참조 추가하기**

```python
artifact.add_reference(uri, name=None, checksum=True)
```

* **uri \(string\):** 추적할 참조 URI
* **name \(string\):** 선택적 이름 무효. 지정되지 않은 경우, **uri**에서 이름이 추론 됩니다.
* **checksum \(bool\):** 참인 경우, 참조는 유효성 목적을 위해 **uri**에서 체크섬 정보 및 메타데이터를 수집합니다.

실제 파일 대신 외부 URL에 대한 참조를 아티팩트에 추가하실 수 있습니다. URL에 wandb가 처리 방식을 알고 있는 스킴\(scheme\)이 있는 경우, 아티팩트는 재생산\(reproductivity\)를 위해 체크섬 및 기타 정보를 추적합니다. 아티팩트는 현재 다음과 같은 URL 스킴을 지원합니다:

* `http(s)://`: HTTP를 통해 액세스할 수 있는 파일의 경로. HTTP서버가 `ETag` 및 `Content-Length` 응답 헤더를 지원하는 경우, 아티팩트는 etags 및 사이즈 메타데이터 형태의 체크섬을 추적합니다.
* `s3://`: S3로의 객체 또는 객체 접두사에 대한 경로. 참조된 객체에 대해, 아티팩트는 체크섬 및 버저닝 정보\(버킷이 객체 버저닝 활성화 된 경우\)를 추적합니다.
* `gs://`: GCS에서 객체 또는 객체 접두사로의 경로. 아티팩트는 체크섬 및 버저닝 정보를 추적합니다 \(버킷이 객체 버저닝 활성화 된 경우\). 객체 접두사는 최대 10,000개의 객체까지 접두사 하에 객체를 포함하도록 확장됩니다.

다음 예에서, 이러한 형식의 파일이 있는 S3 버킷을 가지고 있다고 가정해봅시다:

```text
s3://my-bucket
|-- images
|   |-- cat.png
|   +-- dog.png
|-- checkpoints
|   +-- model.h5
+-- model.h5
```

<table>
  <thead>
    <tr>
      <th style="text-align:left">API call</th>
      <th style="text-align:left">Resulting artifact contents</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">artifact.add_reference(&apos;s3://my-bucket/model.h5&apos;)</td>
      <td style="text-align:left">model.h5</td>
    </tr>
    <tr>
      <td style="text-align:left">artifact.add_reference(&apos;s3://my-bucket/checkpoints/model.h5&apos;)</td>
      <td
      style="text-align:left">model.h5</td>
    </tr>
    <tr>
      <td style="text-align:left">artifact.add_reference(&apos;s3://my-bucket/model.h5&apos;, name=&apos;models/mymodel.h5&apos;)</td>
      <td
      style="text-align:left">models/mymodel.h5</td>
    </tr>
    <tr>
      <td style="text-align:left">artifact.add_reference(&apos;s3://my-bucket/images&apos;)</td>
      <td style="text-align:left">
        <p>cat.png</p>
        <p>dog.png</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">artifact.add_reference(&apos;s3://my-bucket/images&apos;, name=&apos;images&apos;)</td>
      <td
      style="text-align:left">
        <p>images/cat.png</p>
        <p>images/dog.png</p>
        </td>
    </tr>
  </tbody>
</table>

##  **아티팩트 사용 및 다운로드하기**

```python
run.use_artifact(artifact=None)
```

* 아티팩트를 실행에 대한 입력으로 표시합니다.

아티팩트 사용에 관해서, 두 가지 패턴이 있습니다. W&B에 명시적으로 저장된 아티팩트 이름을 사용하시거나, 아티팩트 객체를 구축하고, 전달하여 필요한 경우 중복제거 하실 수 있습니다.

### **W&B에 저장된 아티팩트 사용하기**

```python
artifact = run.use_artifact('bike-dataset:latest')
```

반환된 아티팩트에 다음의 수단을 호출하실 수 있습니다:

```python
datadir = artifact.download(root=None)
```

* 현재 존재하지 않는 모든 아티팩트의 콘텐츠를 다운로드 합니다. 이는 아티팩트 콘텐츠를 포함하고 있는 디렉토리의 경로로 반환됩니다. **루트\(root\)**를 설정하여 다운로드 경로를 명시적으로 지정하실 수 있습니다.

```python
path = artifact.get_path(name)
```

* 경로 이름에 파일만 가져 옵니다. 다음 수단을 통해서 엔트리 객체`(Entry object)`를 반환합니다.
  * **Entry.download\(\)**: 경로 이름에 아티팩트로부터 파일을 다운로드합니다.
  * **Entry.ref\(\)**: 엔트리가 `add_reference`를 사용하여 참조로 저장되어 있다면, URL을 반환합니다.

W&B가 처리 방식을 알고 있는 스킴\(scheme\)을 포함한 참조는 아티팩트 파일처럼 다운로드 하실 수 있습니다. 소비자 API \(consumer API\) 또한 동일합니다.

###  **아티팩트 구축 및 사용하기**

아티팩트 객체를 구축하고 **use\_artifact**로 전달하실 수 있습니다. 아직 아티팩트가 존재하지 않는 경우, W&B에 아티팩트가 생성됩니다. 이는 멱등성\(idempotent\)이며, 따라서 여러분이 원하시는 횟수만큼 하실 수 있습니다. `model.h5`의 콘텐츠가 동일하게 유지되는 한, 아티팩트는 오직 한 번 생성됩니다.

```python
artifact = wandb.Artifact('reference model')
artifact.add_file('model.h5')
run.use_artifact(artifact)
```

### **실행 외부에서 아티팩트 다운로드하기**

```python
api = wandb.Api()
artifact = api.artifact('entity/project/artifact:alias')
artifact.download()
```

##  **아티펙트 업데이트하기**

 아티팩트의 설명, 메타데이터, 앨리어스를 희망 값\(desired values\)로 설정한 후 calling save\(\) 함으로써 아티팩트의 설명, 메타데이터, 앨리어스를 업데이트 하실 수 있습니다.

```python
api = wandb.Api()
artifact = api.artifact('bike-dataset:latest')

# Update the description
artifact.description = "My new description"

# Selectively update metadata keys
artifact.metadata["oldKey"] = "new value"

# Replace the metadata entirely
artifact.metadata = {"newKey": "new value"}

# Add an alias
artifact.aliases.append('best')

# Remove an alias
artifact.aliases.remove('latest')

# Completely replace the aliases
artifact.aliases = ['replaced']

# Persist all artifact modifications
artifact.save()
```

##  **데이터 프라이버시**

아티팩트는 안전한 API-수준 액세스 제어를 사용합니다. 유후 상태 또는 전송 중에 파일은 암호화됩니다. 아티팩트는 또한 파일 콘텐츠를 W&B에 전송하지 않고 개인 버킷에 대한 참조를 추적합니다. 그 대신 contact@wandb.com로 연락하셔서 개인 클라우디 및 온프렘\(on-prem\) 설치에 대해 문의하실 수 있습니다.

