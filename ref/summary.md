# Summary

## wandb.sdk.wandb\_summary

 [\[ソースを表示\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_summary.py#L2)

### SummaryDictオブジェクト

```python
@six.add_metaclass(abc.ABCMeta)
class SummaryDict(object)
```

 [\[ソースを表示\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_summary.py#L18)

dict-likeは、ネストされたすべての辞書をSummarySubDictにラップし、プロパティの変更時にself.\_root.\_callbackをトリガーします。

### サマリーオブジェクト

```python
class Summary(SummaryDict)
```

 [\[ソースを表示\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_summary.py#L78)

概要

要約統計量は、モデルごとに単一のメトリックを追跡するために使用されます。wandb.log（{'accuracy'：0.9}）を呼び出すと、コードがwandb.summary\['accuracy'\]を手動で変更しない限り、wandb.summary\['accuracy'\]が自動的に0.9に設定されます。

wandb.log（）を使用してすべてのステップで精度の記録を保持しながら、最適なモデルの精度の記録を保持したい場合は、wandb.summary\['accuracy'\]を手動で設定すると便利です。

トレーニングが完了した後、評価メトリックを試行サマリーに保存することをお勧めします。Summaryは、numpy配列、pytorchテンソル、またはtensorflowテンソルを処理できます。値がこれらのタイプの1つである場合、テンソル全体をバイナリファイルに永続化し、最小、平均、分散、95％パーセンタイルなどの高レベルのメトリックをサマリーオブジェクトに格納します。

 **例：**

```text
wandb.init(config=args)

best_accuracy = 0
for epoch in range(1, args.epochs + 1):
test_loss, test_accuracy = test()
if (test_accuracy > best_accuracy):
wandb.run.summary["best_accuracy"] = test_accuracy
best_accuracy = test_accuracy
```

SummarySubDictオブジェクト

```python
class SummarySubDict(SummaryDict)
```

 [\[ソースを表示\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_summary.py#L128)

サマリーデータ構造の非ルートノード。ルートからそれ自体へのパスが含まれます。

