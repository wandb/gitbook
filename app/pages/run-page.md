---
description: モデルの各トレーニング実行には、より大きなプロジェクト内で編成された専用ページがあります
---

# Run Page

実行ページを使用して、モデルの単一バージョンに関する詳細情報を調べます。

##  **概要\]タブ**·

* 実行名、説明、およびタグ
*  ホスト名、オペレーティングシステム、Pythonバージョン、および実行を開始したコマンド
*   [wandb.config](https://docs.wandb.com/library/config)で保存された構成パラメーターのリスト
*    ****[**wandb.log\(\)**](https://docs.wandb.com/library/log)**で保存された要約パラメーターのリスト。デフォルトでは、ログに記録された最後の値に設定されます**

  [実例を見る→](https://app.wandb.ai/carey/pytorch-cnn-fashion/runs/munu5vvg/overview?workspace=user-carey)

![](../../.gitbook/assets/run-page-overview-tab.png)

 ページ自体を公開しても、Pythonの詳細は非公開になります。左側がシークレットモードの実行ページ、右側がアカウントの例です。

![](../../.gitbook/assets/screen-shot-2020-04-07-at-7.46.39-am.png)

## **\[グラフ\]タブ**

* ビジュアライゼーションの検索、グループ化、および配置
*  編集するには、グラフの鉛筆アイコン✏️をクリックします
* x軸、メトリック、および範囲の変化
* グラフの凡例、タイトル、色の編集
*    検証セットからの予測例を表示します

[実例を見る→](https://wandb.ai/wandb/examples-keras-cnn-fashion/runs/wec25l0q?workspace=user-carey)

![](../../.gitbook/assets/image%20%2837%29.png)

##  **\[システム\]タブ**

* CPU使用率、システムメモリ、ディスクI/O、ネットワークトラフィック、GPU使用率、GPU温度、メモリへのアクセスに費やされたGPU時間、割り当てられたGPUメモリ、およびGPU電力使用量を視覚化します
*     Lambda Labsは、システムメトリックの使用について書いています。[ブログ投稿を読む→](https://lambdalabs.com/blog/weights-and-bias-gpu-cpu-utilization/)

[実例を見る→](https://wandb.ai/wandb/feb8-emotion/runs/toxllrmm/system)

![](../../.gitbook/assets/image%20%2888%29%20%282%29%20%283%29%20%283%29%20%283%29%20%283%29%20%285%29.png)

## **［モデル］タブ**

* モデルのレイヤー、パラメーターの数、および各レイヤーの出力形状を確認します

[実例を見る→](https://wandb.ai/stacey/deep-drive/runs/pr0os44x/model)

![](../../.gitbook/assets/image%20%2829%29%20%281%29%20%282%29%20%284%29%20%282%29.png)

##  **\[ログ\]タブ**

* コマンドラインに出力され、モデルをトレーニングするマシンからのstdoutとstderr
*   最後の1000行を表示します。実行が終了した後、完全なログファイルをダウンロードする場合は、右上隅にあるダウンロードボタンをクリックします。

 [実例を見る→](https://wandb.ai/stacey/deep-drive/runs/pr0os44x/logs)

![](../../.gitbook/assets/image%20%2869%29%20%284%29%20%286%29%20%287%29.png)

##  **\[ファイル\]タブ**

*     [wandb.save\(\)](https://docs.wandb.com/library/save)を使用して実行と同期するためにファイルを保存します。私たちはAI用のDropboxで
* モデルのチェックポイント、検証セットの例などを保持します
*   diff.patchを使用して、コードの正確なバージョンを復元します

 [`実例を見る→`](https://wandb.ai/stacey/deep-drive/runs/pr0os44x/files/media/images)\`\`

![](../../.gitbook/assets/image%20%283%29.png)

