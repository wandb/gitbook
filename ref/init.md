# Init

## wandb.sdk.wandb\_init

 [\[ソースを表示\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_init.py#L3)

 wandb.init（）は、新しい試行の開始を示します。たとえば、MLトレーニングパイプラインでは、トレーニングスクリプトと評価スクリプトの先頭にwandb.init（）を追加すると、これらの各ステップがW＆Bでの試行として追跡されます。

### \_WandbInit オブジェクト

```python
class _WandbInit(object)
```

 [\[ソースを表示\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_init.py#L50)

 **設定**

```python
 | setup(kwargs)
```

 [\[ソースを表示\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_init.py#L62)

wandb.init（）の完全な設定。これには、すべての引数の解析、設定による適用、およびロギングの有効化が含まれます。

**init**

```python
init(job_type: Optional[str] = None, dir=None, config: Union[Dict, str, None] = None, project: Optional[str] = None, entity: Optional[str] = None, reinit: bool = None, tags: Optional[Sequence] = None, group: Optional[str] = None, name: Optional[str] = None, notes: Optional[str] = None, magic: Union[dict, str, bool] = None, config_exclude_keys=None, config_include_keys=None, anonymous: Optional[str] = None, mode: Optional[str] = None, allow_val_change: Optional[bool] = None, resume: Optional[Union[bool, str]] = None, force: Optional[bool] = None, tensorboard=None, sync_tensorboard=None, monitor_gym=None, save_code=None, id=None, settings: Union[Settings, Dict[str, Any], None] = None) -> Union[Run, Dummy]
```

 [\[ソースを表示\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_init.py#L450)

W＆Bの初期化ローカルで試行を開始または再開し、wandbサーバーと通信するための新しいプロセスを生成します。 wandb.logを呼び出す前に呼び出す必要があります。

**引数：**

* `job_type` _str_、オプション-試行中のジョブのタイプ。デフォルトは「train」です。
* `dir` _str_、オプション–メタデータが保存されるディレクトリへの絶対パス

  試行時に保存する構成パラメーター（通常はハイパーパラメーター）を設定します。wandb.configも参照してください。

  dict, argparse or absl.flags：の場合、キーと値のペアが試行構成オブジェクトにロードされます。

  str：がconfigパラメーターを含むyamlファイルを探し、それらを試行のconfigオブジェクトにロードする場合。

* `project` _str_、オプション-W＆Bプロジェクト。
* `entity` _str_、オプション-W＆Bエンティティ。
* `reinit` _bool_、オプション-同じプロセスで複数の呼び出しを初期化できるようにします。
* タグリスト、オプション–試行に適用するタグのリスト。
* `group` _str_、オプション–特定のグループ内のすべての試行で共有されるユニークの文字列。
* `name` _str_、オプション–試行の表示名。ユニークである必要はありません。
* `notes` _str_、オプション-試行に関連付けられた複数行の文字列。
* `magic` bool、dict、またはstr、オプション-bool、dict、json string、yamlファイル名としての魔法の構成。
*  `config_exclude_keys`リスト、オプション-configを指定するときにW＆Bへの保存を除外する文字列キー。
*  `config_include_keys`リスト、オプション-configを指定するときにW＆Bに格納する文字列キー。
* `anonymous`  str、オプション-「allow」、「must」、または「never」にすることができます。匿名ロギングを許可するかどうかを制御します。デフォルトはneverです。
* `mode` _str_、オプション-「オンライン」、「オフライン」、または「無効」にすることができます。デフォルトはオンラインです。
* `allow_val_change` 、オプション-設定後に構成値を変更できるようにします。jupyterではデフォルトでtrueになり、それ以外の場合はfalseになります。
* `resume` _bool, str, optional_ - 再開動作を設定します。「許可」、「必須」、「決して」、「自動」、または「なし」のいずれかである必要があります。デフォルトはNoneです。

   ケース：

* "auto"（またはTrue）：同じマシンでの前回の試行を自動的に再開します。前の試行がクラッシュした場合、それ以外の場合は新しい試行を開始します。
* "allow"：idがinit（id="UNIQUE\_ID"）またはWANDB\_RUN\_ID="UNIQUE\_ID"で設定されていて、それが前の試行と同じである場合、wandbは自動的にidで試行を再開します。それ以外の場合、wandbは新しい試行を開始します。
* "never"：idがinit（id="UNIQUE\_ID"）またはWANDB\_RUN\_ID="UNIQUE\_ID"で設定されていて、前の試行と同じである場合、wandbはクラッシュします。
* 「必須」：idがinit（id="UNIQUE\_ID"）またはWANDB\_RUN\_ID="UNIQUE\_ID"で設定されていて、前の試行と同じである場合、wandbは自動的にidで試行を再開します。そうしないと、wandbがクラッシュします。
* なし：再開しない-試行に重複するrun\_idがある場合、前の試行が上書きされます。

  は、[https://docs.wandb.com/library/advanced/resuming](https://docs.wandb.com/library/advanced/resuming)を参照してください。

* `force` bool、オプション-trueの場合、ユーザーがwandbサーバーにログインできない、またはログインしていないと、スクリプトがクラッシュします。falseの場合、ユーザーがwandbサーバーにログインできない、またはログインしていない場合、スクリプトはオフラインモードで試行されます。デフォルトはfalseです。
* `sync_tensorboard` オプション‐tensorboardまたはtensorboardXからのwandbログを同期し、関連するイベントファイルを保存します。デフォルトはfalseです。
* `monitor_gym` - bool、オプション）：OpenAI Gymを使用するときに環境のビデオを自動的にログに記録します（[https://docs.wandb.com/library/integrations/openai-gym](https://docs.wandb.com/library/integrations/openai-gym)を参照）。デフォルトはfalseです。
* `save_code` 、オプション-エントリポイントまたはjupyterセッション履歴のソースコードを保存します。
* `id` _str_、オプション–試行のグローバルにユニークの（プロジェクトごとの）識別子。これは主に再開に使用されます。

 **例：**

 基本的な使い方

```text
wandb.init()
```

同じスクリプトから複数の試行を起動します

```text
for x in range(10):
with wandb.init(project="my-projo") as run:
for y in range(100):
- `run.log({"metric"` - x+y})
```

 **レイズ：**

* `例外`‐問題がある場合。

 **戻り値：**

 `Run`オブジェクト。

