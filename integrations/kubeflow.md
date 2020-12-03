# Kubeflow

##  Kubeflow統合

 特定の機能を使用するには、追加の依存関係が必要です。`pip install wandb [kubeflow]`を実行して、すべてのKubeflow依存関係をインストールします。

### トレーニングジョブ

現在、W＆BはTF\_CONFIG環境変数を自動的に読み取り、分散実行をグループ化します。

### Arena

wandbライブラリは、コンテナ環境に資格情報を自動的に追加することにより、[アリーナ](https://github.com/kubeflow/arena)と統合されます。wandbラッパーをローカルで使用する場合は、以下を`.bashrc`に追加します。

```text
alias arena="python -m wandb.kubeflow.arena"
```

アリーナがローカルにインストールされていない場合、上記のコマンドは`wandb/arena` dockerイメージを使用して、kubectl構成をマウントしようとします。

###  パイプライン

wandbは、[パイプライン](https://github.com/kubeflow/pipelines)で使用できる`arena_launcher_op`を提供します。独自のカスタムランチャー操作を作成する場合は、この[コード](https://github.com/wandb/client/blob/master/wandb/kubeflow/__init__.py)を使用してpipeline\_metadataを追加することもできます。wandbが認証するには、**WANDB\_API\_KEY**を操作に追加する必要があります。そうすると、ランチャーは同じ環境変数をトレーニングコンテナーに追加できます。

```python
import os
from kubernetes import client as k8s_client

op = dsl.ContainerOp( ... )
op.add_env_variable(k8s_client.V1EnvVar(
        name='WANDB_API_KEY',
        value=os.environ["WANDB_API_KEY"]))
```

