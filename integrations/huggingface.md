---
description: >-
  W&B integration with the awesome NLP library Hugging Face, which has
  pre-trained models, scripts, and datasets
---

# Hugging Face

 [Hugging Face Transformers](https://huggingface.co/transformers/)는 100개 이상의 언어 사전 훈련된 모델과 TensorFlow 2.0과 PyTorch간의 깊은 상호운용성\(deep interoperability\)을 통하여 Natural Language Understanding\(자연어 이해\) \(NLU\)와 Natural Language Generation\(자연어 생성\) \(NLG\)에 대한 다목적 아키텍처를 제공합니다.  


훈련을 자동으로 로그하시려면, 라이브러리를 설치하고 로그인하시기만 하면 됩니다:

```text
pip install wandb
wandb login
```

 `Trainer` 또는 `TFTrainer`는 손실, 평가 매트릭, 모델 토폴로지\(topology\) 및 경사\(gradients\)를 자동으로 로그합니다.

 [wandb 환경변수\(environment variables\)](https://docs.wandb.ai/v/ko/library/environment-variables)를 통해 고급 구성을 하실 수 있습니다.

트랜스포머\(transformer\)를 활용해 추가 변수를 사용하실 수 있습니다:

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#xD658;&#xACBD;&#xBCC0;&#xC218;</th>
      <th style="text-align:left">&#xC635;&#xC158;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">WANDB_WATCH</td>
      <td style="text-align:left">
        <ul>
          <li><b>gradients</b>  <b> (&#xAE30;&#xBCF8;&#xAC12;): &#xACBD;&#xC0AC;(gradients)&#xC758; &#xD788;&#xC2A4;&#xD1A0;&#xADF8;&#xB7A8;&#xC744; &#xB85C;&#xADF8;&#xD569;&#xB2C8;&#xB2E4;</b>
          </li>
          <li><b>all</b>: &#xACBD;&#xC0AC;(gradients) &#xBC0F; &#xB9E4;&#xAC1C;&#xBCC0;&#xC218;(parameter)&#xC758;
            &#xD788;&#xC2A4;&#xD1A0;&#xADF8;&#xB7A8;&#xC744; &#xB85C;&#xADF8;&#xD569;&#xB2C8;&#xB2E4;</li>
          <li><b>false</b>: &#xACBD;&#xC0AC;(gradients) &#xBC0F; &#xB9E4;&#xAC1C;&#xBCC0;&#xC218;(parameter)
            &#xB85C;&#xAE45;&#xC774; &#xC5C6;&#xC2B5;&#xB2C8;&#xB2E4;</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">WANDB_DISABLED</td>
      <td style="text-align:left"><em><b>boolean</b>:</em>  <b> </b>true&#xB85C; &#xC124;&#xC815;&#xD558;&#xC5EC;
        &#xB85C;&#xAE45;&#xC744; &#xC644;&#xC804;&#xD788; &#xBE44;&#xD65C;&#xC131;&#xD654;&#xD569;&#xB2C8;&#xB2E4;
        <br
        />
      </td>
    </tr>
  </tbody>
</table>

###  **예시**

저희는 다음의 통합\(integration\)이 어떻게 작동하는지 확인할 수 있는 몇 가지 예시를 만들었습니다:

* [colab에서 실행](https://colab.research.google.com/drive/1NEiqNPhiouu2pPwDAVeFoN4-vTYMz9F8?usp=sharing): 시작을 위한 간단한 notebook 예시
* [단계별 가이드: ](https://app.wandb.ai/jxmorris12/huggingface-demo/reports/A-Step-by-Step-Guide-to-Tracking-Hugging-Face-Model-Performance--VmlldzoxMDE2MTU)Hugging Face 모델 퍼포먼스를 추적합니다
* [모델 사이즈가 중요한가요?](https://app.wandb.ai/jack-morris/david-vs-goliath/reports/Does-model-size-matter%3F-A-comparison-of-BERT-and-DistilBERT--VmlldzoxMDUxNzU) BERT와 DistilBERT간 비교

###  **피드백**

 저희는 여러분들의 피드백을 얻고 싶으며, 이러한 통합\(integration\)을 개선할 수 있게 되어 매우 기쁩니다. 문의 및 제안 하실 점이 있으시면 언제든 저희에게 [문의](https://docs.wandb.ai/v/ko/company/getting-help)해 주시기 바랍니다  


### **결과 시각화**

**W&B 대시보드에서 결과를 동적으로 탐색합니다. 수십 개의 실험을 쉽게 살펴보고, 흥미로운 결과를 확대해 볼 수 있으며, 고차원 데이터를 시각화하실 수 있습니다.**

![](../.gitbook/assets/hf-gif-15%20%282%29%20%282%29%20%283%29%20%283%29%20%283%29%20%283%29.gif)

 다음은 [BERT 대 DistilBERT](https://app.wandb.ai/jack-morris/david-vs-goliath/reports/Does-model-size-matter%3F-Comparing-BERT-and-DistilBERT-using-Sweeps--VmlldzoxMDUxNzU) 비교 예시입니다. 자동 라인 플롯 시각화를 활용한 훈련을 통해서 평과 정확도에 서로 다른 아키텍처가 어떤 영향을 끼치는지 쉽게 확인하실 수 있습니다.

![](../.gitbook/assets/gif-for-comparing-bert.gif)

