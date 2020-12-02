---
description: ファイルをクラウドに保存して、現在の実行を関連付けます
---

# wandb.save\(\)

実行に関連付けるファイルを保存する方法は2つあります。

1. `wandb.save（filename）`を使用します。
2. ファイルをwandbrunディレクトリに置くと、実行の最後にアップロードされます。

{% hint style="info" %}
runを[再開](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MNTo635YwwyToLxk-CQ/v/japanese/library/resuming)する場合は、wandb.restore（filename）を呼び出すことでファイルを回復できます。
{% endhint %}

書き込み中にファイルを同期する場合は、`wandb.save`でファイル名またはglobを指定できます。

##  **wandb.saveの例**

 完全な実例については、[このレポート](https://wandb.ai/lavanyashukla/save_and_restore/reports/Saving-and-Restoring-Models-with-W&B--Vmlldzo3MDQ3Mw)を参照してください。

```python
# Save a model file from the current directory
wandb.save('model.h5')

# Save all files that currently exist containing the substring "ckpt"
wandb.save('../logs/*ckpt*')

# Save any files starting with "checkpoint" as they're written to
wandb.save(os.path.join(wandb.run.dir, "checkpoint*"))
```

{% hint style="info" %}
W＆Bのローカル実行ディレクトリはデフォルトでスクリプトに関連する./wandbディレクトリ内にあり、パスはrun-20171023\_105053-3o4933r0のようになります。ここで、20171023\_105053はタイムスタンプ、3o4933r0は実行のIDです。 WANDB\_DIR環境変数を設定するか、wandb.initのdirキーワード引数を絶対パスに設定すると、代わりにそのディレクトリ内にファイルが書き込まれます。
{% endhint %}

### **wandbrunディレクトリにファイルを保存する例**

ファイル「model.h5」はwandb.run.dirに保存され、トレーニングの最後にアップロードされます

```python
import wandb
wandb.init()

model.fit(X_train, y_train,  validation_data=(X_test, y_test),
    callbacks=[wandb.keras.WandbCallback()])
model.save(os.path.join(wandb.run.dir, "model.h5"))
```

これが公開のサンプルページです。\[ファイル\]タブで、model-best.h5があります。これは、Keras統合によってデフォルトで自動的に保存されますが、チェックポイントを手動で保存することができ、あなたの実行に関連して保存されます。

[実例を見る→](https://wandb.ai/wandb/neurips-demo/runs/206aacqo/files)

![](../.gitbook/assets/image%20%2844%29.png)

##  **よくある質問**

###  **特定のファイルを無視します**

`wandb/settings`ファイルを編集して、ignore\_globsをコンマで区切られたグロブのリストと同じに設定できます。**WANDB\_IGNORE\_GLOBS**環境変数を設定することもできます。一般的な使用例は、自動的に作成するgitパッチがアップロードされないようにすることです。つまり、**WANDB\_IGNORE\_GLOBS=\*.patch**です。

**実行が終了する前にファイルを同期します**

長時間実行している場合は、実行が終了する前に、モデルチェックポイントなどのファイルがクラウドにアップロードされていることを確認することをお勧めします。デフォルトでは、実行が終了するまでほとんどのファイルのアップロードを待機します。スクリプトに`wandb.save('*.pth')`または単に`wandb.save('latest.pth')`を追加して、ファイルが書き込まれたり更新されたりするたびにそれらのファイルをアップロードできます。

**ファイルを保存するためのディレクトリを変更します**

デフォルトでAWSS3またはGoogleCloud Storageにファイルを保存すると、`events.out.tfevents.1581193870.gpt-tpu-finetune-8jzqk-2033426287 is a cloud storage url, can't save file to wandb`のようなエラーが発生する可能性があります。

TensorBoardイベントファイルまたは同期するその他のファイルのログディレクトリを変更するには、ファイルをwandb.run.dirに保存して、クラウドに同期させます。

### Get the run name

**実行名を取得します**

スクリプト内から実行名を使用する場合は、`wandb.run.name`を使用すると、実行名（たとえば、「blissful-waterfall-2」）を取得できます。

表示名にアクセスする前に、実行時にsaveを呼び出す必要があります。

```text
run = wandb.init(...)
run.save()
print(run.name)
```

### **保存したすべてのファイルをwandbにプッシュします**

wandb.initの後にスクリプトの先頭で`wandb.save("*.pt")`を1回呼び出すと、そのパターンに一致するすべてのファイルは、wandb.run.dirに書き込まれるとすぐに保存されます。

**クラウドストレージに同期されているローカルファイルを削除します**

クラウドストレージにすでに同期されているローカルファイルを削除するために実行できるコマンド`wandb gc`があります。使用法の詳細については、\`wandb gc—helpを参照してください。

