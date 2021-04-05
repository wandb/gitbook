# Resuming

 사용자는 `resume=True`를 `wandb.init()`로 전달하여 wandb가 자동으로 실행을 재개하도록 하실 수 있습니다. 프로세스가 제대로 종료되지 않은 경우, 다음에 프로세스를 실행할 때 wandb는 최종 단계에서부터 로깅을 시작합니다. 아래는 Keras의 간단한 예시입니다:

```python
import keras
import numpy as np
import wandb
from wandb.keras import WandbCallback
wandb.init(project="preemptable", resume=True)

if wandb.run.resumed:
    # restore the best model
    model = keras.models.load_model(wandb.restore("model-best.h5").name)
else:
    a = keras.layers.Input(shape=(32,))
    b = keras.layers.Dense(10)(a)
    model = keras.models.Model(input=a,output=b)

model.compile("adam", loss="mse")
model.fit(np.random.rand(100, 32), np.random.rand(100, 10),
    # set the resumed epoch
    initial_epoch=wandb.run.step, epochs=300,
    # save the best model if it improved each epoch
    callbacks=[WandbCallback(save_model=True, monitor="loss")])
```

자동 재개\(automatic resuming\)는 프로세스가 실패한 프로세스와 동일한 파일시스템 위에서 재실행되는 경우에만 작동합니다. 파일시스템을 공유하실 수 없는 경우, 스크립트의 단일 실행에 상응하는 **WANDB\_RUN\_ID**: a globally unique string\(글로벌 고유 스트링\) \(프로젝트 당\). 이는 64자 이하여야 합니다. 모든 비 단어 문자\(non-word characters\)는 대시\(-\)로 변환됩니다.

```python
# store this id to use it later when resuming
id = wandb.util.generate_id()
wandb.init(id=id, resume="allow")
# or via environment variables
os.environ["WANDB_RESUME"] = "allow"
os.environ["WANDB_RUN_ID"] = wandb.util.generate_id()
wandb.init()
```

**WANDB\_RESUME**을 “allow\(허용\)”으로 설정하신 경우, 사용자는 항상 **WANDB\_RUN\_ID**를 고유한 문자열로 설정하실 수 있으며, 프로세스의 재시작은 자동으로 처리됩니다. 

**WANDB\_RESUME을 “must\(필수\)”로 설정하신 경우, 새 실행 자동 생성 대신 재개될 실행이 존재하지 않는 경우, wandb는 오류를 발생시킵니다.**

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#xBC29;&#xBC95;</th>
      <th style="text-align:left">&#xC2E0;&#xD0DD;&#xC2A4;</th>
      <th style="text-align:left">
        <p><b>&#xC7AC;&#xAC1C; &#xC548; &#xD568;</b>
        </p>
        <p><b>(&#xAE30;&#xBCF8;&#xAC12;)</b>
          <br />
        </p>
      </th>
      <th style="text-align:left"><b>&#xD56D;&#xC0C1; &#xC7AC;&#xAC1C;</b>
      </th>
      <th style="text-align:left"><b>&#xC2E4;&#xD589; ID &#xC9C0;&#xC815; &#xC7AC;&#xAC1C;</b>
      </th>
      <th style="text-align:left">&#xB3D9;&#xC77C; &#xB514;&#xB809;&#xD1A0;&#xB9AC;&#xC5D0;&#xC11C; &#xC7AC;&#xAC1C;(Resume
        from same directory)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">command line</td>
      <td style="text-align:left">wandb run --resume=</td>
      <td style="text-align:left">&quot;never&quot;</td>
      <td style="text-align:left">&quot;must&quot;</td>
      <td style="text-align:left">&quot;allow&quot; (Requires WANDB_RUN_ID=RUN_ID)</td>
      <td style="text-align:left">(not available)</td>
    </tr>
    <tr>
      <td style="text-align:left">environment</td>
      <td style="text-align:left">WANDB_RESUME=</td>
      <td style="text-align:left">&quot;never&quot;</td>
      <td style="text-align:left">&quot;must&quot;</td>
      <td style="text-align:left">&quot;allow&quot; (Requires WANDB_RUN_ID=RUN_ID)</td>
      <td style="text-align:left">(not available)</td>
    </tr>
    <tr>
      <td style="text-align:left">init</td>
      <td style="text-align:left">wandb.init(resume=)</td>
      <td style="text-align:left"></td>
      <td style="text-align:left">(not available)</td>
      <td style="text-align:left">resume=RUN_ID</td>
      <td style="text-align:left">resume=True</td>
    </tr>
  </tbody>
</table>

{% hint style="warning" %}
여러 프로세스가 동일 run\_id를 동시에 사용하는 경우, 예상치 못한 결과가 기록되며, 비율 제한\(rate limiting\)이 발생합니다.
{% endhint %}

{% hint style="info" %}
실행을 재개하고 wandb.init\(\)에 지정된 **노트**가 있는 경우, 해당 노트는 사용자가 UI에 추가한 모든 노트를 덮어씁니다.
{% endhint %}

