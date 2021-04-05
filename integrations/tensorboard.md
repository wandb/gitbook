# TensorBoard

W&B는 스크립트의 모든 메트릭을 자동으로 저희의 네이티브 차트\(native chart\)에 로그 되도록 TensorBoard 패칭\(patching\)을 지원합니다.

```python
import wandb
wandb.init(sync_tensorboard=True)
```

저희는 모든 버전의 TensorFlow와 함께 TensorBoard를 지원합니다. 만약 다른 프레임워크와 TensorBoard를 사용 중이신 경우, W&B는 1.14 이상의 TensorBoard\(TensorBoard &gt; 1.14 with PyTorch\)와 PyTorch 및 TensorBoardX를 지원합니다.

### **사용자 정의 메트릭**

TensorBoard에 로그되지 않은 추가 사용자 정의 메트릭을 로그해야 하는 경우, TensorBoard가 사용하는 동일 스텝 전달인자\(same step argument\)를 포함한 코드의 wandb.log를 호출하실 수 있습니다. 즉, `wandb.log({"custom": 0.8}, step=global_step)`입니다.

###  **고급 구성**

 TensorBoard의 패치 적용 방식에 대한 많은 통제권을 가지고 싶으시다면, 초기화를 위한 `sync_tensorboard=True`를 전달하는 것 대신 `wandb.tensorboard.patch를` 호출하실 수 있습니다. tensorboardX=False를 이 수단에 전달해서 vanilla TensorBoard가 패치되도록 할 수 있으며, 1.14 이상의 TensorBoard\(TensorBoard &gt; 1.14 with PyTorch\)와 PyTorch를 사용하시는 경우, `pytorch=True`를 전달하여 패치되었는지 확인하실 수 있습니다. 어떤 버전의 라이브러리가 가져오게 되었는지 이 두 옵션 모두 스마트 기본값\(smart defaults\)을 가지고 있습니다.  


또한, 기본값으로, `tfevents` 파일과 모든 \*.pbtxt 파일을 동기화합니다. 이를 통해 저희는 `TensorBoard` 인스턴스\(instance\)를 여러분을 대신하여 실행할 수 있습니다. 실행 페이지의 [TensorBoard 탭](https://www.wandb.com/articles/hosted-tensorboard)에서 확인하실 수 있습니다. 이 동작은 save=False를 wandb.tensorboard.patch로 우회하여 비활성화하실 수 있습니다.   
  


```python
import wandb
wandb.init()
wandb.tensorboard.patch(save=False, tensorboardX=True)
```

###  **이전 TensorBoard 실행 동기화하기**

 wandb로 가져오려는 기존 실험이 있는 경우, `wandb sync log_dir`을 실행하실 수 있습니다. 여기서 `log_dir`은 tfevents 파일을 포함한 로컬 디렉토리입니다.

또한 `wandb sync directory_with_tf_event_file`을 실행하실 수도 있습니다.

```python
"""This script will import a directory of tfevents files into a single W&B run.
You must install wandb from a special branch until this feature is merged into the mainline: 
```bash
pip install --upgrade git+git://github.com/wandb/client.git@feature/import#egg=wandb
```

`python no_image_import.py dir_with_tf_event_file`을 통해 이 스크립트를 호출할 수 있습니다. 이를 통해 해당 디렉터리에 있는 이벤트 파일의 메트릭과 함께 wandb에서 단일 실행이 생성됩니다. 여러 디렉터리에서 이 스크립트를 실행 하는 경우, 실행당 한 번만 이 작업을 수행해야 합니다. 따라서 다음과 같이 표시될 수 있습니다.

```python
import glob
for run_dir in glob.glob("logdir-*"):
  subprocess.Popen(["python", "no_image_import.py", run_dir], stderr=subprocess.PIPE, stdout=subprocess.PIPE)
```

""" import glob import os import wandb import sys import time import tensorflow as tf from wandb.tensorboard.watcher import Consumer, Event from six.moves import queue

if len\(sys.argv\) == 1: raise ValueError\("Must pass a directory as the first argument"\)

paths = glob.glob\(sys.argv\[1\]+"/_\*/_.tfevents.\*", recursive=True\) root = os.path.dirname\(os.path.commonprefix\(paths\)\).strip\("/"\) namespaces = {path: path.replace\(root, ""\).replace\( path.split\("/"\)\[-1\], ""\).strip\("/"\) for path in paths} finished = {namespace: False for path, namespace in namespaces.items\(\)} readers = \[\(namespaces\[path\], tf.train.summary\_iterator\(path\)\) for path in paths\] if len\(readers\) == 0: raise ValueError\("Couldn't find any event files in this directory"\)

directory = os.path.abspath\(sys.argv\[1\]\) print\("Loading directory %s" % directory\) wandb.init\(project="test-detection"\)

Q = queue.PriorityQueue\(\) print\("Parsing %i event files" % len\(readers\)\) con = Consumer\(Q, delay=5\) con.start\(\) total\_events = 0 while True:

```text
# Consume 500 events at a time from all readers and push them to the queue
for namespace, reader in readers:
    if not finished[namespace]:
        for i in range(500):
            try:
                event = next(reader)
                kind = event.value.WhichOneof("value")
                if kind != "image":
                    Q.put(Event(event, namespace=namespace))
                    total_events += 1
            except StopIteration:
                finished[namespace] = True
                print("Finished parsing %s event file" % namespace)
                break
if all(finished.values()):
    break
```

print\("Persisting %i events..." % total\_events\) con.shutdown\(\) print\("Import complete"\)

\`\`\`

###  **Google Colab 및 TensorBoard**

 Colab의 명령줄에서 명령을 실행하려면 반드시 !`wandb sync directoryname`을 실행하셔야 합니다. Tensorflow 2.1+에 대한 notebook 환경에서는 현재 tensorboard 동기화가 작동하지 않습니다. 여러분의 Colab에서 이전 버전의 TensorBoard를 사용하거나, `!python your_script.py`와 함께 명령줄에서 스크립트를 실행하실 수 있습니다.

## **W&B는 TensorBoard와 어떻게 다른가요?**

저희는 여러분 모두를 위해 실험 추적 툴을 개선하기 위해 열심히 최선을 다하고 있습니다. 공동창립사들이 W&B 작업을 시작했을 때, OpenAI에서 좌절한 TensorBoard 사용자들을 위한 툴 개발을 위한 영감을 얻었습니다. 다음은 저희가 개선하기 위해 노력하고 있는 몇 가지 항목들입니다.

1. **모델 재현**: Weights & Biases는 실험, 탐색 및 추후 모델 재현에 유용합니다. 저희는 메트릭뿐만 아니라 초매개변수 및 코드의 버전을 캡처하고, 프로젝트를 재현할 수 있도록 모델 체크포인트를 저장하실 수 있습니다
2. **자동 구성:** 프로젝트를 공동작업자에게 넘기거나, 휴가를 낸 경우, W&B는 여러분이 시도한 모든 모델을 쉽게 확인할 수 있도록 해드립니다. 따라서 오래된 실험 재실행에 시간을 허비하지 않으셔도 됩니다.
3. **빠르고 유연한 통합**:  ****5분 안에 W&B를 여러분의 프로젝트에 추가하세요. 저희의 무료 오픈소스 Python 패키지를 설치하시고, 여러분의 코드에 몇 줄만 추가하세요. 그리고 나면 모델을 실행하실 때마다, 훌륭한 로그된 메트릭과 레코드를 가지게 됩니다.
4. **지속적인, 중앙 집중식 대시보드**: 로컬 머신, 랩 클러스터\(lab cluster\) 또는 클라우드의 스팟 인스턴스\(spot instances\) 등 모델을 훈련하는 모든 곳에서 동일한 중앙 집중식 대시보드를 제공합니다. 여러 머신에서 TensorBoard 파일을 복사 및 정리하는 데 시간을 쓰실 필요가 없습니다.
5. **강력한 테이블**: 여러 모델에서의 결과를 검색, 필터, 정렬 및 그룹화합니다. 수 천개의 모델 버전을 살펴보고, 다양한 작업에 대한 최고 퍼포먼스 모델을 쉽게 찾으실 수 있습니다. TensorBoard는 대규모 프로젝트에서 잘 작동하도록 설계되어 있지는 않습니다.
6. **공동작업을 위한 툴**: W&B를 사용해서 복잡한 머신 러닝 프로젝트를 구성하세요. W&B로 링크를 쉽게 공유하실 수 있으며, 개인 팀\(private teams\)를 활용해 모두가 공유 프로젝트에 결과를 보내도록 할 수 있습니다. 또한 리포트\(reports\)를 통해 공동 작업을 지원합니다. 양방향 시각화\(interactive visualizations\)를 추가하고, 마크다운\(markdown\)에서 여러분의 작업 내용을 설명하세요. 작업 로그 보관, 상사와의 결과 공유 또는 여러분의 연구실에 결과를 제시할 수 있는 유용한 방법입니다.

[으로 시작하세요 →](http://app.wandb.ai/)

