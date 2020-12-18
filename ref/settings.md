# Settings

## wandb.sdk.wandb\_settings

 [\[ソースを表示\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_settings.py#L2)

このモジュールは、wandb試行の設定を構成します。

設定の読み込み順序:\(優先度とは異なります）デフォルト環境wandb.setup（settings=）system\_config Workspace\_config wandb.init（settings =）network\_org network\_entity network\_project

 設定の優先順位：「ソース」変数を参照してください。

 オーバーライドを使用する場合、オーバーライドしない設定よりも優先されます

オーバーライドの優先順位は、非オーバーライド設定の逆の順序です

###  設定オブジェクト

```python
class Settings(object)
```

 [\[ソースを表示\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_settings.py#L187)

設定コンストラクター

 **引数：**

*  `エンティティ`-試行に使用する個人ユーザーまたはチーム。
* `project` - 試行のプロジェクト名。

 **レイズ：**

* `例外`‐問題がある場合。

**\_\_copy\_\_**

```python
 | __copy__()
```

[ \[ソースを表示\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_settings.py#L656)

コピー（コピーされたオブジェクトはフリーズされないことに注意してください）。

試行リファレンス

