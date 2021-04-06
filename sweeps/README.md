---
description: ハイパーパラメータ検索とモデルの最適化
---

# Sweeps

Weights＆Biasesスイープを使用して、ハイパーパラメータの最適化を自動化し、可能なモデルの空間を探索します。

##  W＆Bスイープを使用する利点

1. **クイックセットアップ**：わずか数行のコードで、W＆Bスイープを実行できます。
2.  **透過性**：使用しているすべてのアルゴリズムを引用します。[当社のコードはオープンソース](https://github.com/wandb/client/tree/master/wandb/sweeps)です。
3. **効果性**：スイープは完全にカスタマイズおよび構成可能です。数十台のマシンでスイープを開始でき、ラップトップでスイープを開始するのと同様に簡単です。

## 一般的な使用例

1. **探索**：ハイパーパラメータの組み合わせの空間を効率的にサンプリングして、有望な領域を発見し、モデルに関する直感を構築します。
2. **最適化**：スイープを使用して、最適なパフォーマンスを持つハイパーパラメータのセットを見つけます。
3.  **K分割交差検証**：W＆Bスイープを使用したK分割交差検証の[簡単なコード例を](https://github.com/wandb/examples/tree/master/examples/wandb-sweeps/sweeps-cross-validation)次に示します

## Approach

1. **wandbの追加：Pythonスクリプトに、ハイパーパラメーターをログに記録し、スクリプトからメトリックを出力するコードを数行追加します。**[**`今すぐ始めましょう→`**](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MN_4xmW6jcYndpU_n9G/v/japanese/sweeps/quickstart)**\`\`**
2.  **構成の記述：スイープする変数と範囲を定義します。**サーチストラテジー**を選択してください。グリッド、ランダム、ベイジアン検索、および早期停止をサポートしています。**[ここ](https://github.com/wandb/examples/tree/master/examples/keras/keras-cnn-fashion)**でいくつかの設定例を確認してください。**
3. **スイープの初期化：スイープサーバーを起動します。この中央コントローラーをホストし、スイープを実行するエージェント間で調整します。**
4. **エージェントの起動：スイープでモデルをトレーニングするために使用する各マシンでこのコマンドを実行します。エージェントは中央スイープサーバーに次に試行するハイパーパラメータを尋ね、実行を実行します。**
5. **結果の視覚化：ライブダッシュボードを開いて、すべての結果を1か所で確認できます。**

![](../.gitbook/assets/central-sweep-server-3%20%282%29%20%282%29%20%283%29%20%282%29.png)

{% page-ref page="quickstart.md" %}

{% page-ref page="existing-project.md" %}

{% page-ref page="configuration.md" %}

{% page-ref page="local-controller.md" %}

{% page-ref page="python-api.md" %}

{% page-ref page="sweeps-examples.md" %}

