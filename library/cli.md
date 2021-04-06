---
description: ログイン後コードの状態を復元し、ローカルディレクトリをサーバーに同期します。その後コマンドラインインターフェイスでハイパーパラメータスイープを実行します。
---

# Command Line Interface

`pip install wandb`を実行すると、新しいコマンドwandbが使用可能になります。次のサブコマンドを使用できます。

| サブコマンド | 説明 |
| :--- | :--- |
| docs | ブラウザでドキュメントを開きます |
| init | W＆Bでディレクトリのコンフィギュレーションを行います |
| login | W＆Bにログインします |
| offline | 実行データのみをローカルに保存し、クラウド同期は行いません（off`は非推奨`） |
| online | このディレクトリでW＆Bが有効になっていることを確認します（onは非推奨） |
| disabled | すべてのAPI呼び出しを無効にします。テストに役立ちます |
| enabled | `online`と同様、テスト終了後、通常のW＆Bロギングを再開します |
| docker | Dockerイメージを実行し、cwdをマウントして、wandbがインストールされていることを確認します |
| docker-run | Docker実行コマンドにW＆B環境変数を追加します |
| projects | プロジェクトを一覧表示します |
| pull | W＆Bから実行するファイルをプルします |
| restore | 実行に、コードとコンフィグの状態を復元します |
| run | Python以外のプログラムを起動します。Pythonの場合はwandb.init（）を使用します |
| runs | ストはプロジェクトで実行されます |
| sync | tfeventsまたは以前の実行ファイルを含むローカルディレクトリを同期します |
| status | 現在のディレクトリステータスを一覧表示します |
| sweep | YAML定義指定した新しいスイープを作成します |
| agent | エージェントを開始してスイープでプログラムを実行します |

## コードの状態を復元します

特定のrun実行後、自身のコードの状態に戻るには`restore`を使用します。

### 例

```python
# creates a branch and restores the code to the state it was in when run $RUN_ID was executed
wandb restore $RUN_ID
```

**コードの状態をどのようにキャプチャしますか？**

スクリプトから`wandb.init`が呼び出されたときに、コードがgitリポジトリにあれば、最後のgit commitへのリンクが保存されます。コミットされていない変更やリモートと同期していない変更がある場合に備えて、diffパッチも作成されます。

