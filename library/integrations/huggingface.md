---
description: >-
  W&B integration with the awesome NLP library Hugging Face, which has
  pre-trained models, scripts, and datasets
---

# Hugging Face

[Hugging Face Transformers](https://huggingface.co/transformers/) provides general-purpose architectures for Natural Language Understanding \(NLU\) and Natural Language Generation \(NLG\) with pretrained models in 100+ languages and deep interoperability between TensorFlow 2.0 and PyTorch.

To get training logged automatically, just install the library and log in:

```text
pip install git+https://github.com/huggingface/transformers.git
pip install wandb
wandb login
```

The `Trainer` will automatically log losses, evaluation metrics, model topology and gradients.

Customize logging with these optional environment variables:

<table>
  <thead>
    <tr>
      <th style="text-align:left">Environment Variables</th>
      <th style="text-align:left">Options</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">WANDB_WATCH</td>
      <td style="text-align:left">
        <ul>
          <li><b>gradients</b> (default): Log histograms of the gradients</li>
          <li><b>all</b>: Log histograms of gradients and parameters</li>
          <li><b>false</b>: No gradient or parameter logging</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">WANDB_PROJECT</td>
      <td style="text-align:left"><em><b>string</b>:</em> Set the project name. By default it&apos;s &quot;huggingface&quot;.</td>
    </tr>
    <tr>
      <td style="text-align:left">WANDB_DISABLED</td>
      <td style="text-align:left"><em><b>boolean</b>:  </em>Set to <b>true</b> to disable logging entirely</td>
    </tr>
  </tbody>
</table>

### Examples

We created a demo notebook in Google Colab. Use it to see how the integration works:

[Try a notebook example →](https://colab.research.google.com/drive/1NEiqNPhiouu2pPwDAVeFoN4-vTYMz9F8?usp=sharing)

### Feedback

We'd love to hear feedback and we're excited to improve this integration. [Contact us](../../company/getting-help.md) with any questions or suggestions. 

## Visualize Results

Explore your results dynamically in the W&B Dashboard. It's easy to look across dozens of experiments, zoom in on interesting findings, and visualize highly dimensional data.

![](../../.gitbook/assets/hf-gif-15%20%281%29.gif)

Here's an example comparing [BERT vs DistilBERT](https://app.wandb.ai/jack-morris/david-vs-goliath/reports/Does-model-size-matter%3F-Comparing-BERT-and-DistilBERT-using-Sweeps--VmlldzoxMDUxNzU) — it's easy to see how different architectures effect the evaluation accuracy throughout training with automatic line plot visualizations.

![](../../.gitbook/assets/gif-for-comparing-bert.gif)



