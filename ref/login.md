# Login

## wandb.sdk.wandb\_login

 [\[ソースを表示\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_login.py#L3)

Weights＆Biasesにログインし、マシンを認証してデータをアカウントに記録します。

 **ログイン**

```python
login(anonymous=None, key=None, relogin=None, host=None, force=None)
```

 [\[ソースを表示\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_login.py#L22)

 W＆Bにログインします。

**引数：**

*  匿名文字列、オプション-「必須」、「許可」、または「なし」にすることができます。「must」に設定されている場合は常に匿名でログインし、「allow」に設定されている場合はユーザーがまだログインしていない場合にのみ匿名ユーザーを作成します。
* まだログインしていません。
* `relogin` bool、オプション-trueの場合、APIキーの再プロンプトが表示されま
* ホスト文字列、オプション-接続するホスト。

**戻り値：**

* `bool` ーが構成されている場合

**レイズ：**

UsageError-api\_keyを構成できず、ttyがない場合

