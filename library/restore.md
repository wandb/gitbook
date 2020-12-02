---
description: モデルチェックポイントなどのファイルをローカルの実行フォルダーに復元して、スクリプトでアクセスします
---

# wandb.restore\(\)

## **概要**

* `wandb.restore（filename）`を呼び出すと、ファイルがローカルの実行ディレクトリに復元されます。通常、filenameは、以前の実験の実行によって生成され、クラウドにアップロードされたファイルを指します。この呼び出しにより、ファイルのローカルコピーが作成され、読み取り用に開いているローカルファイルストリームが返されます。

  `wandb.restore`は、いくつかのオプションのキーワード引数を受け入れます。

  ·      **run\_path―**ファイルをプルする前の実行を参照する文字列は、'$ENTITY\_NAME/$PROJECT\_NAME/$RUN\_ID' or '$PROJECT\_NAME/$RUN\_ID'（デフォルト：現在のエンティティ、プロジェクト名、および実行ID）の形式になります。

  ·      **replace―**ローカルコピーが使用可能であることが判明した場合に、ファイル名のローカルコピーをクラウドコピーで上書きするかどうかを指定するブール値（デフォルト：False）

  ·      **root―**ファイルのローカルコピーを保存するディレクトリを指定する文字列。これはデフォルトで現在の作業ディレクトリになります。または、`wandb.run.dir`if wandb.initが以前に呼び出されました（デフォルト："."）



一般的な使用例：

*   過去の実行によって生成されたモデルアーキテクチャまたは重みを復元します
*  失敗した場合は、最後のチェックポイントからトレーニングを[再開](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MNTo635YwwyToLxk-CQ/v/japanese/library/resuming)します（重要な詳細については、再開に関するセクションを参照してください）。

##  **例**

完全な実例については、[このレポート](https://app.wandb.ai/lavanyashukla/save_and_restore/reports/Saving-and-Restoring-Models-with-W%26B--Vmlldzo3MDQ3Mw)を参照してください。

```python
# restore a model file from a specific run by user "vanpelt" in "my-project"
best_model = wandb.restore('model-best.h5', run_path="vanpelt/my-project/a1b2c3d")

# restore a weights file from a checkpoint
# (NOTE: resuming must be configured if run_path is not provided)
weights_file = wandb.restore('weights.h5')
# use the "name" attribute of the returned object
# if your framework expects a filename, e.g. as in Keras
my_predefined_model.load_weights(weights_file.name)
```

> x run\_pathを指定しない場合は、実行の[再開](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MNTo635YwwyToLxk-CQ/v/japanese/library/resuming)を構成する必要があります。トレーニング以外でプログラムでファイルにアクセスする場合は、Run APIを使用します。
>
> >

