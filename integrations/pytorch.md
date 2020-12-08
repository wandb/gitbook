---
description: How to integrate a PyTorch script to log metrics to W&B
---

# PyTorch

  W&B는 PyTorch에 대한 최고 수준의 지원을 제공합니다. 자동으로 경사\(gradients\)를 로그하고 네트워크 토폴로지\(network topology\)를 저장하려면 `watch`를 호출하고 여러분의 PyTorch 모델에서 전달할 수 있습니다.

```python
import wandb
wandb.init(config=args)

# Magic
wandb.watch(model)

model.train()
for batch_idx, (data, target) in enumerate(train_loader):
    output = model(data)
    loss = F.nll_loss(output, target)
    loss.backward()
    optimizer.step()
    if batch_idx % args.log_interval == 0:
        wandb.log({"loss": loss})
```

> 경사\(gradients\), 메트릭 및 그래프는 정방향 및 역방향 패스 후 `wandb.log`가 호출 될 때 기록됩니다.

 wandb와 PyTorch 통합에 대한 통합적인 예시는 [Colab notebook](https://github.com/wandb/examples/blob/master/examples/pytorch/pytorch-intro/intro.ipynb)를 참조하십시오. 또한, [예시 프로젝트\(example projects\)](https://docs.wandb.com/examples) 섹션에서 더 많은 예시를 찾으실 수 있습니다.  


###  **옵션**

기본값으로 훅은 오직 경사\(gradients\)만을 로그합니다.

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#xC804;&#xB2EC;&#xC778;&#xC790;</th>
      <th style="text-align:left">&#xC635;&#xC158;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">log</td>
      <td style="text-align:left">
        <ul>
          <li>all: &#xACBD;&#xC0AC;(gradients) &#xBC0F; &#xB9E4;&#xAC1C;&#xBCC0;&#xC218;&#xC758;
            &#xD788;&#xC2A4;&#xD1A0;&#xADF8;&#xB7A8; &#xB85C;&#xAE45;</li>
          <li>&#xACBD;&#xC0AC;(gradients) (&#xAE30;&#xBCF8;&#xAC12;)</li>
          <li>&#xB9E4;&#xAC1C;&#xBCC0;&#xC218; (&#xBAA8;&#xB378;&#xC758; &#xAC00;&#xC911;&#xCE58;)</li>
          <li>&#xC5C6;&#xC74C;(None)</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">log_freq</td>
      <td style="text-align:left">&#xC815;&#xC218;(integer) (&#xAE30;&#xBCF8;&#xAC12; 100): &#xB85C;&#xAE45;
        &#xACBD;&#xC0AC;(logging gradients)&#xAC04; &#xB2E8;&#xACC4;&#xC758; &#xC218;</td>
    </tr>
  </tbody>
</table>

## **이미지**

 이미지 데이터가 포함된 PyTorch tensor를 `wandb.Image`로 전달할 수 있으며, torchvision\(토치비전\) utils를 통해 자동으로 로그됩니다.

 이미지를 로그하고 미디어 패널에서 보려면, 다음의 신택스를 사용할 수 있습니다:

```python
wandb.log({"examples" : [wandb.Image(i) for i in images]})
```

##  **다양한 모델**

동일한 스크립트에서 여러 모델을 추적해야 하는 경우, 각 모델 별로 wandb.watch\(\)를 별도로 월링\(wall\)할 수 있습니다.

##  **예시**

저희는 여러분을 위한 통합\(integration\)의 작동 방식을 확인 할 수 있는 위한 몇 가지 예시를 만들어보았습니다.

* [colab에서 실행 \(Run in colab](https://github.com/wandb/examples/blob/master/examples/pytorch/pytorch-intro/intro.ipynb)\): 시작하기 위한 간단한 notebook 예제
* [Github의 예시 \(Example on Githuㅠ\)](https://github.com/wandb/examples/blob/master/examples/pytorch/pytorch-cnn-mnist/main.py): MNIST 예시
* ​[Wandb 대시보드\(Dashboard\)](https://app.wandb.ai/wandb/pytorch-mnist/runs/): W&B에 대한 결과 보기

