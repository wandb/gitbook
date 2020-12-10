---
description: 다음은 Python API를 사용하여 W&B에서 데이터를 풀 다운(pull down)하는 몇 가지 일반적인 사용 사례입니다.
---

# Data Export API Examples

###  **실행 경로 찾기**

공용 API를 사용하려면, `<entity>/<project>/<run_id>`인 **Run Path**가 자주 필요합니다. 앱에서 실행을 열고 **Overview\(개요\)** 탭을 클릭하여 실행에 대한 실행 경로를 확인합니다.

###  **실행에서 메트릭 읽기**

 이 예시는 //에 저장된 실행에 대한 `wandb.log({"accuracy": acc})`를 통해 저장된 타임스탬프 및 정확도를 출력합니다.

```python
import wandb
api = wandb.Api()

run = api.run("<entity>/<project>/<run_id>")
if run.state == "finished":
   for i, row in run.history().iterrows():
      print(row["_timestamp"], row["accuracy"])
```

###  **두 개의 실행 비교하기**

run1와 run2 사이에 다른 구성 매개변수\(config parameters\)를 출력합니다.

```python
import wandb
api = wandb.Api()

# replace with your <entity_name>/<project_name>/<run_id>
run1 = api.run("<entity>/<project>/<run_id>")
run2 = api.run("<entity>/<project>/<run_id>")

import pandas as pd
df = pd.DataFrame([run1.config, run2.config]).transpose()

df.columns = [run1.name, run2.name]
print(df[df[run1.name] != df[run2.name]])
```

출력:

```text
              c_10_sgd_0.025_0.01_long_switch base_adam_4_conv_2fc
batch_size                                 32                   16
n_conv_layers                               5                    4
optimizer                             rmsprop                 adam
```

###  **실행에 대한 메트릭 업데이트 \(실행이 종료된 후\)** 

 이 예는 이전 실행의 정확도를 0.9로 설정합니다. 또한, 이전 실행의 정확도 히스토그램\(accuracy histogram\)을 numpy\_array의 히스토그램으로 수정합니다

```python
import wandb
api = wandb.Api()

run = api.run("<entity>/<project>/<run_id>")
run.summary["accuracy"] = 0.9
run.summary["accuracy_histogram"] = wandb.Histogram(numpy_array)
run.summary.update()
```

###  **실행에서 config\(구성\) 업데이트**

 이 예시는 구성 설정\(configuration settings\) 중 하나를 업데이트 합니다

```python
import wandb
api = wandb.Api()
run = api.run("<entity>/<project>/<run_id>")
run.config["key"] = 10
run.update()
```

###  **단일 실행에서 CSV 파일로 메트릭 내보내기**

이 스크립트는 단일 실행에 대하여 저장된 모든 메트릭을 찾고, CSV에 저장합니다.

```python
import wandb
api = wandb.Api()

# run is specified by <entity>/<project>/<run id>
run = api.run("<entity>/<project>/<run_id>")

# save the metrics for the run to a csv file
metrics_dataframe = run.history()
metrics_dataframe.to_csv("metrics.csv")
```

###  **샘플링 없이 대형 단일 실행에서 메트릭 내보내기**

기본값 히스토리 방법\(history method\)는 메트릭을 고정된 수의 샘플로 샘플링합니다 \(기본값은 500이며, samples 전달인자를 통해 이를 변경할 수 있습니다\). 대형 실행에서 모든 데이터를 내보내기 하려면, run.scan\_history\(\) 방법을 사용하실 수 있습니다. 이 스크립트는 모든 손실 메트릭을 더 긴 실행에 대한 변수 손실\(variable losses\)로 로드합니다.

```python
import wandb
api = wandb.Api()

run = api.run("<entity>/<project>/<run_id>")
history = run.scan_history()
losses = [row["Loss"] for row in history]
```

###  **프로젝트의 모든 실행에서 메트릭을 CSV 파일로 내보내기**

이 스크립트는 프로젝트를 찾고 이름, 구성\(configs\) 및 요약 통계와 함께 실행의 CSV를 출력합니다.

```python
import wandb
api = wandb.Api()

runs = api.runs("<entity>/<project>")
summary_list = [] 
config_list = [] 
name_list = [] 
for run in runs: 
    # run.summary are the output key/values like accuracy.  We call ._json_dict to omit large files 
    summary_list.append(run.summary._json_dict) 

    # run.config is the input metrics.  We remove special values that start with _.
    config_list.append({k:v for k,v in run.config.items() if not k.startswith('_')}) 

    # run.name is the name of the run.
    name_list.append(run.name)       

import pandas as pd 
summary_df = pd.DataFrame.from_records(summary_list) 
config_df = pd.DataFrame.from_records(config_list) 
name_df = pd.DataFrame({'name': name_list}) 
all_df = pd.concat([name_df, config_df,summary_df], axis=1)

all_df.to_csv("project.csv")
```

###  **실행에서 파일 다운로드하기**

파일 cifar 프로젝트의 실행 ID uxte44z7와 연관된 "model-best.h5"를 찾고 로컬로 저장합니다.

```python
import wandb
api = wandb.Api()
run = api.run("<entity>/<project>/<run_id>")
run.file("model-best.h5").download()
```

###  **실행에서 모든 파일 다운로드하기**

 실행 ID uxte44z7와 관련된 모든 파일을 찾고, 로컬로 저장합니다. \(참조: 명령줄에서 wandb restore 를 실행하여 이를 달성할 수 있습니다.

```python
import wandb
api = wandb.Api()
run = api.run("<entity>/<project>/<run_id>")
for file in run.files():
    file.download()
```

###  **최고 모델 파일 다운로드하기**

```python
import wandb
api = wandb.Api()
sweep = api.sweep("<entity>/<project>/<sweep_id>")
runs = sorted(sweep.runs, key=lambda run: run.summary.get("val_acc", 0), reverse=True)
val_acc = runs[0].summary.get("val_acc", 0)
print(f"Best run {runs[0].name} with {val_acc}% validation accuracy")
runs[0].file("model-best.h5").download(replace=True)
print("Best model saved to model-best.h5")
```

###  **특정 스윕에서 실행 가져오기**

```python
import wandb
api = wandb.Api()
sweep = api.sweep("<entity>/<project>/<sweep_id>")
print(sweep.runs)
```

###  **시스템 메트릭 데이터 다운로드하기**

실행에 대한 모든 시스템 메트릭과 함께 데이터프레임을 얻을 수 있습니다.

```python
import wandb
api = wandb.Api()
run = api.run("<entity>/<project>/<run_id>")
system_metrics = run.history(stream = 'events')
```

###  **요약 메트릭 업데이트하기**

 사전을 전달하여 요약 메트릭을 업데이트합니다.

```python
summary.update({“key”: val})
```

### **실행을 실행한 명령 가져오기**

각 실행은 실행 개요\(run overview\) 페이지에서 실행된 명령을 캡처합니다. 이 명령을 API에서 풀 다운 하려면 다음을 실행할 수 있습니다:

```python
api = wandb.Api()
run = api.run("username/project/run_id")
meta = json.load(run.file("wandb-metadata.json").download())
program = ["python"] + [meta["program"]] + meta["args"]
```

### **히스토리에서 페이지 지정된 데이터 가져오기**

 백엔드에서 메트릭이 느리게 전달되거나 API 요청이 타임 아웃 된 경우, 개별 요청이 타임아웃 되지 않도록 `scan_history`에서 페이지 사이즈를 줄일 수 있습니다. 기본값 페이지 사이즈는 1000이며, 따라서 다른 사이즈로 실험하여 가장 잘 작동하는 것을 확인할 수 있습니다:

```python
api = wandb.Api()
run = api.run("username/project/run_id")
run.scan_history(keys=sorted(cols), page_size=100)
```



