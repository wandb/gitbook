# Hugging Face

 抱抱脸的变压器（[HuggingFace Transformer](https://huggingface.co/transformers/)）为自然语言理解（NLU）和自然语言生成（NLG）提供通用架构，该架构拥有100多种语言的预训练模型，并能够在TensorFlow 2.0与PyTorch之间进行深度操作。

为了自动记录训练，就安转库并登录：

```text
pip install wandb
wandb login
```

 `Trainer`或`TFTrainer`会自动记录损失、评估指标、模型拓扑和梯度。

可利用[权阈环境变量](https://docs.wandb.com/library/environment-variables)做高级配置。

变压器（Transformer）还有其它变量：

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
        <ul>
          <li><b>gradients</b>&#xFF08;&#x9ED8;&#x8BA4;&#x503C;&#xFF09;&#xFF1A;&#x8BB0;&#x5F55;&#x68AF;&#x5EA6;&#x76F4;&#x65B9;&#x56FE;&#x3002;</li>
          <li><b>all</b>&#xFF1A;&#x8BB0;&#x5F55;&#x68AF;&#x5EA6;&#x548C;&#x53C2;&#x6570;&#x7684;&#x76F4;&#x65B9;&#x56FE;&#x3002;</li>
          <li><b>false</b>&#xFF1A;&#x4E0D;&#x8BB0;&#x5F55;&#x68AF;&#x5EA6;&#x548C;&#x53C2;&#x6570;&#x3002;</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">WANDB_DISABLED</td>
      <td style="text-align:left"><em><b>boolean</b>:</em> &#x8BBE;&#x4E3A;<b>True</b>&#x5C31;&#x4F1A;&#x505C;&#x6B62;&#x5168;&#x90E8;&#x8BB0;&#x5F55;&#x3002;</td>
    </tr>
  </tbody>
</table>

###  **范例**

我们准备了几个例子，让大家看看集成的效果：

*  [在Colab运行](https://colab.research.google.com/drive/1NEiqNPhiouu2pPwDAVeFoN4-vTYMz9F8?usp=sharing)：从一个简单的笔记本入手。
*  [一步步教你](https://wandb.ai/jxmorris12/huggingface-demo/reports/A-Step-by-Step-Guide-to-Tracking-Hugging-Face-Model-Performance--VmlldzoxMDE2MTU)：跟踪HuggingFace模型的表现  
*  [模型大小有关系吗？](https://wandb.ai/jack-morris/david-vs-goliath/reports/Does-model-size-matter%3F-A-comparison-of-BERT-and-DistilBERT--VmlldzoxMDUxNzU)BERT与DistilBERT比较

###  **反馈**

 我们希望收到大家的反馈，我们很想改善集成。无论你有任何问题或建议，欢迎[联系我们](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MLvV8HPbd9J_6haqztg/v/ch/company/getting-help)。

##  **结果可视化**

 你可在权阈的指示板（W&B Dashboard）查看动态结果。纵观多个实验，仔细观察有意思的发现，可视化高维数据，这些都轻而易举。

![](../.gitbook/assets/hf-gif-15%20%282%29%20%282%29.gif)

 这个例子是比较[BERT与DistilBERT](https://app.wandb.ai/jack-morris/david-vs-goliath/reports/Does-model-size-matter%3F-Comparing-BERT-and-DistilBERT-using-Sweeps--VmlldzoxMDUxNzU)——利用自动绘制线条可视化功能，很容易看清不同的架构如何影响评估准确率。

![](../.gitbook/assets/gif-for-comparing-bert.gif)

