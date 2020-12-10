# Skorch

Skorch와 함께 Weights & Biases를 사용해서 각 에포크\(epoch\) 후에 모든 모델 퍼포먼스 메트릭, 모델 토폴로지\(topology\)와 함께 최고 퍼포먼스의 모델을 자동으로 로그하고 리소스를 계산하실 수 있습니다. wandb\_run.dir에 저장된 모든 파일은 자동으로 W&B 서버로 로그됩니다.

 [예시 실행](https://app.wandb.ai/borisd13/skorch/runs/s20or4ct?workspace=user-borisd13)을 참조하세요.

## **매개변수**

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>&#xB9E4;&#xAC1C;&#xBCC0;&#xC218;</b>
      </th>
      <th style="text-align:left">&#xC124;&#xBA85;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">
        <p><b>wandb_run</b>:</p>
        <p>wandb.wandb_run.Run</p>
      </td>
      <td style="text-align:left">&#xB370;&#xC774;&#xD130; &#xB85C;&#xADF8;&#xC5D0; &#xC0AC;&#xC6A9;&#xB418;&#xB294;
        wandb &#xC2E4;&#xD589;</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>save_model<br /></b>bool (default=True)</td>
      <td style="text-align:left">&#xCD5C;&#xACE0; &#xBAA8;&#xB378;&#xC758; &#xCCB4;&#xD06C;&#xD3EC;&#xC778;&#xD2B8;
        &#xC800;&#xC7A5; &#xBC0F; W&amp;B &#xC11C;&#xBC84;&#xC5D0;&#xC11C; &#xC5EC;&#xB7EC;&#xBD84;&#xC758;
        &#xC2E4;&#xD589;&#xC73C;&#xB85C; &#xC5C5;&#xB85C;&#xB4DC; &#xD560; &#xC9C0;
        &#xC5EC;&#xBD80;&#xB97C; &#xB098;&#xD0C0;&#xB0C5;&#xB2C8;&#xB2E4;.</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>keys_ignored<br /></b>str or list of str (default=None)</td>
      <td style="text-align:left">tensorboard&#xC5D0; &#xB85C;&#xADF8;&#xB418;&#xC9C0; &#xC54A;&#xC544;&#xC57C;
        &#xD558;&#xB294; &#xD0A4; &#xB610;&#xB294; &#xD0A4;&#xC758; &#xB9AC;&#xC2A4;&#xD2B8;.
        &#xC0AC;&#xC6A9;&#xC790;&#xAC00; &#xC81C;&#xACF5;&#xD55C; &#xD0A4; &#xC774;
        &#xC678;&#xC5D0;&#xB3C4;, &#x2018;event_&#x2019;&#xB85C; &#xC2DC;&#xC791;&#xD558;&#xAC70;&#xB098;
        &#x2018;_best&#x2019;&#xB85C; &#xB05D;&#xB098;&#xB294; &#xACBD;&#xC6B0;&#xC640;
        &#xAC19;&#xC740; &#xD0A4;&#xB294; &#xAE30;&#xBCF8;&#xAC12;&#xC73C;&#xB85C;
        &#xBB34;&#xC2DC;&#xB429;&#xB2C8;&#xB2E4;.</td>
    </tr>
  </tbody>
</table>

##  **예시 코드**

 통합의 작동 방식을 볼 수 있는 몇 가지 예시를 만들었습니다:

* [Colab](https://colab.research.google.com/drive/1Bo8SqN1wNPMKv5Bn9NjwGecBxzFlaNZn?usp=sharing): 통합을 시도하는 간단한 데모
* [단계별 가이드](https://app.wandb.ai/cayush/uncategorized/reports/Automate-Kaggle-model-training-with-Skorch-and-W%26B--Vmlldzo4NTQ1NQ): Skorch 모델 퍼포먼스 추적에 대한 가이드

```python
# Install wandb
... pip install wandb

import wandb
from skorch.callbacks import WandbLogger

# Create a wandb Run
wandb_run = wandb.init()
# Alternative: Create a wandb Run without a W&B account
wandb_run = wandb.init(anonymous="allow")

# Log hyper-parameters (optional)
wandb_run.config.update({"learning rate": 1e-3, "batch size": 32})

net = NeuralNet(..., callbacks=[WandbLogger(wandb_run)])
net.fit(X, y)
```

##  **수단**

| **수단** | 설명 |
| :--- | :--- |
| `initialize`\(\) | callback의 초기 상태 \(재\)설정 |
| `on_batch_begin`\(net\[, X, y, training\]\) | 각 배치의 시작 부분에서 호출됨 |
| `on_batch_end`\(net\[, X, y, training\]\) | 각 배치의 끝 부분에서 호출됨 |
| `on_epoch_begin`\(net\[, dataset\_train, …\]\) | 각 에포크의 시작 부분에서 호출됨 |
| `on_epoch_end`\(net, \*\*kwargs\) | 마지막 히스토리 단계의 값 로깅 및 최고 모델 저장 |
| `on_grad_computed`\(net, named\_parameters\[, X, …\]\) | 경사\(gradients\)가 계산 된 후 및 업데이트 단계가 수행되기 전에 배치당 한 번 호출됨 |
| `on_train_begin`\(net, \*\*kwargs\) | 모델 토폴로지 로깅 및 경사에 대한 훅 추가 |
| `on_train_end`\(net\[, X, y\]\) | 훈련의 끝 부분에서 호출됨 |

