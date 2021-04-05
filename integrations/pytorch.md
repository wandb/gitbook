---
description: How to integrate a PyTorch script to log metrics to W&B
---

# PyTorch

トップの機械学習ライブラリとワークフローとの当社のすべての統合へのギャラリー。

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

> グラデーション、メトリック、およびグラフは、順方向および逆方向のパスの後に`wandb.log`が呼び出されるまでログに記録されません。

 ビデオチュートリアルを含む、wandbとPyTorchの統合のエンドツーエンドの例については、この[colabノートブック](https://colab.research.google.com/github/wandb/examples/blob/master/colabs/pytorch/Simple_PyTorch_Integration.ipynb)を参照してください。また、サ[ンプルプロジェクト](https://colab.research.google.com/github/wandb/examples/blob/master/colabs/pytorch/Simple_PyTorch_Integration.ipynb)セクションで他の例を見つけることができます。

###  **オプション**

 デフォルトでは、フックはグラデーションのみをログに記録します。

| 引数 | オプション |
| :--- | :--- |
| log |     **すべて：グラデーションとパラメータのヒストグラムをログに記録する·       グラデーション（デフォルト）·       パラメータ（モデルの重み）なし** |
| log\_freq | 整数（デフォルトは100）：ロググラデーション間のステップ数 |

## **画像**

 画像データを含むPyTorchテンソルを`wandb.Image`に渡すことができ、torchvision utilsを使用してそれらを自動的にログに記録します。画像をログに記録してメディアパネルに表示するには、次の構文を使用できます。

```python
wandb.log({"examples" : [wandb.Image(i) for i in images]})
```

## **複数のモデル**

同じスクリプトで複数のモデルを追跡する必要がある場合は、各モデルでwandb.watch（）を個別に壁にすることができます。

## Example

統合がどのように機能するかを確認するために、いくつかの例を作成しました。

*  [GoogleColabで実行](https://colab.research.google.com/github/wandb/examples/blob/master/colabs/pytorch/Simple_PyTorch_Integration.ipynb)：開始するための簡単なノートブックの例
*  [Githubの例](https://github.com/wandb/examples/blob/master/examples/pytorch/pytorch-cnn-mnist/main.py)：PythonスクリプトのMNISTの例
*  [Wandbダッシュボード](https://app.wandb.ai/wandb/pytorch-mnist/runs/)：W＆Bで結果を表示

