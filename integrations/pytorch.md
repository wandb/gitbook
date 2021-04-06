---
description: How to integrate a PyTorch script to log metrics to W&B
---

# PyTorch

W＆Bは、PyTorchのファーストクラスのサポートを提供します。グラデーションを自動的にログに記録し、ネットワークトポロジを保存するには、watchを呼び出してPyTorchモデルに渡します。

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

> グラデーション、メトリック、グラフは、順方向および逆方向のパスの後に`wandb.log`が呼び出されるまでログに記録されません。

 [ビデオチュートリアル](https://www.youtube.com/watch?v=G7GH0SeNBMA&ab_channel=Weights%26Biases)[\[KinoTrans1\]](applewebdata://06DFC33A-D952-4B5D-8160-04283E55F43E#_msocom_1) などの、wandbとPyTorchの統合のエンドツーエンドの例については、この[colabノートブック](https://colab.research.google.com/github/wandb/examples/blob/master/colabs/pytorch/Simple_PyTorch_Integration.ipynb)を参照してください。また、サ[ンプルプロジェクト](https://colab.research.google.com/github/wandb/examples/blob/master/colabs/pytorch/Simple_PyTorch_Integration.ipynb)セクションで他にも例を見つけることができます。

###  **オプション**

 デフォルトでは、フックはグラデーションのみをログに記録します。

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x5F15;&#x6570;</th>
      <th style="text-align:left">&#x30AA;&#x30D7;&#x30B7;&#x30E7;&#x30F3;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">log</td>
      <td style="text-align:left">
        <p><b>&#x30FB;&#x3059;&#x3079;&#x3066;&#xFF1A;&#x30B0;&#x30E9;&#x30C7;&#x30FC;&#x30B7;&#x30E7;&#x30F3;&#x3068;&#x30D1;&#x30E9;&#x30E1;&#x30FC;&#x30BF;&#x306E;&#x30D2;&#x30B9;&#x30C8;&#x30B0;&#x30E9;&#x30E0;&#x3092;&#x30ED;&#x30B0;&#x306B;&#x8A18;&#x9332;&#x3059;&#x308B;</b>
        </p>
        <p><b>&#x30FB;&#x30B0;&#x30E9;&#x30C7;&#x30FC;&#x30B7;&#x30E7;&#x30F3;&#xFF08;&#x30C7;&#x30D5;&#x30A9;&#x30EB;&#x30C8;&#xFF09;</b>
        </p>
        <p><b>&#x30FB;&#x30D1;&#x30E9;&#x30E1;&#x30FC;&#x30BF;&#xFF08;&#x30E2;&#x30C7;&#x30EB;&#x306E;&#x91CD;&#x307F;&#xFF09;</b>
        </p>
        <p><b>&#x30FB;&#x306A;&#x3057;</b>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">log_freq</td>
      <td style="text-align:left">&#x6574;&#x6570;&#xFF08;&#x30C7;&#x30D5;&#x30A9;&#x30EB;&#x30C8;&#x306F;1000&#xFF09;&#xFF1A;&#x30ED;&#x30B0;&#x30FB;&#x30B0;&#x30E9;&#x30C7;&#x30FC;&#x30B7;&#x30E7;&#x30F3;&#x9593;&#x306E;&#x30B9;&#x30C6;&#x30C3;&#x30D7;&#x6570;</td>
    </tr>
  </tbody>
</table>

## **画像**

 PyTorchテンソル型の画像データをwandb.Imageに渡し、torchvision utilsを使用して自動でログに記録します。

画像をログに記録してメディアパネルに表示するには、次のシンタクスを使用できます。

```python
wandb.log({"examples" : [wandb.Image(i) for i in images]})
```

## **複数のモデル**

同じスクリプトで複数のモデルをトラッキングする必要がある場合は、各モデルでwandb.watch（）を個別に区切ることができます。

##  **例**

統合がどのように機能するかを確認するために、いくつかの例を作成しました。

*  [GoogleColabで実行](https://colab.research.google.com/github/wandb/examples/blob/master/colabs/pytorch/Simple_PyTorch_Integration.ipynb)：開始するための簡単なノートブックの例
*  [Githubの例](https://github.com/wandb/examples/blob/master/examples/pytorch/pytorch-cnn-mnist/main.py)：PythonスクリプトのMNISTの例
*  [Wandbダッシュボード](https://app.wandb.ai/wandb/pytorch-mnist/runs/)：W＆Bで結果を表示

