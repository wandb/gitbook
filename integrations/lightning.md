---
description: Visualize PyTorch Lightning models with W&B
---

# PyTorch Lightning

 PyTorch Lightningは、PyTorchコードを整理し、[分散トレーニング](https://pytorch-lightning.readthedocs.io/en/latest/multi_gpu.html)や[16ビット](https://pytorch-lightning.readthedocs.io/en/latest/amp.html)の高度な機能を簡単に追加するための軽量ラッパーを提供します。W＆Bは、ML実験をロ[精度](https://pytorch-lightning.readthedocs.io/en/latest/loggers.html#weights-and-biases)などグに記録するための軽量ラッパーを提供します。PyTorch Lightningライブラリに直接組み込まれているため、いつでもそのドキュメントを確認できます。

### **⚡たった2行で超高速で進みましょう**

```python
from pytorch_lightning.loggers import WandbLogger
from pytorch_lightning import Trainer

wandb_logger = WandbLogger()
trainer = Trainer(logger=wandb_logger)
```

##  ✅実際の例をチェックしてください！

 統合がどのように機能するかを確認するために、いくつかの例を作成しました。

*  [GoogleColabで実行し](https://colab.research.google.com/github/wandb/examples/blob/master/colabs/pytorch-lightning/Supercharge_your_Training_with_Pytorch_Lightning_%2B_Weights_%26_Biases.ipynb)、シンプルなノートブックで統合を試してください
* モデルのパフォーマンスを追跡するための[ステップバイステップガイド](https://app.wandb.ai/cayush/pytorchlightning/reports/Use-Pytorch-Lightning-with-Weights-%26-Biases--Vmlldzo2NjQ1Mw)
*  [Lightningによるセマンティックセグメンテーション](https://wandb.ai/borisd13/lightning-kitti/reports/Lightning-Kitti--Vmlldzo3MTcyMw)：自動運転車のニューラルネットワークを最適化します

## **💻 API参照**

### `WandbLogger`

パラメータ：

* **name** \(_str_\) – 実行の表示名。
* **save\_dir** \(_str_\) – データが保存されるパス。
* **offline** \(_bool_\) – オフラインで実行します（データは後
* **version** \(_id_\) – でwandbサーバーにストリーミングできます）。
* **anonymous** \(_bool_\) – 匿名ログを有効または明示的に無効にします。
* **project** \(_str_\) – この実行が属するプロジェクトの名前。
* **tags** \(_list of str_\) – この実行に関連付けられたタグ。

### **`WandbLogger.watch`**

 対数モデルトポロジ、およびオプションでグラデーションと重み。

```python
wandb_logger.watch(model, log='gradients', log_freq=100)
```

 パラメータ：

* **model** \(_nn.Module_\) – ログに記録されるモデル。
* **log** \(_str_\) – 「gradients」（デフォルト）、「parameters」、「all」、またはNoneのいずれかです。
* **log\_freq** \(_int_\) – グラデーションとパラメータのロギング間のステップ数。

### **`WandbLogger.log_hyperparams`**

ハイパーパラメータ構成を記録します。

注：この関数は`Trainer`によって自動的に呼び出されます

```python
wandb_logger.log_hyperparams(params)
```

 パラメータ：

* **params** \(dict\)  – ハイパーパラメータ名をキーとして、構成値を値として持つ辞書

### `WandbLogger.log_metrics`

トレーニング指標を記録します。

 注：この関数は`Trainer`によって自動的に呼び出されます

```python
wandb_logger.log_metrics(metrics, step=None)
```

パラメータ：

* **metric** \(numeric\) – メトリック名をキーとして、測定量を値として持つ辞書
* **step** \(int\|None\) – メトリックを記録するステップ番号

\*\*\*\*

