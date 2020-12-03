---
description: プロジェクトですでにwandb.init、wandb.config、およびwandb.logを使用している場合は、ここから始めてください。
---

# Sweep from an existing project

既存のW＆Bプロジェクトがある場合は、ハイパーパラメータスイープを使用してモデルの最適化を簡単に開始できます。実用的な例を使用して手順を説明します。[W＆Bダッシュボード](https://wandb.ai/carey/pytorch-cnn-fashion)を開くことができます。このサンプルリポジトリのコードを使用しています。このコードは、PyTorch畳み込みニューラルネットワークをトレーニングして、[Fashion MNIST ](https://github.com/zalandoresearch/fashion-mnist)データセットからの画像を分類します。

## 1. **プロジェクトの作成**

最初のベースライン実行を手動で実行して、W＆Bロギングが正しく機能していることを確認します。この単純なサンプルモデルをダウンロードし、数分間トレーニングすると、サンプルがWebダッシュボードに表示されます。

* このリポジトリの git clone [https://github.com/wandb/examples.gitのクローンを作成します](https://github.com/wandb/examples.gitのクローンを作成します)
* **•この例の\`cd examples/pytorch/pytorch-cnn-fashion\`を開きます**
* **手動で \`python train.py実行を実行します**

[プロジェクトページの例を見る→](https://app.wandb.ai/carey/pytorch-cnn-fashion)

## 2. Create a sweep

From your project page, open the Sweep tab in the sidebar and click "Create Sweep".

![](../.gitbook/assets/sweep1.png)

The auto-generated config guesses values to sweep over based on the runs you've done already. Edit the config to specify what ranges of hyperparameters you want to try. When you launch the sweep, it starts a new process on our hosted W&B sweep server. This centralized service coordinates the agents— your machines that are running the training jobs.

![](../.gitbook/assets/sweep2.png)

## 3. Launch agents

Next, launch an agent locally. You can launch dozens of agents on different machines in parallel if you want to distribute the work and finish the sweep more quickly. The agent will print out the set of parameters it’s trying next.

![](../.gitbook/assets/sweep3.png)

That’s it! Now you're running a sweep. Here’s what the dashboard looks like as my example sweep gets started. [View an example project page →](https://app.wandb.ai/carey/pytorch-cnn-fashion)

![](https://paper-attachments.dropbox.com/s_5D8914551A6C0AABCD5718091305DD3B64FFBA192205DD7B3C90EC93F4002090_1579066494222_image.png)

## Seed a new sweep with existing runs

Launch a new sweep using existing runs that you've previously logged.

1. Open your project table.
2. Select the runs you want to use with checkboxes on the left side of the table.
3. Click the dropdown to create a new sweep.

Your sweep will now be set up on our server. All you need to do is launch one or more agent to start running runs.

![](../.gitbook/assets/create-sweep-from-table%20%281%29.png)

