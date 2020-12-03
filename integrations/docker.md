# Docker

## Docker統合

W＆Bは、コードが実行されたDockerイメージへのポインターを格納できるため、以前の実験を実行された正確な環境に復元できます。`wandb`ライブラリは、この状態を維持するために**WANDB\_DOCKER**環境変数を探します。この状態を自動的に設定するヘルパーをいくつか提供します。

### ローカル開発

wandb dockerは、dockerコンテナーを起動し、wandb環境変数を渡し、コードをマウントし、wandbがインストールされていることを確認するコマンドです。デフォルトでは、コマンドはTensorFlow、PyTorch、Keras、およびJupyterがインストールされたDockerイメージを使用します。同じコマンドを使用して、独自のDockerイメージ（`wandb docker my/image:latest`）を開始できます。このコマンドは、現在のディレクトリをコンテナの「/app」ディレクトリにマウントします。これは、「-dir」フラグを使用して変更できます。

### 製造

`wandb-docker-run`コマンドは、実稼働ワークロード用に提供されています。これは、`nvidia-docker`のドロップイン代替品となることを目的としています。これは、資格情報と**WANDB\_DOCKER**環境変数を呼び出しに追加する`dockerrun`コマンドの単純なラッパーです。「--runtime」フラグを渡さず、マシンで`nvidia-docker`が使用可能な場合、これにより、ランタイムがnvidiaに設定されます。

### Kubernetes

トレーニングワークロードをKubernetesで実行し、k8s APIがポッドに公開されている場合（デフォルトの場合）. wandbは、DockerイメージのダイジェストをAPIに照会し、**WANDB\_DOCKER**環境変数を自動的に設定します。

## 復元

実行にWANDB\_DOCKER環境変数が組み込まれている場合、`wandb restore username/project:run_id`を呼び出すと、コードを復元する新しいブランチがチェックアウトされ、元のコマンドが事前入力されたトレーニングに使用される正確なDockerイメージが起動します

