# Artifacts FAQs

## **어떻게 아티팩트를 프로그래밍 방식으로 업데이트할 수 있나요?**

아티팩트 특성을 원하는 값으로 설정하고, `.save()`를 호출하여  \(`description`, `metadata`, `aliases`와 같은\) 다양한 아티팩트 특성을 스크립트에서 직접 업데이트할 수 있습니다:

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

## **실행을 시작하지 않고 아티팩트를 로그하려면 어떻게 해야 하나요?**

 `wandb artifact put` 명령을 사용하여 업로드를 처리하기 위해 스크립트를 작성할 필요 없이 아티팩트를 로그할 수 있습니다.

```text
$ wandb artifact put -n project/artifact -t dataset mnist/
```

마찬가지로, 다음의 명령을 통해 아티팩트를 디렉토리에 다운로드할 수 있습니다:

```text
$ wandb artifact get project/artifact:alias --root mnist/
```

## **실행의 외부에서 아티팩트를 다운로드하려면 어떻게 해야 하나요?**

`wandb artifact get` 명령을 사용하여 아티팩트를 다운로드할 수 있습니다:

```text
$ wandb artifact get project/artifact:alias --root mnist/
```

 또는 공용 API를 사용하는 스크립트를 작성하실 수도 있습니다:

```text
api = wandb.Api()

artifact = api.artifact('data:v0')
artifact_dir = artifact.checkout()
```

## **사용하지 않은 아티팩트 버전을 정리하는 방법은 무엇인가요?**

시간이 경과함에 따라 아티팩트가 발전하면, UI를 복잡하게 하고 저장공간을 잡아먹는 수많은 버전을 가지게 될 수 있습니다. 오직 아티팩트의 가장 최근 버전만이 유용한 \(가장 최근에 태그된 버전\) 모델 체크포인트에 대한 아티팩트를 사용하는 경우 특히 그렇습니다. 별칭\(aliases\)이 없는 아티팩트의 모든 버전을 삭제하는 방법은 다음과 같습니다:

```python
api = wandb.Api()

artifact_type, artifact_name = ... # fill in the desired type + name
for version in api.artifact_versions(artifact_type, artifact_name):
    # Clean up all versions that don't have an alias such as 'latest'.
		#
		# NOTE: You can put whatever deletion logic you want here.
    if len(version.aliases) == 0:
        version.delete()
```

## **아티팩트 그래프를 트래버스\(traverse, 탐색\) 하는 방법은 무엇인가요?**

W&B는 자동으로 주어진 실행이 로그한 아티팩트와 주어진 실행에서 사용한 아티팩트를 추적합니다. API를 통해 프로그래밍 방식으로 이 그래프를 탐색할 수 있습니다.

```python
api = wandb.Api()

artifact = api.artifact('data:v0')

# Walk up and down the graph from an artifact:
producer_run = artifact.logged_by()
consumer_runs = artifact.used_by()

# Walk up and down the graph from a run:
logged_artifacts = run.logged_artifacts()
used_artifacts = run.used_artifacts()
```

## **로컬 아티팩트 캐시를 정리하는 방법은 무엇인가요?**

 W&B는 아티팩트 파일을 캐시하여 여러 파일을 공통으로 공유하는 버전 간 다운로드 속도를 높입니다. 그러나 시간이 지남에 따라, 이 캐시 디렉토리는 커질 수 있습니다. 다음의 명령을 실행하여 캐시를 축소하여, 최근에 사용되지 않은 파일을 정리할 수 있습니다:

```text
$ wandb artifact cache cleanup 1GB
```

위의 명령을 실행하여 캐시 크기를 1GB로 제한하고 마지막으로 액세스한 시간을 기준으로 삭제할 파일의 우선순위를 지정합니다.

## **분산 프로세스를 통해 아티팩트를 로그하는 방법은 무엇인가요?**

 대형 데이터세트 또는 분산 훈련\(distributed training\)의 경우, 여러 병렬 실행\(parallel runs\)은 단일 아티팩트에 기여해야 할 수 있습니다. 다음의 패턴을 사용하여 이러한 병렬 아티팩트를 구축할 수 있습니다:

```python
import wandb
import time

# We will use ray to launch our runs in parallel
# for demonstration purposes. You can orchestrate
# your parallel runs however you want.
import ray

ray.init()

artifact_type = "dataset"
artifact_name = "parallel-artifact"
table_name = "distributed_table"
parts_path = "parts"
num_parallel = 5

# Each batch of parallel writers should have its own
# unique group name.
group_name = "writer-group-{}".format(round(time.time()))

@ray.remote
def train(i):
  """
  Our writer job. Each writer will add one image to the artifact.
  """
  with wandb.init(group=group_name) as run:
    artifact = wandb.Artifact(name=artifact_name, type=artifact_type)
    
    # Add data to a wandb table. In this case we use example data
    table = wandb.Table(columns=["a", "b", "c"], data=[[i, i*2, 2**i]])
    
    # Add the table to folder in the artifact
    artifact.add(table, "{}/table_{}".format(parts_path, i))
    
    # Upserting the artifact creates or appends data to the artifact
    run.upsert_artifact(artifact)
  
# Launch your runs in parallel
result_ids = [train.remote(i) for i in range(num_parallel)]

# Join on all the writers to make sure their files have
# been added before finishing the artifact. 
ray.get(result_ids)

# Once all the writers are done writing, finish the artifact
# to mark it ready.
with wandb.init(group=group_name) as run:
  artifact = wandb.Artifact(artifact_name, type=artifact_type)
  
  # Create a "PartitionTable" pointing to the folder of tables
  # and add it to the artifact.
  artifact.add(wandb.data_types.PartitionedTable(parts_path), table_name)
  
  # Finish artifact finalizes the artifact, disallowing future "upserts"
  # to this version.
  run.finish_artifact(artifact)
```

## **스윕에서의 가장 적합한 실행에서 아티팩트를 찾는 방법은 무엇인가요?**

스윕 내에서 모든 실행이 동일 아티팩트의 버전을 생산하게 하는 것 대신 개별 실행이 자체 아티팩트를 내보내게 할 수 있습니다. 이 패턴을 통해, 다음의 코드를 사용하여 스윕에서 가장 뛰어난 실행과 관련된 아티팩트를 검색할 수 있습니다:

```python
import wandb
api = wandb.Api()
sweep = api.sweep("<entity>/<project>/<sweep_id>")
runs = sorted(sweep.runs, key=lambda run: run.summary.get("val_acc", 0), reverse=True)
best_run = runs[0]
for artifact in best_run.logged_artifacts():
  artifact_path = artifact.download()
  print(artifact_path)
```

## **어떻게 코드를 저장하나요?**‌

`wandb.init`의 `save_code=True`를 사용하여 메인 스크립트 또는 실행을 시작하는 노트북을 저장합니다. 모든 코드를 실행에 저장하려면 아티팩트와 함께 코드를 버저닝합니다. 이에 대한 예시는 다음과 같습니다:

```python
code_artifact = wandb.Artifact(type='code')
code_artifact.add_dir('.')
wandb.log_artifact(code_artifact)
```

##  **아티팩트 파일은 어디에 저장되나요?**

기본값으로 W&B는 아티팩트 파일을 미국에 위치한 개인 Google Cloud Storage 버킷에 저장합니다. 모든 파일은 유휴 및 전송 시에 암호화됩니다.

민감한 파일의 경우, 개인 W&B 설치 또는 참조 아티팩트를 사용하시기 바랍니다.

## **언제 아티팩트 파일이 삭제되나요?**

 W&B는 연속 아티팩트 버전 간의 중복을 최소화하는 방식으로 아티팩트 파일을 저장합니다. 즉, 1,000개 이미지의 데이터세트를 로그하고 100개의 추가 이미지를 더한 후속 버전을 로그하는 경우, 두 번째 버전은 도입된 파일과 크기가 정확히 동일합니다.

아티팩트 버전을 삭제하는 경우 W&B는 어떠한 파일을 삭제해도 되는지 확인합니다. 즉, 이전 또는 후속 아티팩트 버전이 파일을 사용하지 않음을 보장합니다. 제거해도 되는 경우, 파일은 즉시 삭제되며 서버에 흔적이 남지 않습니다.

## **제 아티팩트에 액세스할 수 있는 사람은 누구인가요?**

아티팩트는 부모 프로젝트의 액세스를 물려받습니다:

* 비공개 프로젝트인 경우, 프로젝트팀의 구성원만 해당 아티팩트에 액세스할 수 있습니다.
* 공공 프로젝트\(public projects\)의 경우, 모든 사용자가 아티팩트에 대한 읽기 권한을 가지지만, 오직 프로젝트팀의 구성원만이 아티팩트를 생성 또는 수정할 수 있습니다.
* 공개 프로젝트\(open projects\)의 경우, 모든 사용자가 아티팩트에 대한 읽기 및 쓰기 권한을 가집니다.

