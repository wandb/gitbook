# Fast.ai

 fastaiを使用してモデルをトレーニングしている場合、W＆BはWandbCallbackを使用して簡単に統合できます。[インタラクティブドキュメント](https://app.wandb.ai/borisd13/demo_config/reports/Visualize-track-compare-Fastai-models--Vmlldzo4MzAyNA)\(例付き\)で詳細を確認→

 まずWeights＆Biasesをインストールし、ログインします。

```text
pip install wandb
wandb login
```

次に、`learner` またはfit メソッドにコールバックを追加します。

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
 Fastaiのバージョン1を使用している場合は、[Fastai v1のドキュメント](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MN_4xmW6jcYndpU_n9G/v/japanese/integrations/fastai/v1)を参照してください。
{% endhint %}

`WandbCallback`は、次の引数を受け入れます。

<table>
  <thead>
    <tr>
      <th style="text-align:left">Args</th>
      <th style="text-align:left">&#x8AAC;&#x660E;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">log</td>
      <td style="text-align:left">&#x300C;gradients&#x300D;&#xFF08;&#x30C7;&#x30D5;&#x30A9;&#x30EB;&#x30C8;&#xFF09;&#x3001;&#x300C;parameters&#x300D;&#x3001;&#x300C;all&#x300D;&#x3001;&#x307E;&#x305F;&#x306F;&#x300C;None&#x300D;&#x3002;&#x640D;&#x5931;&#x3068;&#x30E1;&#x30C8;&#x30EA;&#x30C3;&#x30AF;&#x306F;&#x5E38;&#x306B;&#x30ED;&#x30B0;&#x306B;&#x8A18;&#x9332;&#x3055;&#x308C;&#x307E;&#x3059;&#x3002;</td>
    </tr>
    <tr>
      <td style="text-align:left">log_preds</td>
      <td style="text-align:left">&#x4E88;&#x6E2C;&#x30B5;&#x30F3;&#x30D7;&#x30EB;&#x3092;&#x30ED;&#x30B0;&#x306B;&#x8A18;&#x9332;&#x3059;&#x308B;&#x304B;&#x3069;&#x3046;&#x304B;&#xFF08;&#x30C7;&#x30D5;&#x30A9;&#x30EB;&#x30C8;&#x306F;True&#xFF09;&#x3002;</td>
    </tr>
    <tr>
      <td style="text-align:left">log_model</td>
      <td style="text-align:left">&#x30E2;&#x30C7;&#x30EB;&#x3092;&#x30ED;&#x30B0;&#x306B;&#x8A18;&#x9332;&#x3059;&#x308B;&#x304B;&#x3069;&#x3046;&#x304B;&#xFF08;&#x30C7;&#x30D5;&#x30A9;&#x30EB;&#x30C8;&#x306F;True&#xFF09;&#x3002;&#x3053;&#x308C;&#x306B;&#x306F;&#x3001;SaveModelCallback&#x3082;&#x5FC5;&#x8981;&#x3067;&#x3059;&#x3002;</td>
    </tr>
    <tr>
      <td style="text-align:left">log_dataset</td>
      <td style="text-align:left">
        <p>&#xB7; False&#xFF08;&#x30C7;&#x30D5;&#x30A9;&#x30EB;&#x30C8;&#xFF09;</p>
        <p>&#xB7; True&#x306F;&#x3001;learn.dls.path&#x306B;&#x3088;&#x3063;&#x3066;&#x53C2;&#x7167;&#x3055;&#x308C;&#x308B;&#x30D5;&#x30A9;&#x30EB;&#x30C0;&#x30FC;&#x3092;&#x30ED;&#x30B0;&#x306B;&#x8A18;&#x9332;&#x3057;&#x307E;&#x3059;&#x3002;.</p>
        <p>&#xB7; &#x30ED;&#x30B0;&#x306B;&#x8A18;&#x9332;&#x3059;&#x308B;&#x30D5;&#x30A9;&#x30EB;&#x30C0;&#x30FC;&#x3092;&#x53C2;&#x7167;&#x3059;&#x308B;&#x305F;&#x3081;&#x306B;&#x3001;&#x30D1;&#x30B9;&#x3092;&#x660E;&#x793A;&#x7684;&#x306B;&#x5B9A;&#x7FA9;&#x3067;&#x304D;&#x307E;&#x3059;&#x3002;</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">dataset_name</td>
      <td style="text-align:left">&#x30ED;&#x30B0;&#x306B;&#x8A18;&#x9332;&#x3055;&#x308C;&#x305F;&#x30C7;&#x30FC;&#x30BF;&#x30BB;&#x30C3;&#x30C8;&#x306E;&#x540D;&#x524D;&#xFF08;&#x30C7;&#x30D5;&#x30A9;&#x30EB;&#x30C8;&#x306F;&#x30D5;&#x30A9;&#x30EB;&#x30C0;&#x30FC;&#x540D;&#xFF09;&#x3002;</td>
    </tr>
    <tr>
      <td style="text-align:left">valid_dl</td>
      <td style="text-align:left">&#x4E88;&#x6E2C;&#x30B5;&#x30F3;&#x30D7;&#x30EB;&#x306B;&#x4F7F;&#x7528;&#x3055;&#x308C;&#x308B;&#x30A2;&#x30A4;&#x30C6;&#x30E0;&#x3092;&#x542B;&#x3080;DataLoader&#xFF08;&#x30C7;&#x30D5;&#x30A9;&#x30EB;&#x30C8;&#x306F;learn.dls.valid&#x306E;&#x30E9;&#x30F3;&#x30C0;&#x30E0;&#x30A2;&#x30A4;&#x30C6;&#x30E0;&#xFF09;&#x3002;</td>
    </tr>
    <tr>
      <td style="text-align:left">n_preds</td>
      <td style="text-align:left">&#x30ED;&#x30B0;&#x306B;&#x8A18;&#x9332;&#x3055;&#x308C;&#x305F;&#x4E88;&#x6E2C;&#x306E;&#x6570;&#xFF08;&#x30C7;&#x30D5;&#x30A9;&#x30EB;&#x30C8;&#x306F;36&#xFF09;&#x3002;</td>
    </tr>
    <tr>
      <td style="text-align:left">seed</td>
      <td style="text-align:left">&#x30E9;&#x30F3;&#x30C0;&#x30E0;&#x30B5;&#x30F3;&#x30D7;&#x30EB;&#x306E;&#x5B9A;&#x7FA9;&#x306B;&#x4F7F;&#x7528;&#x3002;</td>
    </tr>
  </tbody>
</table>

カスタムワークフローの場合、データセットとモデルをマニュアルでログに記録できます。

* `log_dataset(path, name=None, medata={})`
* `log_model(path, name=None, metadata={})` 

注：サブフォルダーの「モデル」はすべて無視されます。

##  **例**

* [ Fastaiモデルの視覚化、追跡、比較](https://app.wandb.ai/borisd13/demo_config/reports/Visualize-track-compare-Fastai-models--Vmlldzo4MzAyNA)：完全に文書化されたウォークスルー
* [ CamVidでの画像セグメンテーション](https://colab.research.google.com/drive/1IWrhwcJoncCKHm6VXsNwOr9Yukhz3B49?usp=sharing)：統合のサンプルユースケース

