---
description: ログインし、コードの状態を復元し、ローカルディレクトリをサーバーに同期し、コマンドラインインターフェイスでハイパーパラメータスイープを実行します
---

# Command Line Interface

`pip install wandb`を実行すると、新しいコマンドwandbが使用可能になります。次のサブコマンドを使用できます。

| サブコマンド | 説明 |
| :--- | :--- |
| docs | ブラウザでドキュメントを開きます |
| init | W＆Bでディレクトリを構成します |
| login | W＆Bにログイン |
| offline |  このディレクトリでW＆Bを無効にします。これは、テストに役立ちます |
| online | このディレクトリでW＆Bが有効になっていることを確認します |
| disabled | Disables all API calls, useful for testing |
| enabled | Same as `online`, resumes normal W&B logging, once you've finished testing |
| docker | Dockerイメージを実行し、cwdをマウントして、wandbがインストールされていることを確認します |
| docker-run | Docker実行コマンドにW＆B環境変数を追加します |
| projects | プロジェクトを一覧表示します |
| pull | W＆Bから実行するためにファイルをプルします |
| restore | 行のためにコードと構成状態を復元します |
| run | on以外のプログラムを起動します。Pythonの場合はwandb.init |
| runs | ストはプロジェクトで実行されます |
| sync | tfeventsまたは以前の実行ファイルを含むローカルディレクトリを同期します |
| status | 在のディレクトリステータスを一覧表示します |
| sweep | L定義を指定して新しいスイープを作成します |
| agent | ジェントを起動して、スイープでプログラムを実行します |

## コードの状態を復元します

特定の実行を実行したときのコードの状態に戻るには、`restore`を使用します。

### 例

```python
# creates a branch and restores the code to the state it was in when run $RUN_ID was executed
wandb restore $RUN_ID
```

コードの状態をどのようにキャプチャしますか？

スクリプトから`wandb.init`が呼び出されると、コードがgitリポジトリにある場合、最後のgit commitへのリンクが保存されます。コミットされていない変更やリモートと同期していない変更がある場合に備えて、diffパッチも作成されます。

