# TensorBoard

W&B supports patching TensorBoard to automatically log all the metrics from your script into our native charts.

```python
import wandb
wandb.init(sync_tensorboard=True)
```

We support TensorBoard with all versions of TensorFlow. If you're using TensorBoard with another framework W&B supports TensorBoard &gt; 1.14 with PyTorch as well as TensorBoardX.

### Custom Metrics

If you need to log additional custom metrics that aren't being logged to TensorBoard, you can call `wandb.log` in your code with the same step argument that TensorBoard is using: i.e. `wandb.log({"custom": 0.8}, step=global_step)`

### Advanced Configuration

If you want more control over how TensorBoard is patched you can call `wandb.tensorboard.patch` instead of passing `sync_tensorboard=True` to init. You can pass `tensorboardX=False` to this method to ensure vanilla TensorBoard is patched, if you're using TensorBoard &gt; 1.14 with PyTorch you can pass `pytorch=True` to ensure it's patched. Both of these options are have smart defaults depending on what versions of these libraries have been imported.

By default we also sync the `tfevents` files and any `.pbtxt` files. This enables us to launch a TensorBoard instance on your behalf. You will see a [TensorBoard tab](https://www.wandb.com/articles/hosted-tensorboard) on the run page. This behavior can be disabled by passing `save=False` to `wandb.tensorboard.patch`

```python
import wandb
wandb.init()
wandb.tensorboard.patch(save=False, tensorboardX=True)
```

**Note:** if your script instantiates a File Writer via `tf.summary.create_file_writer`, you must call either `wandb.init` or `wandb.tensorboard.patch` before constructing the File Writer.

### Syncing Previous TensorBoard Runs

If you have existing `tfevents` files stored locally that were already generated using the wandb library integration and you would like to import them into wandb, you can run `wandb sync log_dir`, where `log_dir` is a local directory containing the `tfevents` files.

You can also run `wandb sync directory_with_tf_event_file`

```bash
"""This script will import a directory of tfevents files into a single W&B run.
You must install wandb from a special branch until this feature is merged
into the mainline:""" 
pip install --upgrade git+git://github.com/wandb/client.git@feature/import#egg=wandb
```

You can call this script with `python no_image_import.py dir_with_tf_event_file`. This will create a single run in wandb with metrics from the event files in that directory. If you want to run this on many directories, you should only execute this script once per run, so a loader might look like:

```python
import glob
for run_dir in glob.glob("logdir-*"):
  subprocess.Popen(["python", "no_image_import.py", run_dir],
                   stderr=subprocess.PIPE, stdout=subprocess.PIPE)
```

```python
import glob
import os
import wandb
import sys
import time
import tensorflow as tf
from wandb.tensorboard.watcher import Consumer, Event
from six.moves import queue

if len(sys.argv) == 1:
    raise ValueError("Must pass a directory as the first argument")

paths = glob.glob(sys.argv[1]+"/*/.tfevents.*", recursive=True)
root = os.path.dirname(os.path.commonprefix(paths)).strip("/")
namespaces = {path: path.replace(root, "")\
              .replace(path.split("/")[-1], "").strip("/")
              for path in paths}
finished = {namespace: False for path, namespace in namespaces.items()}
readers = [(namespaces[path], tf.train.summary_iterator(path)) for path in paths] 
if len(readers) == 0: 
    raise ValueError("Couldn't find any event files in this directory")

directory = os.path.abspath(sys.argv[1])
print("Loading directory %s" % directory)
wandb.init(project="test-detection")

Q = queue.PriorityQueue()
print("Parsing %i event files" % len(readers))
con = Consumer(Q, delay=5)
con.start()
total_events = 0

while True:

    # Consume 500 events at a time from all readers and push them to the queue
    for namespace, reader in readers:
        if not finished[namespace]:
            for i in range(500):
                try:
                    event = next(reader)
                    kind = event.value.WhichOneof("value")
                    if kind != "image":
                        Q.put(Event(event, namespace=namespace))
                        total_events += 1
                except StopIteration:
                    finished[namespace] = True
                    print("Finished parsing %s event file" % namespace)
                    break
    if all(finished.values()):
        break

print("Persisting %i events..." % total_events)
con.shutdown(); print("Import complete")
```

### Google Colab and TensorBoard

To run commands from the command line in Colab, you must run `!wandb sync directoryname` . Currently TensorBoard syncing does not work in a notebook environment for Tensorflow 2.1+. You can have your Colab use an earlier version of TensorBoard, or run a script from the command line with `!python your_script.py` .

## How is W&B different from TensorBoard?

We were inspired to improve experiment tracking tools for everyone. When the cofounders started working on W&B, they were inspired to build a tool for the frustrated TensorBoard users at OpenAI. Here are a few things we focused on improving:

1. **Reproduce models**: Weights & Biases is good for experimentation, exploration, and reproducing models later. We capture not just the metrics, but also the hyperparameters and version of the code, and we can save your model checkpoints for you so your project is reproducible. 
2. **Automatic organization**: If you hand off a project to a collaborator or take a vacation, W&B makes it easy to see all the models you've tried so you're not wasting hours re-running old experiments.
3. **Fast, flexible integration**: Add W&B to your project in 5 minutes. Install our free open-source Python package and add a couple of lines to your code, and every time you run your model you'll have nice logged metrics and records.
4. **Persistent, centralized dashboard**: Anywhere you train your models, whether on your local machine, your lab cluster, or spot instances in the cloud, we give you the same centralized dashboard. You don't need to spend your time copying and organizing TensorBoard files from different machines.
5. **Powerful table**: Search, filter, sort, and group results from different models. It's easy to look over thousands of model versions and find the best performing models for different tasks. TensorBoard isn't built to work well on large projects.
6. **Tools for collaboration**: Use W&B to organize complex machine learning projects. It's easy to share a link to W&B, and you can use private teams to have everyone sending results to a shared project. We also support collaboration via reports— add interactive visualizations and describe your work in markdown. This is a great way to keep a work log, share findings with your supervisor, or present findings to your lab.

Get started with a [free personal account →](http://app.wandb.ai/)

