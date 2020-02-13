---
description: 'Keep track of metrics, video, custom plots, and more'
---

# wandb.log\(\)

Calling `wandb.log(dict)` logs the keys and values of the dictionary passed in and associates the values with a _step_. Wandb.log can log histograms and custom matplotlib objects and rich media. For a complete list of supported data types see our [Data Types ](reference/data_types.md)reference.

`wandb.log(dict)` accepts a few keyword arguments:

* **step** — Step to associate the log with \(see [Incremental Logging](log.md#incremental-logging)\)
* **commit** — If true, increments the step associated with the log\(_default: true_\)

### Example

Any time you call `wandb.log` and pass in a dictionary of keys and values, it will be saved as a new time step for plots in the W&B app.

```python
wandb.log({'accuracy': 0.9, 'epoch': 5})
```

### Incremental Logging

If you want to log to a single history step from lots of different places in your code you can pass a step index to `wandb.log()` as follows:

```python
wandb.log({'loss': 0.2}, step=step)
```

As long as you keep passing the same value for `step`, W&B will collect the keys and values from each call in one unified dictionary. As soon you call `wandb.log()` with a different value for `step` than the previous one, W&B will write all the collected keys and values to the history, and start collection over again. Note that this means you should only use this with consecutive values for `step`: 0, 1, 2, .... This feature doesn't let you write to absolutely any history step that you'd like, only the "current" one and the "next" one.

You can also set **commit=False** in `wandb.log` to accumulate metrics, just be sure to call `wandb.log` without the **commit** flag to persist the metrics.

```python
wandb.log({'loss': 0.2}, commit=False)
# Somewhere else when I'm ready to report this step:
wandb.log({'accuracy': 0.8})
```

### Logging Objects

Wandb handles a variety of common objects that you might want to log.

#### Logging Tensors

If you pass a numpy array, pytorch tensor or tensorflow tensor to `wandb.log` we automatically convert it as follows:

1. If the object has a size of 1 just log the scalar value
2. If the object has a size of 32 or less, convert the tensor to json
3. If the object has a size greater than 32, log a histogram of the tensor

#### Logging Plots

```python
import matplotlib.pyplot as plt
plt.plot([1, 2, 3, 4])
plt.ylabel('some interesting numbers')
wandb.log({"chart": plt})
```

You can pass a `matplotlib` pyplot or figure object into `wandb.log`. By default we'll convert the plot into a [plotly](https://plot.ly/) plot. If you want to explictly log the plot as an image, you can pass the plot into `wandb.Image`. We also accept directly logging plotly charts.

#### Logging Images

```python
wandb.log({"examples": [wandb.Image(numpy_array_or_pil, caption="Label")]})
```

If a numpy array is supplied we assume it's gray scale if the last dimension is 1, RGB if it's 3, and RGBA if it's 4. If the array contains floats we convert them to ints between 0 and 255. You can specify a [mode](https://pillow.readthedocs.io/en/3.1.x/handbook/concepts.html#concept-modes) manually or just supply a `PIL.Image`. It's recommended to log fewer than 50 images per step.

In the web app, click "Create Visualization" to customize the image gallery.

#### Logging Video

```python
wandb.log({"video": wandb.Video(numpy_array_or_path_to_video, fps=4, format="gif")})
```

If a numpy array is supplied we assume the dimensions are: time,channels,width,height. By default we create a 4 fps gif image \(ffmpeg and the moviepy python library is required when passing numpy objects\). Supported formats are "gif", "mp4", "webm", and "ogg". If you pass a string to `wandb.Video` we assert the file exists and is a supported format before uploading to wandb. Passing a BytesIO object will create a tempfile with the specified format as the extension.

On the W&B runs page, you will see your videos in the Media section.

#### Logging Audio

```python
wandb.log({"examples": [wandb.Audio(numpy_array, caption="Nice", sample_rate=32)]})
```

The maximum number of audio clips that can be logged per step is 100.

#### Logging Text / Tables

```python
# Method 1
data = [["I love my phone", "1", "1"],["My phone sucks", "0", "-1"]]
wandb.log({"examples": wandb.Table(data=data, columns=["Text", "Predicted Label", "True Label"])})

# Method 2
table = wandb.Table(columns=["Text", "Predicted Label", "True Label"])
table.add_data("I love my phone", "1", "1")
table.add_data("My phone sucks", "0", "-1")
wandb.log({"examples": table})
```

By default, the column headers are `["Input", "Output", "Expected"]`. The maximum number of rows is 300.

#### Logging HTML

```python
wandb.log({"custom_file": wandb.Html(open("some.html"))})
wandb.log({"custom_string": wandb.Html('<a href="https://mysite">Link</a>')})
```

Custom html can be logged at any key, this exposes an HTML panel on the run page. By default we inject default styles, you can disable default styles by passing `inject=False`.

```python
wandb.log({"custom_file": wandb.Html(open("some.html"), inject=False)})
```

#### Logging Histograms

```python
wandb.log({"gradients": wandb.Histogram(numpy_array_or_sequence)})
wandb.run.summary.update({"gradients": wandb.Histogram(np_histogram=np.histogram(data))})
```

If a sequence is provided as the first argument, we will bin the histogram automatically. You can also pass what is returned from `np.histogram` to the **np\_histogram** keyword argument to do your own binning. The maximum number of bins supported is 512. You can use the optional **num\_bins** keyword argument when passing a sequence to override the default of 64 bins.

If histograms are in your summary they will appear as sparklines on the individual run pages. If they are in your history, we plot a heatmap of bins over time.

#### Logging 3D Objects

```python
wandb.log({"generated_samples":
           [wandb.Object3D(open("sample.obj")),
            wandb.Object3D(open("sample.gltf")),
            wandb.Object3D(open("sample.glb"))]})
```

Wandb supports logging 3D file types of in three different formats: glTF, glb, obj. The 3D files will be viewable on the run page upon completion of your run.

#### Logging Point Clouds

Point Clouds logging has currently has two modes.  Logging a single set of points representing an object , useful for representing datasets like [ShapeNet\(Example Report\)](https://app.wandb.ai/nbaryd/SparseConvNet-examples_3d_segmentation/reports/Semantic-Segmentation-of-3D-Point-Clouds--VmlldzoxMDk3OA). Along with a new beta release lidar scene renderer. 

Logging a set of points is as simple as passing in a numpy array containing your coordinates and the desired colors for the points

```python
point_cloud = np.array([[0, 0, 0, COLOR...], ...])

wandb.log({"point_cloud": wandb.Object3D(point_cloud)})
```

Three different shapes of numpy arrays are supported for flexible color schemes, supporting common ML us

* `[[x, y, z], ...]` `nx3`
* `[[x, y, z, c], ...]` `nx4` `| c is a category` in the range `[1, 14]` \(Useful for segmentation\)
* `[[x, y, z, r, g, b], ...]` `nx6 | r,g,b` are values in the range `[0,255]`for red, green, and blue color channels.

```python
# Log points and boxes in W&B
wandb.log(
        {
            "point_scene": wandb.Object3D(
                {
                    "type": "lidar/beta",
                    "points": np.array(
                        [
                            [0.4, 1, 1.3], 
                            [1, 1, 1], 
                            [1.2, 1, 1.2]
                        ]
                    ),
                    "boxes": np.array(
                        [
                            {
                                "corners": [
                                    [0,0,0],
                                    [0,1,0],
                                    [0,0,1],
                                    [1,0,0],
                                    [1,1,0],
                                    [0,1,1],
                                    [1,0,1],
                                    [1,1,1]
                                ],
                                "label": "Box",
                                "color": [123,321,111],
                            },
                            {
                                "corners": [
                                    [0,0,0],
                                    [0,2,0],
                                    [0,0,2],
                                    [2,0,0],
                                    [2,2,0],
                                    [0,2,2],
                                    [2,0,2],
                                    [2,2,2]
                                ],
                                "label": "Box-2",
                                "color": [111,321,0],
                            }
                        ]
                    ),
                }
            )
        }
    )


```

* `points`is a numpy array with the same format as the simple point cloud renderer shown above.
* `boxes` is a numpy array of python dictionaries with three attributes:
  * `corners`- a list of eight corners
  * `label`- a string representing the label to be rendered on the box \(Optional\)
  * `color`- rgb values representing the color of the box 
* `type` is a string representing the scene type to render. Currently the only supported value is `lidar/beta`

More scene types will be added in the future. If there is a type of 3d scene you would like to log in Weights & Biases reach out and let us know! We love feedback.

### Summary Metrics

The summary statistics are used to track single metrics per model. If a summary metric is modified, only the updated state is saved. We automatically set summary to the last history row added unless you modify it manually. If you change a summary metric, we only persist the last value it was set to.

```python
wandb.init(config=args)

best_accuracy = 0
for epoch in range(1, args.epochs + 1):
  test_loss, test_accuracy = test()
  if (test_accuracy > best_accuracy):
    wandb.run.summary["best_accuracy"] = test_accuracy
    best_accuracy = test_accuracy
```

You may want to store evaluation metrics in a runs summary after training has completed. Summary can handle numpy arrays, pytorch tensors or tensorflow tensors. When a value is one of these types we persist the entire tensor in a binary file and store high level metrics in the summary object such as min, mean, variance, 95% percentile, etc.

```python
api = wandb.Api()
run = api.run("username/project/run_id")
run.summary["tensor"] = np.random.random(1000)
run.summary.update()
```

### Accessing Logs Directly

The history object is used to track metrics logged by _wandb.log_. You can access a mutable dictionary of metrics via `run.history.row`. The row will be saved and a new row created when `run.history.add` or `wandb.log` is called.

#### Tensorflow Example

```python
wandb.init(config=flags.FLAGS)

# Start tensorflow training
with tf.Session() as sess:
  sess.run(init)

  for step in range(1, run.config.num_steps+1):
      batch_x, batch_y = mnist.train.next_batch(run.config.batch_size)
      # Run optimization op (backprop)
      sess.run(train_op, feed_dict={X: batch_x, Y: batch_y})
      # Calculate batch loss and accuracy
      loss, acc = sess.run([loss_op, accuracy], feed_dict={X: batch_x, Y: batch_y})

      wandb.log({'acc': acc, 'loss':loss}) # log accuracy and loss
```

#### PyTorch Example

```python
# Start pytorch training
wandb.init(config=args)

for epoch in range(1, args.epochs + 1):
  train_loss = train(epoch)
  test_loss, test_accuracy = test()

  torch.save(model.state_dict(), 'model')

  wandb.log({"loss": train_loss, "val_loss": test_loss})
```

## Common Questions

### **Compare images from different epochs**

Each time you log images from a step, we save them to show in the UI. Pin the image panel, and use the **step slider** to look at images from different steps. This makes it easy to compare how a model's output changes over training.

```python
wandb.log({'epoch': epoch, 'val_acc': 0.94})
```

### Batch logging

If you'd like to log certain metrics in every batch and standardize plots, you can log x axis values that you want to plot with your metrics. Then in the custom plots, click edit and select the custom x-axis.

```python
wandb.log({'batch': 1, 'loss': 0.3})
```

### **Log a PNG**

If you're logging images with wandb.log, we'll log a PNG with:

```python
wandb.log({"example": wandb.Image(...)})
```

### **Log a JPEG**

We'll save a JPEG if you call:

```python
wandb.log({"example": [wandb.Image(...) for i in images]})
```

### **Log a Video**

Yes. Click on a run page, and you'll see the file tab on the left sidebar. Click on the file tab to see the files you uploaded in association with your run. If you log videos, you'll be able to find them here. You can also view videos in the media browser. Go to your project workspace, run workspace, or report and click "Add visualization" to add a rich media panel.

### Custom x-axis

By default, we increment the global step every time you call wandb.log. If you'd like, you can log your own monotonically increasing step and then select it as a custom x-axis on your graphs.

For example, if you have training and validation steps you'd like to align, pass us your own step counter: `wandb.log({“acc”:1, “global_step”:1})`. Then in the graphs choose "global\_step" as the x-axis.

### Nothing shows up in the graphs

If you're seeing "No visualization data logged yet" that means that we haven't gotten the first wandb.log call from your script yet. This could be because your run takes a long time to finish a step. If you're logging at the end of each epoch, you could log a few times per epoch to see data stream in more quickly.

### **Duplicate metric names**

If you're logging different types under the same key, we have to split them out in the database. This means you'll see multiple entries of the same metric name in a dropdown in the UI. The types we group by are `number`, `string`, `bool`, `other` \(mostly arrays\), and any wandb type \(`histogram`, `images`, etc\). Please send only one type to each key to avoid this behavior.

### Performance and limits

**Sampling**

The more points you send us, the longer it will take to load your graphs in the UI. If you have more than 1000 points on a line, we sample down to 1000 points on the backend before we send your browser the data. This sampling is nondeterministic, so if you refresh the page you'll see a different set of sampled points.

If you'd like all the original data, you can use our [data API](https://docs.wandb.com/library/api) to pull down unsampled data.

**Guidelines**

We recommend that you try to log less than 10,000 points per metric. If you have more than 500 columns of config and summary metrics, we'll only show 500 in the table. If you log more than 1 million points in a line, it will take us while to load the page.

We store metrics in a case-insensitive fashion, so make sure you don't have two metrics with the same name like "My-Metric" and "my-metric".

