# Environment Variables

自動化された環境でスクリプトを実行している場合、スクリプトの実行前またはスクリプト内で設定された環境変数を使用して**wandb**を制御できます。

```bash
# This is secret and shouldn't be checked into version control
WANDB_API_KEY=$YOUR_API_KEY
# Name and notes optional
WANDB_NAME="My first run"
WANDB_NOTES="Smaller learning rate, more regularization."
```

```bash
# Only needed if you don't checkin the wandb/settings file
WANDB_ENTITY=$username
WANDB_PROJECT=$project
```

```python
# If you don't want your script to sync to the cloud
os.environ['WANDB_MODE'] = 'dryrun'
```

## オプショナル環境変数

これらのオプションの環境変数を用いて、リモートマシンでの認証の設定などを行います。

| Variable name | Usage |
| :--- | :--- |
| **WANDB\_API\_KEY** | Sets the authentication key associated with your account. You can find your key on [your settings page](https://app.wandb.ai/settings). This must be set if `wandb login` hasn't been run on the remote machine. |
| **WANDB\_BASE\_URL** | If you're using [wandb/local](../self-hosted/) you should set this environment variable to `http://YOUR_IP:YOUR_PORT` |
| **WANDB\_NAME** | The human-readable name of your run. If not set it will be randomly generated for you |
| **WANDB\_NOTES** | Longer notes about your run.  Markdown is allowed and you can edit this later in the UI. |
| **WANDB\_ENTITY** | The entity associated with your run. If you have run `wandb init` in the directory of your training script, it will create a directory named _wandb_ and will save a default entity which can be checked into source control. If you don't want to create that file or want to override the file you can use the environmental variable. |
| **WANDB\_USERNAME** | The username of a member of your team associated with the run. This can be used along with a service account API key to enable attribution of automated runs to members of your team. |
| **WANDB\_PROJECT** | The project associated with your run. This can also be set with `wandb init`, but the environmental variable will override the value. |
| **WANDB\_MODE** | By default this is set to _run_ which saves results to wandb. If you just want to save your run metadata locally, you can set this to _dryrun_. |
| **WANDB\_TAGS** | A comma separated list of tags to be applied to the run. |
| **WANDB\_DIR** | Set this to an absolute path to store all generated files here instead of the _wandb_ directory relative to your training script. _be sure this directory exists and the user your process runs as can write to it_ |
| **WANDB\_RESUME** | By default this is set to _never_. If set to _auto_ wandb will automatically resume failed runs. If set to _must_ forces the run to exist on startup. If you want to always generate your own unique ids, set this to _allow_ and always set **WANDB\_RUN\_ID**. |
| **WANDB\_RUN\_ID** | Set this to a globally unique string \(per project\) corresponding to a single run of your script. It must be no longer than 64 characters. All non-word characters will be converted to dashes. This can be used to resume an existing run in cases of failure. |
| **WANDB\_IGNORE\_GLOBS** | Set this to a comma separated list of file globs to ignore. These files will not be synced to the cloud. |
| **WANDB\_ERROR\_REPORTING** | Set this to false to prevent wandb from logging fatal errors to its error tracking system. |
| **WANDB\_SHOW\_RUN** | Set this to **true** to automatically open a browser with the run url if your operating system supports it. |
| **WANDB\_DOCKER** | Set this to a docker image digest to enable restoring of runs. This is set automatically with the wandb docker command. You can obtain an image digest by running `wandb docker my/image/name:tag --digest` |
| **WANDB\_DISABLE\_CODE** | Set this to true to prevent wandb from storing a reference to your source code |
| **WANDB\_ANONYMOUS** | Set this to "allow", "never", or "must" to let users create anonymous runs with secret urls. |
| **WANDB\_CONSOLE** | Set this to "off" to disable stdout / stderr logging.  This defaults to "on" in environments that support it. |
| **WANDB\_CONFIG\_PATHS** | Comma separated list of yaml files to load into wandb.config.  See [config](config.md#file-based-configs). |
| **WANDB\_CONFIG\_DIR** | This defaults to ~/.config/wandb, you can override this location with this environment variable |
| **WANDB\_NOTEBOOK\_NAME** | If you're running in jupyter you can set the name of the notebook with this variable. We attempt to auto detect this. |
| **WANDB\_HOST** | Set this to the hostname you want to see in the wandb interface if you don't want to use the system provided hostname |
| **WANDB\_SILENT** | Set this to **true** to silence wandb log statements. If this is set all logs will be written to **WANDB\_DIR**/debug.log |
| **WANDB\_RUN\_GROUP** | Specify the experiment name to automatically group runs together. See [grouping](grouping.md) for more info. |
| **WANDB\_JOB\_TYPE** | Specify the job type, like "training" or "evaluation" to indicate different types of runs. See [grouping](grouping.md) for more info. |

## Singularity環境

[Singularity](https://singularity.lbl.gov/index.html)でコンテナーを実行している場合は、上記の変数の前に**SINGULARITYENV\_**を付けることで、環境変数を渡すことができます。Singularity環境変数の詳細については、[こちら](https://singularity.lbl.gov/docs-environment-metadata#environment)をご覧ください。

## AWSで実行

AWSでバッチジョブを実行している場合は、W＆B認証情報を使用してマシンを簡単に認証できます。[設定ページ](https://wandb.ai/settings)からAPIキーを取得し、[AWSバッチジョブ仕様](https://docs.aws.amazon.com/batch/latest/userguide/job_definition_parameters.html#parameters)でWANDB\_API\_KEY環境変数を設定します。

## よくある質問

**自動実行とサービスアカウント**W＆Bへの実行ログを起動する自動テストまたは内部ツールがある場合は、チーム設定ページで**サービスアカウント**を作成します。これにより、自動化されたジョブにサービスAPIキーを使用できるようになります。サービスアカウントジョブを特定のユーザーに関連付ける場合は、WANDB\_USER\_NAMEまたはWANDB\_USER\_EMAIL環境変数を使用できます。

![ &#x81EA;&#x52D5;&#x5316;&#x3055;&#x308C;&#x305F;&#x30B8;&#x30E7;&#x30D6;&#x306E;&#x30B5;&#x30FC;&#x30D3;&#x30B9;&#x30A2;&#x30AB;&#x30A6;&#x30F3;&#x30C8;&#x3092;&#x30C1;&#x30FC;&#x30E0;&#x8A2D;&#x5B9A;&#x30DA;&#x30FC;&#x30B8;&#x3067;&#x4F5C;&#x6210;&#x3057;&#x307E;&#x3059;](../.gitbook/assets/image%20%2892%29.png)

これは、自動化された単体テストを設定する場合に、継続的インテグレーションやTravisCIやCircleCIなどのツールに役立ちます。

### 環境変数は、wandb.init（）に渡されたパラメータを上書きしますか？

wandb.initに渡される引数は、環境よりも優先されます。環境変数が設定されていないときにシステムデフォルト以外のデフォルトを設定する場合は、wandb.init `wandb.init(dir=os.getenv("WANDB_DIR", my_default_override))`を呼び出すことができます。

### ログをオフにします

コマンドwandboffは、環境変数, `WANDB_MODE=dryrun`を設定します。これにより、マシンからリモートwandbサーバーへのデータの同期が停止します。複数のプロジェクトがある場合、それらはすべて、ログに記録されたデータのW＆Bサーバーへの同期を停止します。

警告メッセージを無効にするには：

```python
import logging
logger = logging.getLogger("wandb")
logger.setLevel(logging.WARNING)
```

### 共有マシン上の複数のwandbユーザー

共有マシンを使用していて、他の人がwandbユーザーである場合、実行が常に適切なアカウントに記録されていることを確認するのは簡単です。認証するようにWANDB\_API\_KEY環境変数を設定します。環境でソースを作成する場合、ログインすると適切な資格情報が得られます。または、スクリプトから環境変数を設定することもできます。この`export WANDB_API_KEY=X`コマンドを実行します。ここで、XはAPIキーです。ログインすると、APIキーは[wandb.ai/authorize](https://wandb.ai/authorize)にあります。

