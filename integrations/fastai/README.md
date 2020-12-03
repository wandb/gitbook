# Fast.ai

 fastaiを使用してモデルをトレーニングしている場合、W＆BはWandbCallback.を使用して簡単に統合できます。[例を使用してインタラクティブドキュメント](https://app.wandb.ai/borisd13/demo_config/reports/Visualize-track-compare-Fastai-models--Vmlldzo4MzAyNA)で詳細を確認→

 最初はWeights＆Biasesをインストールし、ログインします。

```text
pip install wandb
wandb login
```

Then add the callback to the `learner` or `fit` method:

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

| Args | 説明 |
| :--- | :--- |
| log | 「gradients」（デフォルト）、「parameters」、「all」、または「None」。損失とメトリックは常にログに記録されます。 |
| log\_preds | 予測サンプルをログに記録するかどうか（デフォルトはTrue）。 |
| log\_model | モデルをログに記録するかどうか（デフォルトはTrue）。これには、SaveModelCallbackも必要です。 |
| log\_dataset | False（デフォルト） |
| dataset\_name | Trueは、learn.dls.pathによって参照されるフォルダーをログに記録します。 |
| valid\_dl |  ログに記録するフォルダーを参照するために、パスを明示的に定義できます。 |
| n\_preds | 注：サブフォルダー「モデル」は常に無視されます。 |
| seed | ログに記録されたデータセットの名前（デフォルトはフォルダー名）。 |

カスタムワークフローの場合、データセットとモデルを手動でログに記録できます。

* `log_dataset(path, name=None, medata={})`
* `log_model(path, name=None, metadata={})` 

注：サブフォルダーの「モデル」はすべて無視されます。

##  **例**

* [ Fastaiモデルの視覚化、追跡、比較](https://app.wandb.ai/borisd13/demo_config/reports/Visualize-track-compare-Fastai-models--Vmlldzo4MzAyNA)：完全に文書化されたウォークスルー
* [ CamVidでの画像セグメンテーション](https://colab.research.google.com/drive/1IWrhwcJoncCKHm6VXsNwOr9Yukhz3B49?usp=sharing)：統合のサンプルユースケース

