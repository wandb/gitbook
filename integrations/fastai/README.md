# Fast.ai

모델을 훈련시키기 위해 fastai를 사용하시는 경우, W&B에서 WandbCallback을 사용하여 쉽게 통합하실 수 있습니다. 더 자세한 사항은 다음에서 확인하실 수 있습니다. **​**​  


 우선 Weights & Biases를 설치하고 로그인합니다:

```text
pip install wandb
wandb login
```

  다음, callback을 `learner` 또는 `fit` 방법에 추가합니다:

```python
import wandb
from fastai.callback.wandb import *

# start logging a wandb run
wandb.init(project='my_project')

# To log only during one training phase
learn.fit(..., cbs=WandbCallback())

# To log continuously for all training phases
learn = learner(..., cbs=WandbCallback())
```

{% hint style="info" %}
Fastai 버전1을 사용하시는 경우, 다음을 참조하시기 바랍니다: [Fastai v1 docs](https://docs.wandb.com/library/integrations/fastai/fastai)
{% endhint %}

 `WandbCallback`은 다음의 전달인자를 허용합니다:

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#xC804;&#xB2EC;&#xC778;&#xC790;</th>
      <th style="text-align:left">&#xC124;&#xBA85;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">log</td>
      <td style="text-align:left">&quot;gradients&quot; (&#xAE30;&#xBCF8;&#xAC12;), &quot;parameters(&#xB9E4;&#xAC1C;&#xBCC0;&#xC218;)&quot;,
        &quot;all(&#xC804;&#xBD80;)&quot; &#xB610;&#xB294; None(&#xC5C6;&#xC74C;).
        &#xC190;&#xC2E4;&#xAC12; &#xBC0F; &#xBA54;&#xD2B8;&#xB9AD;&#xC740; &#xC5B8;&#xC81C;&#xB098;
        &#xB85C;&#xADF8;&#xB429;&#xB2C8;&#xB2E4;.</td>
    </tr>
    <tr>
      <td style="text-align:left">log_preds</td>
      <td style="text-align:left">&#xC608;&#xCE21; &#xC0D8;&#xD50C;&#xC744; &#xB85C;&#xADF8;&#xD560;&#xC9C0;&#xC5D0;
        &#xB300;&#xD55C; &#xC5EC;&#xBD80; (&#xAE30;&#xBCF8;&#xAC12;&#xC740; True(&#xCC38;).</td>
    </tr>
    <tr>
      <td style="text-align:left">log_model</td>
      <td style="text-align:left">&#xBAA8;&#xB378;&#xC5D0; &#xB85C;&#xADF8;&#xD560; &#xC9C0;&#xC5D0; &#xB300;&#xD55C;
        &#xC5EC;&#xBD80; (&#xAE30;&#xBCF8;&#xAC12;&#xC740; True(&#xCC38;)). &#xB610;&#xD55C;
        SaveModelCallback&#xC774; &#xD544;&#xC694;&#xD569;&#xB2C8;&#xB2E4;.</td>
    </tr>
    <tr>
      <td style="text-align:left">log_dataset</td>
      <td style="text-align:left">
        <ul>
          <li>False (&#xAE30;&#xBCF8;&#xAC12;)</li>
          <li>True&#xB294; learn.dls.path&#xC5D0;&#xC11C; &#xCC38;&#xC870;&#xD558;&#xB294;
            &#xD3F4;&#xB354;&#xB97C; &#xB85C;&#xADF8;&#xD569;&#xB2C8;&#xB2E4;.</li>
          <li>&#xACBD;&#xB85C;&#xB97C; &#xC5B4;&#xB5A4; &#xD3F4;&#xB354;&#xB97C; &#xB85C;&#xADF8;&#xD560;
            &#xC9C0;&#xC5D0; &#xB300;&#xD55C; &#xCC38;&#xC870;&#xC5D0; &#xBA85;&#xC2DC;&#xC801;&#xC73C;&#xB85C;
            &#xC815;&#xC758;&#xD560; &#xC218; &#xC788;&#xC2B5;&#xB2C8;&#xB2E4;.</li>
        </ul>
        <p>&#xCC38;&#xACE0;: &#xD558;&#xC704; &#xD3F4;&#xB354; &#x201C;models&#x201D;&#xB294;
          &#xD56D;&#xC0C1; &#xBB34;&#xC2DC;&#xB429;&#xB2C8;&#xB2E4;.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">dataset_name</td>
      <td style="text-align:left">&#xB85C;&#xADF8;&#xB41C; &#xB370;&#xC774;&#xD130; &#xC138;&#xD2B8;&#xC758;
        &#xC774;&#xB984; (&#xAE30;&#xBCF8;&#xAC12;&#xC740; &#xD3F4;&#xB354; &#xC774;&#xB984;).</td>
    </tr>
    <tr>
      <td style="text-align:left">valid_dl</td>
      <td style="text-align:left">&#xC608;&#xCE21; &#xC0D8;&#xD50C;&#xC5D0; &#xC0AC;&#xC6A9;&#xB418;&#xB294;
        &#xD56D;&#xBAA9;(item)&#xC744; &#xD3EC;&#xD568;&#xD558;&#xB294; DataLoaders
        (&#xAE30;&#xBCF8;&#xAC12;&#xC740; learn.dls.valid&#xC5D0;&#xC11C;&#xC758;
        &#xC784;&#xC758;&#xC758; &#xD56D;&#xBAA9;(random items)).</td>
    </tr>
    <tr>
      <td style="text-align:left">n_preds</td>
      <td style="text-align:left">&#xB85C;&#xADF8;&#xB41C; &#xC608;&#xCE21;&#xC758; &#xC218; (&#xAE30;&#xBCF8;&#xAC12;&#xC740;
        36)</td>
    </tr>
    <tr>
      <td style="text-align:left">seed</td>
      <td style="text-align:left">&#xC784;&#xC758;&#xC758; &#xC0D8;&#xD50C;&#xC744; &#xC815;&#xC758;&#xD558;&#xB294;
        &#xB370; &#xC0AC;&#xC6A9;.</td>
    </tr>
  </tbody>
</table>

 사용자 정의 워크 플로우의 경우, 수동으로 데이터 세트 및 모델을 로그할 수 있습니다:

* `log_dataset(path, name=None, medata={})`
* `log_model(path, name=None, metadata={})` 

 __참고: 모든 하위 폴더 “models”는 무시됩니다.

##  **예시**

* [Fastai models 모델 시각화, 추적 및 비교](https://app.wandb.ai/borisd13/demo_config/reports/Visualize-track-compare-Fastai-models--Vmlldzo4MzAyNA): 완전하게 문서화된 자세한 설명\(walkthrough\)
* [CamVid에서 이미지 분할\(Image Segmentation\)](http://bit.ly/fastai-wandb): 통합 샘플 이용 사례

