# MLflow \(beta\)

> MLFlow統合は現在ベータ版であり、公式のwandbpythonパッケージの一部ではありません。この統合を試すには、次のコマンドを実行してgitブランチからwandbをインストールします。

```bash
pip install --upgrade git+git://github.com/wandb/client.git@feature/mlflow#egg=wandb
```

## MLflow統合

すでに[MLflow](https://www.mlflow.org/docs/latest/tracking.html)を使用して実験を追跡している場合は、W＆Bで簡単に視覚化できます。 mlflowスクリプトで`importwandb`を呼び出すだけで、すべてのメトリック、パラメータ、およびアーティファクトがW＆Bにミラーリングされます。これを行うには、[mlflowpython](https://github.com/mlflow/mlflow)ライブラリにパッチを適用します。現在の統合は書き込み専用です。すべてのデータは、[mlflow](https://www.mlflow.org/docs/latest/tracking.html)用に構成したバックエンドにも書き込まれます。

## コンセプトマッピング

データをwandbとmlflowの両方の追跡バックエンドにミラーリングする場合、次の概念が相互にマッピングされます。

| MLflow | W&B |
| :--- | :--- |
| [Experiment](https://www.mlflow.org/docs/latest/tracking.html#organizing-runs-in-experiments) | [Project](../app/pages/project-page.md) |
| [mlflow.start\_run](https://www.mlflow.org/docs/latest/python_api/mlflow.html#mlflow.start_run) | [wandb.init](../library/init.md) |
| [mlflow.log\_params](https://www.mlflow.org/docs/latest/python_api/mlflow.html#mlflow.log_param) | [wandb.config](../library/config.md) |
| [mlflow.log\_metrics](https://www.mlflow.org/docs/latest/python_api/mlflow.html#mlflow.log_metric) | [wandb.log](../library/log.md) |
| [mlflow.log\_artifacts](https://www.mlflow.org/docs/latest/python_api/mlflow.html#mlflow.log_artifact) | [wandb.save](../library/save.md) |
| [mlflow.start\_run\(nested=True\)](https://mlflow.org/docs/latest/python_api/mlflow.html#mlflow.start_run) | [Grouping](../library/grouping.md) |

## 豊富なメトリックのロギング

画像、ビデオ、プロットなどのリッチメディアをログに記録する場合は、コードで[wandb.log](../library/log.md) を呼び出すこともできます。ログへの呼び出しにstep引数を渡して、mlflowでログに記録しているメトリックと一致させるようにしてください。

## 高度な構成

デフォルトでは、wandbはメトリック、パラメータ、およびアーティファクトのみをログに記録します。wandbでアーティファクトを保存したくない場合は、`WANDB_SYNC_MLFLOW=metrics,params`を設定できます。wandbへのすべてのデータのミラーリングを無効にする場合は、`WANDB_SYNC_MLFLOW=false`を設定できます。

