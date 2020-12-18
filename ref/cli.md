# Command Line Reference

## wandb

 **使用法**

`wandb [OPTIONS] COMMAND [ARGS]...`

**オプション**

| **オプション** | **説明** |
| :--- | :--- |
| --version | バージョンを表示して終了します。 |
| --help | このメッセージを表示して終了します。 |

**コマンド**

| **コマンド** | **説明** |
| :--- | :--- |
| agent | W＆Bエージェントを試行 |
| artifact | アーティファクトと対話するためのコマンド |
| controller | W＆Bローカルスイープコントローラーを試行 |
| disabled | W＆Bを無効にします。 |
| docker | dockerを使用すると、dockerイメージでコードを試行して... |
| docker-run | W＆B環境を設定する`docker run`のシンプルなラッパー... |
| enabled | W＆Bを有効にします。 |
| init | 重みとバイアスを使用してディレクトリを構成します |
| local | ローカルW＆Bコンテナを起動します（実験的） |
| login | ウェイト＆バイアスにログイン |
| offline | W＆B同期を無効にする |
| online | W＆B同期を有効にする |
| pull | ウェイトとバイアスからファイルをプルする |
| restore | 試行のためにコード、構成、およびDockerの状態を復元 |
| status | 構成設定を表示 |
| sweep | スイープを作成 |
| sync | オフライントレーニングディレクトリをW＆Bにアップロードします |

## wandb agent

**使用法**

`wandb agent [OPTIONS] SWEEP_ID`

 **概要**

wandbエージェント

 **オプション**

| **オプション** | **説明** |
| :--- | :--- |
| -p, --project | スイープのプロジェクト。 |
| -e, --entity | プロジェクトのエンティティスコープ。 |
| --count | このエージェントの最大試行数。 |
| --help | このメッセージを表示して終了します。 |

## wandb artifact

**使用法**

`wandb artifact [OPTIONS] COMMAND [ARGS]...`

**概要**

アーティファクトと対話するためのコマンド

**オプション**

| **オプション** | **説明** |
| :--- | :--- |
| --help | このメッセージを表示して終了します。 |

### wandb artifact get

 **使用法**

`wandb artifact get [OPTIONS] PATH`

 **概要**

wandbからアーティファクトをダウンロードする

 **オプション**

| **オプション** | **説明** |
| :--- | :--- |
| --root | アーティファクトをダウンロードするディレクトリ |
| --type | ダウンロードしているアーティファクトの種類 |
| --help | このメッセージを表示して終了します。 |

### wandb artifact ls

 **使用法**

`wandb artifact ls [OPTIONS] PATH`

**概要**

wandbプロジェクトのすべてのアーティファクトを一覧表示します

 **オプション**

| **オプション** | **説明** |
| :--- | :--- |
| -t, --type | リストするアーティファクトのタイプ |
| --help | このメッセージを表示して終了します。 |

### wandb artifact put

 **使用法**

`wandb artifact put [OPTIONS] PATH`

 **概要**

アーティファクトをwandbにアップロードします

**オプション**

| **オプション** | **説明** |
| :--- | :--- |
| -n, --name | プッシュするアーティファクトの名前： |
| -d, --description | このアーティファクトの説明 |
| -t, --type | アーティファクトのタイプ |
| -a, --alias | このアーティファクトに適用するエイリアス |
| --help | このメッセージを表示して終了します。 |

## wandb controller

 **使用法**

`wandb controller [OPTIONS] SWEEP_ID`

 **概要**

W＆Bローカルスイープコントローラーを試行する

 **オプション**

| **オプション** | **説明** |
| :--- | :--- |
| --verbose | 詳細な出力を表示する |
| --help | このメッセージを表示して終了します。 |

## wandb disabled

 **使用法**

`wandb disabled [OPTIONS]`

 **概要**

W＆Bを無効にします。

 **オプション**

| **オプション** | **説明** |
| :--- | :--- |
| --help | このメッセージを表示して終了します。 |

## wandb docker

**使用法**

`wandb docker [OPTIONS] [DOCKER_RUN_ARGS]... [DOCKER_IMAGE]`

 **概要**

W＆B `docker`を使用すると、コードをdockerイメージで試行して、wandbが構成されていることを確認できます。WANDB\_DOCKERおよびWANDB\_API\_KEY環境変数をコンテナに追加し、デフォルトで現在のディレクトリを/appにマウントします。イメージ名が宣言される前にdockerrunに追加される追加の引数を渡すことができます。渡されない場合は、デフォルトのイメージが選択されます。

wandb docker -v /mnt/dataset:/app/data wandb docker gcr.io/kubeflow- images-public/tensorflow-1.12.0-notebook-cpu:v0.4.0 --jupyter wandb docker wandb/deepo:keras-gpu --no-tty --cmd "python train.py --epochs=5"

デフォルトでは、エントリポイントをオーバーライドしてwandbの存在を確認し、存在しない場合はインストールします。--jupyterフラグを渡すと、jupyterがインストールされていることを確認し、ポート8888でjupyter labを開始します。システムでnvidia-dockerが検出された場合は、nvidiaランタイムを使用します。 wandbで環境変数を既存のdockerrunコマンドに設定するだけの場合は、wandbdocker-runコマンドを参照してください。

**オプション**

| **オプション** | **説明** |
| :--- | :--- |
| --nvidia | / --no-nvidia nvidiaランタイムを使用します。デフォルトでは、nvidiaになります。 |
| nvidia-docker | 存在します |
| --digest | 画像ダイジェストを出力して終了します |
| --jupyter | /-no-jupyterコンテナでjupyterlabを試行します |
| --dir | コンテナにコードをマウントするディレクトリ |
| --no-dir | 現在のディレクトリをマウントしない |
| --shell | コンテナを開始するためのシェル |
| --port | jupyterをバインドするホストポート |
| --cmd | コンテナで試行するコマンド |
| --no-tty | ttyなしでコマンドを試行 |
| --help | このメッセージを表示して終了します。 |

## wandb enabled

 **使用法**

`wandb enabled [OPTIONS]`

**概要**

W＆Bを有効にします。

 **オプション**

| **オプション** | **説明** |
| :--- | :--- |
| --**オプション** | このメッセージを表示して終了します。 |

## wandb init

 **使用法**

`wandb init [OPTIONS]`

 **概要**

Weights＆Biasesを使用してディレクトリを構成します

 **オプション**

| **オプション** | **説明** |
| :--- | :--- |
| -p, --project | 使用するプロジェクト。 |
| -e, --entity | プロジェクトのスコープを設定するエンティティ。 |
| --reset | 設定をリセット |
| -m, --mode | 「オンライン」、「オフライン」、「無効」のいずれかになります。 デフォルトは |
| --help | このメッセージを表示して終了します。 |

## wandb local

 **使用法**

`wandb local [OPTIONS]`

 **概要**

ローカルW＆Bコンテナを起動します（実験的）

**Options**

| **オプション** | **説明** |
| :--- | :--- |
| -p, --port | W＆Bローカルをバインドするホストポート |
| -e, --env | wandb/localに渡す環境変数 |
| --daemon | / --no-daemonデーモンモードで試行するか試行しない |
| --upgrade | 最新バージョンにアップグレードする |
| --help | このメッセージを表示して終了します。 |

## wandb login

 **使用法**

`wandb login [OPTIONS] [KEY]...`

 **概要**

 ウェイト＆バイアスにログイン

**オプション**

| **オプション** | **説明** |
| :--- | :--- |
| --cloud | ローカルではなくクラウドにログインする |
| --host | W＆Bの特定のインスタンスにログインします |
| --relogin | すでにログインしている場合は、強制的に再ログインします。 |
| --anonymously | 匿名でログインする |
| --help | このメッセージを表示して終了します。 |

## wandb offline

 **使用法**

`wandb offline [OPTIONS]`

**概要** 

W＆B同期を無効にする

**オプション**

| **オプション** | **説明** |
| :--- | :--- |
| --help | このメッセージを表示して終了します。 |

## wandb online

 **使用法**

`wandb online [OPTIONS]`

 **概要**

W＆B同期を有効にする

 **オプション**

| **オプション** | **説明** |
| :--- | :--- |
| --help | このメッセージを表示して終了します。 |

## wandb pull

 **使用法**

`wandb pull [OPTIONS] RUN`

 **概要**

 ウェイトとバイアスからファイルをプルする

 **オプション**

| オプション | **説明** |
| :--- | :--- |
| -p, --project | ダウンロードしたいプロジェクト。 |
| -e, --entity | リストのスコープを設定するエンティティ。 |
| --help | このメッセージを表示して終了します。 |

## wandb restore

 **使用法**

`wandb restore [OPTIONS] RUN`

**概要**

試行のためにコード、構成、およびDockerの状態を復元します

 **オプション**

| **オプション** | **説明** |
| :--- | :--- |
| --no-git | Skupp |
| --branch | /--no-branchブランチを作成するか、チェックアウトをデタッチするか |
| -p, --project | アップロードしたいプロジェクト。 |
| -e, --entity | リストのスコープを設定するエンティティ。 |
| --help | このメッセージを表示して終了します。 |

## wandb status

 **使用法**

`wandb status [OPTIONS]`

 **概要**

 構成設定を表示します

**オプション**

| **オプション** | **説明** |
| :--- | :--- |
| --settings | /--no-settings現在の設定を表示します |
| --help | このメッセージを表示して終了します。 |

## wandb sweep

 **使用法**

`wandb sweep [OPTIONS] CONFIG_YAML`

**概要**

スイープを作成します

**オプション**

| **オプション** | **説明** |
| :--- | :--- |
| -p, --project | スイープのプロジェクト。 |
| -e, --entity | プロジェクトのエンティティスコープ。 |
| --controller | ローカルコントローラーを試行する |
| --verbose | 詳細な出力を表示する |
| --name | スイープ名を設定する |
| --program | スイーププログラムを設定する |
| --update | 保留中のスイープを更新 |
| --help | このメッセージを表示して終了します。 |

## wandb sync

 **使用法**

`wandb sync [OPTIONS] [PATH]...`

**概要**

オフライントレーニングディレクトリをW＆Bにアップロードします

**オプション**

| **オプション** | **説明** |
| :--- | :--- |
| --id | アップロードするラン。 |
| -p, --project | アップロードするプロジェクト。 |
| -e, --entity | スコープするエンティティ。 |
| --include-globs | 含めるグロブのコンマ区切りリスト。 |
| --exclude-globs | 除外するグロブのコンマ区切りリスト。 |
| --include-online | / --no-include-online |
| Include | オンライン試行 |
| --include-offline | / --no-include-offline |
| Include | オフライン試行 |
| --include-synced | / --no-include-synced |
| Include | 同期された試行 |
| --mark-synced | / --no-mark-synced |
| Mark | 同期として試行 |
| --sync-all | すべての試行を同期する |
| --clean | 同期された試行を削除する |
| --clean-old-hours | この数時間前に作成された試行を削除します。 |
| To | --cleanフラグと一緒に使用します。 |
| --clean-force | 確認プロンプトなしでクリーニングします。 |
| --show | 表示する試行数 |
| --help | このメッセージを表示して終了します。 |

