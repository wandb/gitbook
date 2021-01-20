# Summary

## wandb.sdk.wandb\_summary

[\[view\_source\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_summary.py#L2)

### SummaryDict Objects

```python
@six.add_metaclass(abc.ABCMeta)
class SummaryDict(object)
```

[\[view\_source\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_summary.py#L18)

dict-like which wraps all nested dictionraries in a SummarySubDict, and triggers self.\_root.\_callback on property changes.

### Summary Objects

```python
class Summary(SummaryDict)
```

[\[view\_source\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_summary.py#L78)

Summary

The summary statistics are used to track single metrics per model. Calling wandb.log\({'accuracy': 0.9}\) will automatically set wandb.summary\['accuracy'\] to be 0.9 unless the code has changed wandb.summary\['accuracy'\] manually.

Setting wandb.summary\['accuracy'\] manually can be useful if you want to keep a record of the accuracy of the best model while using wandb.log\(\) to keep a record of the accuracy at every step.

You may want to store evaluation metrics in a runs summary after training has completed. Summary can handle numpy arrays, pytorch tensors or tensorflow tensors. When a value is one of these types we persist the entire tensor in a binary file and store high level metrics in the summary object such as min, mean, variance, 95% percentile, etc.

**Examples**:

```text
wandb.init(config=args)

best_accuracy = 0
for epoch in range(1, args.epochs + 1):
test_loss, test_accuracy = test()
if (test_accuracy > best_accuracy):
wandb.run.summary["best_accuracy"] = test_accuracy
best_accuracy = test_accuracy
```

### SummarySubDict Objects

```python
class SummarySubDict(SummaryDict)
```

[\[view\_source\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_summary.py#L128)

Non-root node of the summary data structure. Contains a path to itself from the root.

