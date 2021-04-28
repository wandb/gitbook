---
description: >-
  A Weights & Biases integration for Hugging Face's Transformers library for
  experiment tracking and model and data versioning
---

# Hugging Face

 ****[抱抱脸的Transformers（HuggingFace Transformers）](https://huggingface.co/transformers/)项目提供了用于自然语言理解（NLU）和自然语言生成（NLG）的通用架构，拥有100多种语言的预训练模型，并在TensorFlow 2.0与PyTorch之间能够进行深度互操作。[抱抱脸的Transformers（HuggingFace Transformers）](https://huggingface.co/transformers/)项目提供了用于自然语言理解（NLU）和自然语言生成（NLG）的通用架构，拥有100多种语言的预训练模型，并在TensorFlow 2.0与PyTorch之间能够进行深度互操作。

要想自动记录训练，只需安装库并登录：

pip install wandb

wandb login

`Trainer`或`TFTrainer`会自动记录损失、评估指标、模型拓扑和梯度。

通过[wandb环境变量](https://docs.wandb.com/library/environment-variables)可以进行高级配置。

还有一些变量可与Transformers一起使用：

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x73AF;&#x5883;&#x53D8;&#x91CF;</th>
      <th style="text-align:left">&#x9009;&#x9879;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">WANDB_WATCH</td>
      <td style="text-align:left">
        <p>l <b>gradients</b>&#xFF08;&#x9ED8;&#x8BA4;&#x503C;&#xFF09;&#xFF1A;&#x8BB0;&#x5F55;&#x68AF;&#x5EA6;&#x76F4;&#x65B9;&#x56FE;&#x3002;</p>
        <p>l <b>all</b>&#xFF1A;&#x8BB0;&#x5F55;&#x68AF;&#x5EA6;&#x548C;&#x53C2;&#x6570;&#x7684;&#x76F4;&#x65B9;&#x56FE;&#x3002;</p>
        <p>l <b>false</b>&#xFF1A;&#x4E0D;&#x8BB0;&#x5F55;&#x68AF;&#x5EA6;&#x548C;&#x53C2;&#x6570;&#x3002;</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">WANDB_DISABLED</td>
      <td style="text-align:left"><em>boolean:</em> &#x8BBE;&#x4E3A;<b>True&#x4EE5;&#x5B8C;&#x5168;&#x7981;&#x7528;&#x8BB0;&#x5F55;</b>
      </td>
    </tr>
  </tbody>
</table>

**示例**

我们已经为你创建了一些示例，以了解集成的工作原理：

  [在Colab中运行](https://colab.research.google.com/drive/1NEiqNPhiouu2pPwDAVeFoN4-vTYMz9F8?usp=sharing)：一个简单的笔记本示例，让你入门。

 [一步步教你](https://wandb.ai/jxmorris12/huggingface-demo/reports/A-Step-by-Step-Guide-to-Tracking-Hugging-Face-Model-Performance--VmlldzoxMDE2MTU)：跟踪你的抱抱脸（HuggingFace）模型的性能

  [模型大小重要吗？](https://wandb.ai/jack-morris/david-vs-goliath/reports/Does-model-size-matter?-A-comparison-of-BERT-and-DistilBERT--VmlldzoxMDUxNzU)BERT与DistilBERT的比较  

**反馈**

我们很乐意收到大家的反馈意见，我们很高兴能改善这个集成。如果有任何问题或建议，请[联系我们](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MLvV8HPbd9J_6haqztg/v/ch/company/getting-help)。

**可视化结果**

你可在W&B仪表盘中动态探究你的结果。 你可以轻松查看数十项实验，放大感兴趣的发现，并可视化高维数据。

下面是一个比较[BERT与DistilBERT](https://app.wandb.ai/jack-morris/david-vs-goliath/reports/Does-model-size-matter?-Comparing-BERT-and-DistilBERT-using-Sweeps--VmlldzoxMDUxNzU)的例子——通过自动线图可视化，很容易看清不同架构对整个训练过程中评估精确率的影响。

​

