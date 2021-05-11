# summary



[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/v0.10.30/wandb/sdk/wandb_summary.py#L82-L134)




Tracks single values for each metric for each run.

<pre><code>summary(
    get_current_summary_callback: t.Callable
)</code></pre>




By default, a metric's summary is the last value of its History.

For example, `wandb.log({'accuracy': 0.9})` will add a new step to History and
update Summary to the latest value. In some cases, it's more useful to have
the maximum or minimum of a metric instead of the final value. You can set
history manually `(wandb.summary['accuracy'] = best_acc)`.

In the UI, summary metrics appear in the table to compare across runs.
Summary metrics are also used in visualizations like the scatter plot and
parallel coordinates chart.

After training has completed, you may want to save evaluation metrics to a
run. Summary can handle numpy arrays and PyTorch/TensorFlow tensors. When
you save one of these types to Summary, we persist the entire tensor in a
binary file and store high level metrics in the summary object, such as min,
mean, variance, and 95th percentile.

#### Examples:

```python
wandb.init(config=args)

best_accuracy = 0
for epoch in range(1, args.epochs + 1):
test_loss, test_accuracy = test()
if (test_accuracy > best_accuracy):
    wandb.run.summary["best_accuracy"] = test_accuracy
    best_accuracy = test_accuracy
```
