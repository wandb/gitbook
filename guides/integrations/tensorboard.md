# TensorBoard

W\&B supports patching TensorBoard to automatically log all the metrics from your script into our [rich, interactive dashboards](../track/app.md).

```python
import wandb
# Just start a W&B run, passing `sync_tensorboard=True)`, to plot your Tensorboard files
wandb.init(project='my-project', sync_tensorboard=True)

# Your Keras, Tensorflow or PyTorch training code using TensorBoard
...

# If running in a notebook, finish the wandb run to upload the tensorboard logs to W&B
wandb.finish()
```

We support TensorBoard with all versions of TensorFlow. W\&B also supports TensorBoard > 1.14 with PyTorch as well as TensorBoardX.



## How is W\&B different from TensorBoard?

When the cofounders started working on W\&B, they were inspired to build a tool for the frustrated TensorBoard users at OpenAI. Here are a few things we've focused on improving:

1. **Reproduce models**: Weights & Biases is good for experimentation, exploration, and reproducing models later. We capture not just the metrics, but also the hyperparameters and version of the code, and we can save your version-control status and model checkpoints for you so your project is reproducible.
2. **Automatic organization**: Whether you're picking up a project from a collaborator, coming back from a vacation, or dusting off an old project, W\&B makes it easy to see all the models that have been tried so no one wastes hours, GPU cycles, or carbon re-running experiments.
3. **Fast, flexible integration**: Add W\&B to your project in 5 minutes. Install our free open-source Python package and add a couple of lines to your code, and every time you run your model you'll have nice logged metrics and records.
4. **Persistent, centralized dashboard**: No matter where you train your models, whether on your local machine, in a shared lab cluster, or on spot instances in the cloud, your results are shared to the same centralized dashboard. You don't need to spend your time copying and organizing TensorBoard files from different machines.
5. **Powerful tables**: Search, filter, sort, and group results from different models. It's easy to look over thousands of model versions and find the best performing models for different tasks. TensorBoard isn't built to work well on large projects.
6. **Tools for collaboration**: Use W\&B to organize complex machine learning projects. It's easy to share a link to W\&B, and you can use private teams to have everyone sending results to a shared project. We also support collaboration via reports— add interactive visualizations and describe your work in markdown. This is a great way to keep a work log, share findings with your supervisor, or present findings to your lab or team.

Get started with a [free personal account →](https://wandb.ai)

## Common questions

### How can I log metrics to W\&B that aren't logged to TensorBoard?

If you need to log additional custom metrics that aren't being logged to TensorBoard, you can call `wandb.log` in your code `wandb.log({"custom": 0.8})`

Setting the step argument in `wandb.log` is disabled when syncing Tensorboard. If you'd like to set a different step count, you can log the metrics with a step metric as:

`wandb.log({"custom": 0.8, "global_step": global_step})`

### How do I configure Tensorboard when I'm using it with `wandb`?

If you want more control over how TensorBoard is patched you can call `wandb.tensorboard.patch` instead of passing `sync_tensorboard=True` to `wandb.init`.

```python
import wandb
wandb.tensorboard.patch(root_logdir="<logging_directory>")
wandb.init()

# If running in a notebook, finish the wandb run to upload the tensorboard logs to W&B
wandb.finish()
```

You can pass `tensorboard_x=False` to this method to ensure vanilla TensorBoard is patched, if you're using TensorBoard > 1.14 with PyTorch you can pass `pytorch=True` to ensure it's patched. Both of these options have smart defaults depending on what versions of these libraries have been imported.

By default, we also sync the `tfevents` files and any `.pbtxt` files. This enables us to launch a TensorBoard instance on your behalf. You will see a [TensorBoard tab](https://www.wandb.com/articles/hosted-tensorboard) on the run page. This behavior can be disabled by passing `save=False` to `wandb.tensorboard.patch`

```python
import wandb
wandb.init()
wandb.tensorboard.patch(save=False, tensorboard_x=True)

# If running in a notebook, finish the wandb run to upload the tensorboard logs to W&B
wandb.finish()
```

{% hint style="warning" %}
You must call either `wandb.init` or `wandb.tensorboard.patch` **before** calling `tf.summary.create_file_writer` or constructing a`SummaryWriter` via `torch.utils.tensorboard`.
{% endhint %}

### Syncing Previous TensorBoard Runs

If you have existing `tfevents` files stored locally and you would like to import them into W\&B, you can run `wandb sync log_dir`, where `log_dir` is a local directory containing the `tfevents` files.

### Google Colab, Jupyter and TensorBoard

If running your code in a Jupyter or Colab notebook, make sure to call `wandb.finish()` and the end of your training. This will finish the wandb run and upload the tensorboard logs to W\&B so they can be visualised. This is not necessary when running a `.py` script as wandb finishes automatically when a script finishes.

To run shell commands in a notebook environment, you must prepend a `!`, as in `!wandb sync directoryname`.

### PyTorch and TensorBoard

If you use PyTorch's TensorBoard integration, you may need to manually upload the PyTorch Profiler JSON file**:**&#x20;

```
wandb.save(glob.glob(f"runs/*.pt.trace.json")[0], base_path=f"runs")
```
