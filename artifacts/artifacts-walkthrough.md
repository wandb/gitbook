# Artifacts Walkthrough

데이터세트 추적 및 모델 버저닝을 위해 W&B 아티팩트를 사용하세요. 실행을 초기화하고, 아티팩트를 생성한 후, 워크플로의 다른 부분에서 사용하세요. 아티팩트를 파일 추적 및 저장 또는 외부 URL 추적에 사용하실 수 있습니다.

이 기능은 `wandb` 버전 0.9.0부터 시작하는 클라이언트에서 사용하실 수 있습니다.

## 1.  **실행 초기화** <a id="1-initialize-a-run"></a>

파이프라인 단계를 추적하시려면, 스크립트의 실행을 초기화합니다. job\_type에 대한 스트링을 지정하여 다른 파이프라인 단계를 \(사전처리\(preprocessing\), 훈련\(training\), 평가\(evaluation\) 등\) 구분합니다. W&B로 실행을 조직한 적이 없는 경우, 저희의[ Python 라이브러리 ](https://docs.wandb.ai/v/ko/library)문서에 더 자세히 설명된 실험 추적에 대한 가이드라인을 제공하고 있으니 참조하시기 바랍니다.

```text
run = wandb.init(job_type='train')
```

## 2. **아티팩트 생성하기** <a id="2-create-an-artifact"></a>

아티팩트는 아티팩트에 저장된 실제 파일 또는 외부 URL에 대한 참조인 콘텐츠를 포함한 데이터 폴더와 같습니다. 아티팩트를 생성하기 위해, 아티팩트를 실행의 출력으로 로그하십시오. type\(유형\)에 대한 스트링을 지정하여 다른 아티팩트\(데이터세트, 모델, 결과, 등\)를 구별하실 수 있습니다. 아티팩트에 bike-dataset과 같은 이름을 지정해서 아티팩트 내에 무엇이 있는지 쉽게 기억하실 수 있습니다. 파이프라인의 이후 단계에서, 이 아티팩트를 다운로드하기 위해 `bike-dataset:v`1과 같은 버전을 함께 이 이름을 사용하실 수 있습니다.

**log\_artifact**를 호출할 때, 저희는 아티팩트의 콘텐츠가 변경되었는지 확인하고, 변경이 확인되었다면, 자동으로 새로운 버전의 아티팩트\(v0, v1, v2 등\)를 생성합니다.

**wandb.Artifact\(\)**

* **type \(str\)**: 조직 상의 목적으로 사용되는 아티팩트의 유형을 구별합니다. "dataset", "model", "result”를 계속 사용하시는 것을 추천합니다.
* **name \(str\)**: 아티팩트를 참조할 때 사용되는 고유한 이름을 아티팩트에 지정하세요. 숫자, 문자, 밑줄\(underscore\), 하이픈, 점을 이름에 사용하실 수 있습니다.
* **description \(str, optional\)**: UI에서 아티팩트 버전 옆에 보이는 자유 텍스트입니다.
* **metadata \(dict, optional\)**: 데이터세트의 클래스 분포와 같은 아티팩트와 관련된 구조화된 데이터입니다. 저희가 웹 인터페이스를 확장 중이므로, 플롯 쿼리\(query\) 및 생성에 이 데이터를 사용하실 수 있습니다.

```text
artifact = wandb.Artifact('bike-dataset', type='dataset')​# Add a file to the artifact's contentsartifact.add_file('bicycle-data.h5')​# Save the artifact version to W&B and mark it as the output of this runrun.log_artifact(artifact)
```

 **참고**: `log_artifact`에 대한 호출은 성능 기준에 맞는\(performant\) 업로드를 위해 비동기식으로 수행됩니다. 이는 아티팩트를 루프에서 로그할 때 놀라운 작용을 발생시킬 수 있습니다. 예를 들면 다음과 같습니다:

```text
for i in range(10):    a = wandb.Artifact('race', type='dataset', metadata={        "index": i,    })    # ... add files to artifact a ...    run.log_artifact(a)
```

아티팩트 버전 **v0**는 아티팩트는 임의의 순서로 로그될 수 있기 때문에 아티팩트 버전 v0의 메타데이터에 인덱스 0가 있을지는 확실하지 않습니다.

## 3.  **아티팩트 사용하기** <a id="3-use-an-artifact"></a>

실행에의 입력으로 아티팩트를 사용하실 수 있습니다. 예를 들어, `bike-dataset`의 첫 번째 버전인 `bike-dataset:v0`를 가지고 우리의 파이프라인에 다음 스크립트에서 사용할 수 있습니다. **use\_artifact** 요청하실 때, 지명된 아티팩트 찾기 및 실행으로의 입력으로 표시하기 위해 여러분의 스크립트는 W&B를 쿼리합니다.

```text
# Query W&B for an artifact and mark it as input to this runartifact = run.use_artifact('bike-dataset:v0')​# Download the artifact's contentsartifact_dir = artifact.download()
```

**다른 프로젝트의 아티팩트 사**용하기아티팩트의 이름을 프로젝트의 이름을 통해 자격을 부여하여 액세스할 수 있는 모든 프로젝트의 아티팩트를 자유롭게 참조하실 수 있습니다. 또한, 아티팩트 이름을 개체의 이름으로 추가로 자격을 부여하여 객체에 걸쳐서 아티팩트를 참조하실 수 있습니다.

```text
# Query W&B for an artifact from another project and mark it# as an input to this run.artifact = run.use_artifact('my-project/bike-model:v0')​# Use an artifact from another entity and mark it as an input# to this run.artifact = run.use_artifact('my-entity/my-project/bike-model:v0')
```

아티팩트 객체를 생성하고 **use\_artifact**로 전달할 수 있습니다. 저희는 아티팩트가 이미 W&B에 존재하는지 확인하고, 그렇지 않다면, 새로운 아티팩트를 생성합니다. 이는 멱등성\(idempotent\)을 뜻합니다. 즉, 아티팩트를 원하시는 횟수만큼 use\_artifact에 전달하실 수 있으며, 저희는 콘텐츠가 그대로 유지되는 한 중복 제거\(deduplicate\)해 드립니다.

```text
artifact = wandb.Artifact('bike-model', type='model')artifact.add_file('model.h5')run.use_artifact(artifact)
```

## **버전 및 별칭 \(Versions and aliases\)** <a id="versions-and-aliases"></a>

아티팩트를 처음으로 로그할 때, 저희는 버전 **v0**를 생성합니다. 동일한 아티팩트에 다시 로그하면, 저희는 콘텐츠를 체크섬하고, 아티팩트가 변경된 경우, 새 버전 **v1**을 저장합니다.

별칭을 특정 버전에 대한 포인터로 사용하실 수 있습니다. 기본값으로, run.log\_artifact는 **latest\(최신\)** 별칭을 로그된 버전에 추가합니다.

별칭을 사용해서 아티팩트를 가져올 수 있습니다. 예를 들어, 훈련 스크립트에 항상 최신 버전의 데이터세트를 끌어오려면, 해당 아티팩트를 사용하실 때 **latest**를 지정하십시오.

```text
artifact = run.use_artifact('bike-dataset:latest')
```

또한 사용자 지정 별칭을 아티팩트 버전에 적용할 수 있습니다. 예를 들어, 어떤 모델 체크포인트가 메트릭 AP-50에 적합한지 표시하려면, 모델 아티팩트를 로그하실 때, 스트링 **best-ap50**을 별칭으로 추가하실 수 있습니다.

```text
artifact = wandb.Artifact('run-3nq3ctyy-bike-model', type='model')artifact.add_file('model.h5')run.log_artifact(artifact, aliases=['latest','best-ap50'])
```

## **아티팩트 구축하기** <a id="constructing-artifacts"></a>

 아티팩트는 데이터 폴더와 같습니다. 각 엔트리는 아티팩트 내에 저장된 실제 파일이거나 외부 URL에 대한 참조입니다. 정규 파일시스템과 마찬가지로 아티팩트 내에 폴더를 중첩할 수 있습니다. `wandb.Artifact()` 클래스를 초기화하여 새 아티팩트를 구축하세요.

다음 필드를 `Artifact()` 구축자에게 전달하거나, 아티팩트 객체에 직접 설정하실 수 있습니다.

* **type\(유형\):** ‘dataset’, ‘model’, ‘result’ 이어야 합니다.
* **description\(설명\):** UI에 표시될 자유 형식 텍스트.
* **metadata\(메타데이터\):** 구조화된 데이터를 포함할 수 있는 사전. 플롯 쿼링 및 생성에 이 데이터를 사용하실 수 있습니다. 예를 들면, 메타데이터로써 데이터세트에 대한 클래스 분포를 저장하도록 선택하실 수 있습니다.

```text
artifact = wandb.Artifact('bike-dataset', type='dataset')
```

디렉토리를 추가하신다면, 선택적 파일명 또는 파일 경로 접두사를 지정하여 **name**을 사용하십시오.

```text
# Add a single fileartifact.add_file(path, name='optional-name')​# Recursively add a directoryartifact.add_dir(path, name='optional-prefix')​# Return a writeable file-like object, stored as <name> in the artifactwith artifact.new_file(name) as f:    ...  # Write contents into the file ​# Add a URI referenceartifact.add_reference(uri, name='optional-name')
```

### **파일 및 디렉토리 추가하기** <a id="adding-files-and-directories"></a>

다음의 예에서, 이러한 파일을 포함하는 프로젝트 디렉토리가 있다고 가정해봅시다:

```text
project-directory|-- images|   |-- cat.png|   +-- dog.png|-- checkpoints|   +-- model.h5+-- model.h5
```

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>API &#xD638;&#xCD9C;</b>
      </th>
      <th style="text-align:left"><b>&#xACB0;&#xACFC; &#xC544;&#xD2F0;&#xD329;&#xD2B8; &#xCF58;&#xD150;&#xCE20;</b>
      </th>
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

### **참조 추가** <a id="adding-references"></a>

```text
artifact.add_reference(uri, name=None, checksum=True)
```

* **uri \(string\):** 추적할 참조 **URI**입니다.
* **name \(string\):** 선택적 이름 무효. 지정되지 않은 경우, **uri**에서 이름이 추론됩니다.
* **checksum \(bool\):** true인 경우, 참조는 유효성 목적을 위해 **uri**에서 체크섬 정보 및 메타데이터를 수집합니다.

실제 파일 대신 외부 URL에 대한 참조를 아티팩트에 추가하실 수 있습니다. URL에 wandb가 처리 방식을 알고 있는 스킴\(scheme\)이 있는 경우, 아티팩트는 재생산\(reproductivity\)을 위해 체크섬 및 기타 정보를 추적합니다. 아티팩트는 현재 다음과 같은 URL 스킴\(scheme\)을 지원합니다:

* `http(s)://`: HTTP를 통해 액세스할 수 있는 파일의 경로. HTTP서버가 `ETag` 및 `Content-Length` 응답 헤더를 지원하는 경우, 아티팩트는 etags 및 사이즈 메타데이터 형태의 체크섬을 추적합니다.
* `s3://`: S3로의 객체 또는 객체 접두사에 대한 경로. 참조된 객체에 대해, 아티팩트는 체크섬 및 버저닝 정보\(버킷이 객체 버저닝 활성화된 경우\)를 추적합니다.
* `gs://`: GCS에서 객체 또는 객체 접두사로의 경로. 아티팩트는 체크섬 및 버저닝 정보를 추적합니다 \(버킷이 객체 버저닝 활성화된 경우\). 객체 접두사는 최대 10,000개의 객체까지 접두사 하에 객체를 포함하도록 확장됩니다.

다음 예에서, 이러한 형식의 파일이 있는 S3 버킷을 가지고 있다고 가정해보겠습니다:

```text
s3://my-bucket|-- images|   |-- cat.png|   +-- dog.png|-- checkpoints|   +-- model.h5+-- model.h5
```

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>API &#xD638;&#xCD9C;</b>
      </th>
      <th style="text-align:left"><b>&#xACB0;&#xACFC; &#xC544;&#xD2F0;&#xD329;&#xD2B8; &#xCF58;&#xD150;&#xCE20;</b>
      </th>
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

###  **병렬 실행\(parallel runs\)에서 파일 추가** <a id="adding-files-from-parallel-runs"></a>

대형 데이터세트 또는 분산 훈련의 경우 여러 병렬 실행이 단일 아티팩트에 기여해야 할 수도 있습니다. 다음의 패턴을 사용하여 이러한 병렬 아티팩트를 구축할 수 있습니다:

```text
import wandbimport time​# We will use ray to launch our runs in parallel# for demonstration purposes. You can orchestrate# your parallel runs however you want.import ray​ray.init()​artifact_type = "dataset"artifact_name = "parallel-artifact"table_name = "distributed_table"parts_path = "parts"num_parallel = 5​# Each batch of parallel writers should have its own# unique group name.group_name = "writer-group-{}".format(round(time.time()))​@ray.remotedef train(i):  """  Our writer job. Each writer will add one image to the artifact.  """  with wandb.init(group=group_name) as run:    artifact = wandb.Artifact(name=artifact_name, type=artifact_type)        # Add data to a wandb table. In this case we use example data    table = wandb.Table(columns=["a", "b", "c"], data=[[i, i*2, 2**i]])        # Add the table to folder in the artifact    artifact.add(table, "{}/table_{}".format(parts_path, i))        # Upserting the artifact creates or appends data to the artifact    run.upsert_artifact(artifact)  # Launch your runs in parallelresult_ids = [train.remote(i) for i in range(num_parallel)]​# Join on all the writers to make sure their files have# been added before finishing the artifact. ray.get(result_ids)​# Once all the writers arefinished, finish the artifact# to mark it ready.with wandb.init(group=group_name) as run:  artifact = wandb.Artifact(artifact_name, type=artifact_type)    # Create a "PartitionTable" pointing to the folder of tables  # and add it to the artifact.  artifact.add(wandb.data_types.PartitionedTable(parts_path), table_name)    # Finish artifact finalizes the artifact, disallowing future "upserts"  # to this version.  run.finish_artifact(artifact)
```

## **아티팩트 사용 및 다운로드하기** <a id="using-and-downloading-artifacts"></a>

```text
run.use_artifact(artifact=None)
```

* 아티팩트를 실행에 대한 입력으로 표시합니다.

아티팩트 사용의 경우, 두 가지 패턴이 있습니다. W&B에 명확하게 저장된 아티팩트 이름을 사용하시거나, 아티팩트 객체를 구축하고, 전달하여 필요한 경우 중복제거 하실 수 있습니다.

###  **W&B에 저장된 아티팩트 사용하기** <a id="use-an-artifact-stored-in-w-and-b"></a>

```text
artifact = run.use_artifact('bike-dataset:latest')
```

 반환된 아티팩트에 다음의 수단을 호출할 수 있습니다:

```text
datadir = artifact.download(root=None)
```

* 현재 존재하지 않는 모든 아티팩트의 콘텐츠를 다운로드 합니다. 이는 아티팩트 콘텐츠를 포함하고 있는 디렉토리의 경로로 반환됩니다. root를 설정하여 다운로드 경로를 명확하게 지정하실 수 있습니다.

```text
path = artifact.get_path(name)
```

* 경로 `name`에 파일만 가져옵니다. 다음 수단을 통해서 `Entry` 객체를 반환합니다.
  * **Entry.download\(\)**: 경로 `name`의 아티팩트에서 파일을 다운로드합니다.
  * **Entry.ref\(\)**: 엔트리가 `add_reference`를 사용하여 참조로 저장되어 있다면, URL을 반환합니다.

 W&B가 처리 방식을 알고 있는 스킴\(scheme\)을 포함한 참조는 아티팩트 파일처럼 다운로드할 수 있습니다. 소비자 API \(consumer API\) 또한 동일합니다.

### **아티팩트 구축 및 사용하기** <a id="construct-and-use-an-artifact"></a>

 아티팩트 객체를 구축하고 **use\_artifact**로 전달할 수 있습니다. 아직 아티팩트가 존재하지 않는 경우, W&B에 아티팩트가 생성됩니다. 이는 멱등성\(idempotent\)이며, 따라서 여러분이 원하시는 횟수만큼 하실 수 있습니다. `model.h5`의 콘텐츠가 동일하게 유지되는 한, 아티팩트는 오직 한 번 생성됩니다.

```text
artifact = wandb.Artifact('reference model')artifact.add_file('model.h5')run.use_artifact(artifact)
```

###  **실행 외부에서 아티팩트 다운로드하기** <a id="download-an-artifact-outside-of-a-run"></a>

```text
api = wandb.Api()artifact = api.artifact('entity/project/artifact:alias')artifact.download()
```

## **아티팩트 업데이트하기** <a id="updating-artifacts"></a>

아티팩트의 `description`, `metadata`, `aliases`를 희망 값\(desired values\)으로 설정한 후 `save()`를 호출 함으로써 아티팩트의 설명, 메타데이터, 별칭을 업데이트하실 수 있습니다.

```text
api = wandb.Api()artifact = api.artifact('bike-dataset:latest')​# Update the descriptionartifact.description = "My new description"​# Selectively update metadata keysartifact.metadata["oldKey"] = "new value"​# Replace the metadata entirelyartifact.metadata = {"newKey": "new value"}​# Add an aliasartifact.aliases.append('best')​# Remove an aliasartifact.aliases.remove('latest')​# Completely replace the aliasesartifact.aliases = ['replaced']​# Persist all artifact modificationsartifact.save()
```

## **아티팩트 그래프 트래버스\(traverse\)하기** <a id="traversing-the-artifact-graph"></a>

 W&B는 특정 실행이 로그한 아티팩트 및 특정 실행이 사용한 아티팩트를 자동으로 추적합니다. 다음의 API를 통해 이 그래프를 탐색할 수 있습니다:

```text
api = wandb.Api()​artifact = api.artifact('data:v0')​# Walk up and down the graph from an artifact:producer_run = artifact.logged_by()consumer_runs = artifact.used_by()​# Walk up and down the graph from a run:logged_artifacts = run.logged_artifacts()used_artifacts = run.used_artifacts()
```

## **사용하지 않는 버전 정리** <a id="cleaning-up-unused-versions"></a>

시간이 경과함에 따라 아티팩트가 발전하면, UI를 복잡하게 하고 저장공간을 잡아먹는 수많은 버전을 가지게 될 수 있습니다. 오직 아티팩트의 가장 최근 버전만이 유용한 \(가장 최근에 태그된 버전\) 모델 체크포인트에 대한 아티팩트를 사용하는 경우 특히 그렇습니다. W&B를 통해 이러한 필요 없는 버전을 쉽게 정리하실 수 있습니다:

```text
api = wandb.Api()​artifact_type, artifact_name = ... # fill in the desired type + namefor version in api.artifact_versions(artifact_type, artifact_name):    # Clean up all versions that don't have an alias such as 'latest'.    if len(version.aliases) == 0:        version.delete()
```

## **데이터 프라이버시** <a id="data-privacy"></a>

아티팩트는 안전한 API-수준 액세스 제어를 사용합니다. 유휴 상태 또는 전송 중에 파일은 암호화됩니다. 아티팩트는 또한 파일 콘텐츠를 W&B에 전송하지 않고 개인 버킷에 대한 참조를 추적합니다. 또는 개인 클라우드 및 온프렘\(on-prem\) 설치에 대해 문의하고 싶으시다면 contact@wandb.com으로 연락하시기 바랍니다.

## **그래프 탐색** <a id="explore-the-graph"></a>

 아티팩트의 그래프 탭에서 탐색하려면 "Explode"를 클릭하여 각 작업 유형 및 아티팩트 유형의 모든 개별 인스턴스를 살펴볼 수 있습니다. 그런 다음 노드를 클릭하여 새 탭에 해당 실행 또는 아티팩트를 엽니다. 이 [예시 그래프 페이지](https://wandb.ai/shawn/detectron2-11/artifacts/dataset/furniture-small-val/06d5ddd4deeb2a6ebdd5/graph)에서 직접 해보시기 바랍니다.  


