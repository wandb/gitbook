---
description: 현재 실행을 연결할 파일을 클라우드에 저장합니다
---

# wandb.save\(\)

 실행과 연관된 파일을 저장하는 방법으로 두 가지 가 있습니다.

1.  `wandb.save(파일이름)` 사용히가
2. wandb run directory에 파일을 넣으시면 실행이 끝 날 때 업로드 됩니다

{% hint style="info" %}
실행을 [재개\(resuming\)](https://docs.wandb.com/library/advanced/resuming) 하는 경우, callingwandb.restore\(파일이름\)를 통해 파일을 복구할 수 있습니다
{% endhint %}

파일이 작성되는 동안 동기화 하고 싶으시다면, `wandb.save`에서 파일이름 또는 글로브\(glob\)에 지정할 수 있습니다.

##  **wandb.save 예시**

전체 작업 예시에 대한 [이 리포트](https://app.wandb.ai/lavanyashukla/save_and_restore/reports/Saving-and-Restoring-Models-with-W%26B--Vmlldzo3MDQ3Mw)를 확인 하세요.

```python
# Save a model file from the current directory
wandb.save('model.h5')

# Save all files that currently exist containing the substring "ckpt"
wandb.save('../logs/*ckpt*')

# Save any files starting with "checkpoint" as they're written to
wandb.save(os.path.join(wandb.run.dir, "checkpoint*"))
```

{% hint style="info" %}
W&B의 로컬 실행 디렉토리는 기본값으로 여러분의 스크립트와 관련된 ./wandb directory 내부에 있으며, 경로는 run-20171023\_105053-3o4933r0 와 유사합니다. 여기서 20171023\_105053는 타임스탬프이며, 3o4933r0 실행의 이름입니다. wandb.init의 WANDB\_DIR 환경 변수 또는 dir 키워드 전달인자를 절대 경로로 설정할 수 있으며, 파일은 대신 해당 디렉토리 내에 작성됩니다.
{% endhint %}

## **wandb 실행 디렉토리\(wandb run directory\)에 파일 저장 예시**

model.h5" 파일은 wand.run.dir에 저장되며, 훈련이 끝날 때 업로드 됩니다.

```python
import wandb
wandb.init()

model.fit(X_train, y_train,  validation_data=(X_test, y_test),
    callbacks=[wandb.keras.WandbCallback()])
model.save(os.path.join(wandb.run.dir, "model.h5"))
```

다음은 공개 예시 페이지 입니다. files\(파일\) 탭에서 model-best.h5가 있음을 확인하실 수 있습니다. 이 기능은 기본값으로 Keras 통합ㄴ에 의해 자동으로 저장되지만, 체크포인트를 수동으로 저장하실 수도 있으며, 저희는 여러분을 위해 실행과 관련하여 저장해 드립니다.

 [라이브 예시 보기 →](https://app.wandb.ai/wandb/neurips-demo/runs/206aacqo/files)​

![](../.gitbook/assets/image%20%2839%29%20%286%29%20%281%29%20%285%29.png)

## **공통 질문**

###  **특정 파일 무시하기**

 `wandb/settings` 파일을 편집하고 ignore\_globs를 콤마로 구분된 [globs](https://en.wikipedia.org/wiki/Glob_%28programming%29)의 리스트와 동일하게 설정하실 수 있습니다. 또한 **WANDB\_IGNORE\_GLOBS** 환경 변수를 설정하실 수도 있습니다. 일반적인 사용 케이스는 저희가 자동으로 생성하는 git patch 업로드 되지 않도록 방지하는 것입니다. 즉,**WANDB\_IGNORE\_GLOBS=\*.patch** 입니다.  


###  **실행 종료 전에 파일 동기화 하기**

 장시간 실행하시는 경우, 실행을 종료하기 이전에 모델 체크포인트\(model checkpoints\)와 같은 파일이 클라우드에 업로드 된 것을 확인 하실 수 있습니다. 기본값으로 저희는 실행이 끝날 때 까지 대부분의 파일을 업로드를 위해 대기합니다. `wandb.save('*.pth')` 또는 `wandb.save('latest.pth')`만 스크립트에 추가하셔서 해당 파일이 작성되거나 업데이트 될 때마다 업로드 하실 수 있습니다.

###  **파일 저장을 위한 디렉토리 변경**

AWS S3 또는 Google Cloud Storage에 파일을 저장하도록 기본값을 설정한 경우, 다음과 같은 오류가 발생할 수 있습니다.

`events.out.tfevents.1581193870.gpt-tpu-finetune-8jzqk-2033426287 는 클라우드 저장소 url이므로, 파일을 wandb에 저장할 수 없습니다`.

TensorBoard 이벤트 파일\(events file\) 또는 동기화할 다른 파일의 로그 디렉토리를 변경하려는 경우, 파일을 wandb.run.dir에 저장하시면, 저희 클라우드에 동기화 됩니다.

###  **실행이름 가져오기**

I스크립트 내에서 실행이름을 사용하려는 경우, `wandb.run.name`을 사용하시면 실행이름, 예를 들면, "blissful-waterfall-2"을 얻으실 수 있습니다.

표시 이름\(display name\)에 액세스 하기 전에 실행에 저장을 요청하셔야 합니다.

```text
run = wandb.init(...)
run.save()
print(run.name)
```

###  **저장된 모든 파일을 wandb에 푸시하기**

wandb.init 다음에 스크립트 상단의 `wandb.save("*.pt")`를 한 번 호출하면, 해당 패턴과 일치하는 모든 파일이 wandb.run.dir에 기록되는 즉시 저장됩니다.

### **클라우드 스토리지에 동기화 된 로컬 파일 제거하기**

 클라우드 스토리지에 이미 동기화된 로컬 파일을 제거하기 위해 실행할 수 있는 명령어로 wandb gc 가 있습니다. 사용법에 대한 자세한 정보는 \``wandb gc` —help 에서 확인하실 수 있습니다.

