# WandbCallback



[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/v0.10.31/wandb/integration/keras/keras.py#L214-L888)




<code>WandbCallback</code> automatically integrates keras with wandb.

<pre><code>WandbCallback(
    monitor=&#x27;val_loss&#x27;, verbose=0, mode=&#x27;auto&#x27;,
    save_weights_only=(False), log_weights=(False), log_gradients=(False),
    save_model=(True), training_data=None, validation_data=None, labels=[],
    data_type=None, predictions=36, generator=None, input_type=None,
    output_type=None, log_evaluation=(False), validation_steps=None,
    class_colors=None, log_batch_frequency=None, log_best_prefix=&#x27;best_&#x27;,
    save_graph=(True), validation_indexes=None, validation_row_processor=None,
    prediction_row_processor=None, infer_missing_processors=(True)
)</code></pre>





#### Example:

```python
model.fit(X_train,
    y_train,
    validation_data=(X_test, y_test),
    callbacks=[WandbCallback()]
)
```


<code>WandbCallback</code> will automatically log history data from any
metrics collected by keras: loss and anything passed into <code>keras_model.compile()</code>. 

<code>WandbCallback</code> will set summary metrics for the run associated with the "best" training
step, where "best" is defined by the <code>monitor</code> and <code>mode</code> attribues.  This defaults
to the epoch with the minimum <code>val_loss</code>. <code>WandbCallback</code> will by default save the model 
associated with the best <code>epoch</code>.

<code>WandbCallback</code> can optionally log gradient and parameter histograms. 

<code>WandbCallback</code> can optionally save training and validation data for wandb to visualize.

<!-- Tabular view -->
<table>
<tr><th>Arguments</th></tr>

<tr>
<td>
<code>monitor</code>
</td>
<td>
(str) name of metric to monitor.  Defaults to <code>val_loss</code>.
</td>
</tr><tr>
<td>
<code>mode</code>
</td>
<td>
(str) one of {<code>auto</code>, <code>min</code>, <code>max</code>}.
<code>min</code> - save model when monitor is minimized
<code>max</code> - save model when monitor is maximized
<code>auto</code> - try to guess when to save the model (default).
</td>
</tr><tr>
<td>
<code>save_model</code>
</td>
<td>
True - save a model when monitor beats all previous epochs
False - don't save models
</td>
</tr><tr>
<td>
<code>save_graph</code>
</td>
<td>
(boolean) if True save model graph to wandb (default to True).
</td>
</tr><tr>
<td>
<code>save_weights_only</code>
</td>
<td>
(boolean) if True, then only the model's weights will be
saved (<code>model.save_weights(filepath)</code>), else the full model
is saved (<code>model.save(filepath)</code>).
</td>
</tr><tr>
<td>
<code>log_weights</code>
</td>
<td>
(boolean) if True save histograms of the model's layer's weights.
</td>
</tr><tr>
<td>
<code>log_gradients</code>
</td>
<td>
(boolean) if True log histograms of the training gradients
</td>
</tr><tr>
<td>
<code>training_data</code>
</td>
<td>
(tuple) Same format <code>(X,y)</code> as passed to <code>model.fit</code>.  This is needed 
for calculating gradients - this is mandatory if <code>log_gradients</code> is <code>True</code>.
</td>
</tr><tr>
<td>
<code>validate_data</code>
</td>
<td>
(tuple) Same format <code>(X,y)</code> as passed to <code>model.fit</code>.  A set of data 
for wandb to visualize.  If this is set, every epoch, wandb will
make a small number of predictions and save the results for later visualization.
</td>
</tr><tr>
<td>
<code>generator</code>
</td>
<td>
(generator) a generator that returns validation data for wandb to visualize.  This
generator should return tuples <code>(X,y)</code>.  Either <code>validate_data</code> or generator should
be set for wandb to visualize specific data examples.
</td>
</tr><tr>
<td>
<code>validation_steps</code>
</td>
<td>
(int) if <code>validation_data</code> is a generator, how many
steps to run the generator for the full validation set.
</td>
</tr><tr>
<td>
<code>labels</code>
</td>
<td>
(list) If you are visualizing your data with wandb this list of labels 
will convert numeric output to understandable string if you are building a
multiclass classifier.  If you are making a binary classifier you can pass in
a list of two labels ["label for false", "label for true"].  If <code>validate_data</code>
and generator are both false, this won't do anything.
</td>
</tr><tr>
<td>
<code>predictions</code>
</td>
<td>
(int) the number of predictions to make for visualization each epoch, max 
is 100.
</td>
</tr><tr>
<td>
<code>input_type</code>
</td>
<td>
(string) type of the model input to help visualization. can be one of:
(<code>image</code>, <code>images</code>, <code>segmentation_mask</code>).
</td>
</tr><tr>
<td>
<code>output_type</code>
</td>
<td>
(string) type of the model output to help visualziation. can be one of:
(<code>image</code>, <code>images</code>, <code>segmentation_mask</code>).
</td>
</tr><tr>
<td>
<code>log_evaluation</code>
</td>
<td>
(boolean) if True, save a Table containing validation data and the 
model's preditions at each epoch. See <code>validation_indexes</code>, 
<code>validation_row_processor</code>, and <code>output_row_processor</code> for additional details.
</td>
</tr><tr>
<td>
<code>class_colors</code>
</td>
<td>
([float, float, float]) if the input or output is a segmentation mask, 
an array containing an rgb tuple (range 0-1) for each class.
</td>
</tr><tr>
<td>
<code>log_batch_frequency</code>
</td>
<td>
(integer) if None, callback will log every epoch.
If set to integer, callback will log training metrics every <code>log_batch_frequency</code> 
batches.
</td>
</tr><tr>
<td>
<code>log_best_prefix</code>
</td>
<td>
(string) if None, no extra summary metrics will be saved.
If set to a string, the monitored metric and epoch will be prepended with this value
and stored as summary metrics.
</td>
</tr><tr>
<td>
<code>validation_indexes</code>
</td>
<td>
([wandb.data_types._TableLinkMixin]) an ordered list of index keys to associate 
with each validation example.  If log_evaluation is True and <code>validation_indexes</code> is provided,
then a Table of validation data will not be created and instead each prediction will
be associated with the row represented by the <code>TableLinkMixin</code>. The most common way to obtain
such keys are is use <code>Table.get_index()</code> which will return a list of row keys.
</td>
</tr><tr>
<td>
<code>validation_row_processor</code>
</td>
<td>
(Callable) a function to apply to the validation data, commonly used to visualize the data. 
The function will receive an <code>ndx</code> (int) and a <code>row</code> (dict). If your model has a single input,
then `row["input"]` will be the input data for the row. Else, it will be keyed based on the name of the
input slot. If your fit function takes a single target, then `row["target"]` will be the target data for the row. Else,
it will be keyed based on the name of the output slots. For example, if your input data is a single ndarray,
but you wish to visualize the data as an Image, then you can provide `lambda ndx, row: {"img": wandb.Image(row["input"])}`
as the processor. Ignored if log_evaluation is False or <code>validation_indexes</code> are present.
</td>
</tr><tr>
<td>
<code>output_row_processor</code>
</td>
<td>
(Callable) same as <code>validation_row_processor</code>, but applied to the model's output. `row["output"]` will contain
the results of the model output.
</td>
</tr><tr>
<td>
<code>infer_missing_processors</code>
</td>
<td>
(bool) Determines if <code>validation_row_processor</code> and <code>output_row_processor</code> 
should be inferred if missing. Defaults to True. If <code>labels</code> are provided, we will attempt to infer classification-type
processors where appropriate.
</td>
</tr>
</table>



## Methods

<h3 id="set_model"><code>set_model</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.31/wandb/integration/keras/keras.py#L446-L455">View source</a>

<pre><code>set_model(
    model
)</code></pre>




<h3 id="set_params"><code>set_params</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.31/wandb/integration/keras/keras.py#L443-L444">View source</a>

<pre><code>set_params(
    params
)</code></pre>






