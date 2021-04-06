---
description: >-
  W&B integration with the awesome NLP library Hugging Face, which has
  pre-trained models, scripts, and datasets
---

# Hugging Face

[Hugging Face Transformers](https://huggingface.co/transformers/) は、自然言語理解（NLU）と自然言語生成（NLG）の汎用アーキテクチャを提供し、100以上の言語で事前トレーニングされたモデルと、TensorFlow2.0とPyTorch間の深い相互運用性を備えています。

 トレーニングを自動的にログに記録するには、ライブラリをインストールしてログインするだけです。

```text
pip install wandb
wandb login
```

`Trainer`または`TFTrainer`は、損失、評価メトリック、モデルトポロジ、およびグラデーションを自動的にログに記録します。

 高度な構成は、[wandb環境変数](https://docs.wandb.com/library/environment-variables)を介して可能です。

 追加の変数は、トランスフォーマーで使用できます。

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x74B0;&#x5883;&#x5909;&#x6570;</th>
      <th style="text-align:left">&#x30AA;&#x30D7;&#x30B7;&#x30E7;&#x30F3;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">WANDB_WATCH</td>
      <td style="text-align:left">
        <ul>
          <li> <b>gradients</b>&#xFF08;&#x30C7;&#x30D5;&#x30A9;&#x30EB;&#x30C8;&#xFF09;&#xFF1A;&#x30B0;&#x30E9;&#x30C7;&#x30FC;&#x30B7;&#x30E7;&#x30F3;&#x306E;&#x30D2;&#x30B9;&#x30C8;&#x30B0;&#x30E9;&#x30E0;&#x3092;&#x30ED;&#x30B0;&#x306B;&#x8A18;&#x9332;&#x3057;&#x307E;&#x3059;</li>
          <li> <b>all</b>&#xFF1A;&#x30B0;&#x30E9;&#x30C7;&#x30FC;&#x30B7;&#x30E7;&#x30F3;&#x3068;&#x30D1;&#x30E9;&#x30E1;&#x30FC;&#x30BF;&#x306E;&#x30D2;&#x30B9;&#x30C8;&#x30B0;&#x30E9;&#x30E0;&#x3092;&#x30ED;&#x30B0;&#x306B;&#x8A18;&#x9332;&#x3057;&#x307E;&#x3059;</li>
          <li><b>false</b>&#xFF1A;&#x30B0;&#x30E9;&#x30C7;&#x30FC;&#x30B7;&#x30E7;&#x30F3;&#x307E;&#x305F;&#x306F;&#x30D1;&#x30E9;&#x30E1;&#x30FC;&#x30BF;&#x30FC;&#x306E;&#x30ED;&#x30B0;&#x306F;&#x3042;&#x308A;&#x307E;&#x305B;&#x3093;</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">WANDB_DISABLED</td>
      <td style="text-align:left"> <b>boolean</b>&#xFF1A;&#x30ED;&#x30B0;&#x3092;&#x5B8C;&#x5168;&#x306B;&#x7121;&#x52B9;&#x306B;&#x3059;&#x308B;&#x306B;&#x306F;<b>true</b>&#x306B;&#x8A2D;&#x5B9A;&#x3057;&#x307E;&#x3059;</td>
    </tr>
  </tbody>
</table>

###  **例**

 統合がどのように機能するかを確認するために、いくつかの例を作成しました。

*  [colabで実行](https://colab.research.google.com/drive/1NEiqNPhiouu2pPwDAVeFoN4-vTYMz9F8?usp=sharing)：開始するための簡単なノートブックの例
*  [ステップバイステップガイド](https://app.wandb.ai/jxmorris12/huggingface-demo/reports/A-Step-by-Step-Guide-to-Tracking-Hugging-Face-Model-Performance--VmlldzoxMDE2MTU)：Hugging Faceモデルのパフォーマンスを追跡します
*  [モデルのサイズは重要ですか？](https://wandb.ai/jack-morris/david-vs-goliath/reports/Does-model-size-matter%3F-A-comparison-of-BERT-and-DistilBERT--VmlldzoxMDUxNzU)BERTとDistilBERTの比較

###  **フィードバック**

 あなたからのフィードバックをお待ちしております。また、この統合を改善できたことをたいへんうれしく思っております。ご質問やご提案がございましたら[お問い合わせください](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MN_4xmW6jcYndpU_n9G/v/japanese/company/getting-help)。

## **結果を視覚化します**

W＆Bダッシュボードで結果を動的に調べます。数十の実験の調査、興味深い結果の拡大、高次元のデータの視覚化などが簡単になります。  


![](../.gitbook/assets/hf-gif-15%20%282%29%20%282%29%20%283%29%20%283%29.gif)

 これは、[BERTとDistilBERT](https://wandb.ai/jack-morris/david-vs-goliath/reports/Does-model-size-matter%3F-Comparing-BERT-and-DistilBERT-using-Sweeps--VmlldzoxMDUxNzU)を比較する例です。自動折れ線グラフの視覚化により、さまざまなアーキテクチャがトレーニング全体の評価精度にどのように影響するかを簡単に確認できます。

![](../.gitbook/assets/gif-for-comparing-bert.gif)

