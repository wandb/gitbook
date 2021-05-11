# WandbCallback



[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/94c226afc4925535e6301c9bc9b9ee36061d99d4/wandb/integration/keras/keras.py#L214-L888)




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

<h3 id="on_batch_begin"><code>on_batch_begin</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/94c226afc4925535e6301c9bc9b9ee36061d99d4/wandb/integration/keras/keras.py#L514-L515">View source</a>

<pre><code>on_batch_begin(
    batch, logs=None
)</code></pre>

A backwards compatibility alias for <code>on_train_batch_begin</code>.


<h3 id="on_batch_end"><code>on_batch_end</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/94c226afc4925535e6301c9bc9b9ee36061d99d4/wandb/integration/keras/keras.py#L518-L525">View source</a>

<pre><code>on_batch_end(
    batch, logs=None
)</code></pre>

A backwards compatibility alias for <code>on_train_batch_end</code>.


<h3 id="on_epoch_begin"><code>on_epoch_begin</code></h3>

<pre><code>on_epoch_begin(
    epoch, logs=None
)</code></pre>

Called at the start of an epoch.

Subclasses should override for any actions to run. This function should only
be called during TRAIN mode.

<!-- Tabular view -->
<table>
<tr><th>Args</th></tr>

<tr>
<td>
<code>epoch</code>
</td>
<td>
Integer, index of epoch.
</td>
</tr><tr>
<td>
<code>logs</code>
</td>
<td>
Dict. Currently no data is passed to this argument for this method
but that may change in the future.
</td>
</tr>
</table>



<h3 id="on_epoch_end"><code>on_epoch_end</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/94c226afc4925535e6301c9bc9b9ee36061d99d4/wandb/integration/keras/keras.py#L457-L511">View source</a>

<pre><code>on_epoch_end(
    epoch, logs={}
)</code></pre>

Called at the end of an epoch.

Subclasses should override for any actions to run. This function should only
be called during TRAIN mode.

<!-- Tabular view -->
<table>
<tr><th>Args</th></tr>

<tr>
<td>
<code>epoch</code>
</td>
<td>
Integer, index of epoch.
</td>
</tr><tr>
<td>
<code>logs</code>
</td>
<td>
Dict, metric results for this training epoch, and for the
validation epoch if validation is performed. Validation result keys
are prefixed with <code>val_</code>. For training epoch, the values of the
<code>Model</code>'s metrics are returned. Example : `{'loss': 0.2, 'accuracy':
0.7}`.
</td>
</tr>
</table>



<h3 id="on_predict_batch_begin"><code>on_predict_batch_begin</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/94c226afc4925535e6301c9bc9b9ee36061d99d4/wandb/integration/keras/keras.py#L616-L617">View source</a>

<pre><code>on_predict_batch_begin(
    batch, logs=None
)</code></pre>

Called at the beginning of a batch in <code>predict</code> methods.

Subclasses should override for any actions to run.

Note that if the <code>steps_per_execution</code> argument to <code>compile</code> in
<code>tf.keras.Model</code> is set to <code>N</code>, this method will only be called every <code>N</code>
batches.

<!-- Tabular view -->
<table>
<tr><th>Args</th></tr>

<tr>
<td>
<code>batch</code>
</td>
<td>
Integer, index of batch within the current epoch.
</td>
</tr><tr>
<td>
<code>logs</code>
</td>
<td>
Dict, contains the return value of <code>model.predict_step</code>,
it typically returns a dict with a key 'outputs' containing
the model's outputs.
</td>
</tr>
</table>



<h3 id="on_predict_batch_end"><code>on_predict_batch_end</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/94c226afc4925535e6301c9bc9b9ee36061d99d4/wandb/integration/keras/keras.py#L619-L620">View source</a>

<pre><code>on_predict_batch_end(
    batch, logs=None
)</code></pre>

Called at the end of a batch in <code>predict</code> methods.

Subclasses should override for any actions to run.

Note that if the <code>steps_per_execution</code> argument to <code>compile</code> in
<code>tf.keras.Model</code> is set to <code>N</code>, this method will only be called every <code>N</code>
batches.

<!-- Tabular view -->
<table>
<tr><th>Args</th></tr>

<tr>
<td>
<code>batch</code>
</td>
<td>
Integer, index of batch within the current epoch.
</td>
</tr><tr>
<td>
<code>logs</code>
</td>
<td>
Dict. Aggregated metric results up until this batch.
</td>
</tr>
</table>



<h3 id="on_predict_begin"><code>on_predict_begin</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/94c226afc4925535e6301c9bc9b9ee36061d99d4/wandb/integration/keras/keras.py#L610-L611">View source</a>

<pre><code>on_predict_begin(
    logs=None
)</code></pre>

Called at the beginning of prediction.

Subclasses should override for any actions to run.

<!-- Tabular view -->
<table>
<tr><th>Args</th></tr>

<tr>
<td>
<code>logs</code>
</td>
<td>
Dict. Currently no data is passed to this argument for this method
but that may change in the future.
</td>
</tr>
</table>



<h3 id="on_predict_end"><code>on_predict_end</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/94c226afc4925535e6301c9bc9b9ee36061d99d4/wandb/integration/keras/keras.py#L613-L614">View source</a>

<pre><code>on_predict_end(
    logs=None
)</code></pre>

Called at the end of prediction.

Subclasses should override for any actions to run.

<!-- Tabular view -->
<table>
<tr><th>Args</th></tr>

<tr>
<td>
<code>logs</code>
</td>
<td>
Dict. Currently no data is passed to this argument for this method
but that may change in the future.
</td>
</tr>
</table>



<h3 id="on_test_batch_begin"><code>on_test_batch_begin</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/94c226afc4925535e6301c9bc9b9ee36061d99d4/wandb/integration/keras/keras.py#L604-L605">View source</a>

<pre><code>on_test_batch_begin(
    batch, logs=None
)</code></pre>

Called at the beginning of a batch in <code>evaluate</code> methods.

Also called at the beginning of a validation batch in the <code>fit</code>
methods, if validation data is provided.

Subclasses should override for any actions to run.

Note that if the <code>steps_per_execution</code> argument to <code>compile</code> in
<code>tf.keras.Model</code> is set to <code>N</code>, this method will only be called every <code>N</code>
batches.

<!-- Tabular view -->
<table>
<tr><th>Args</th></tr>

<tr>
<td>
<code>batch</code>
</td>
<td>
Integer, index of batch within the current epoch.
</td>
</tr><tr>
<td>
<code>logs</code>
</td>
<td>
Dict, contains the return value of <code>model.test_step</code>. Typically,
the values of the <code>Model</code>'s metrics are returned.  Example:
`{'loss': 0.2, 'accuracy': 0.7}`.
</td>
</tr>
</table>



<h3 id="on_test_batch_end"><code>on_test_batch_end</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/94c226afc4925535e6301c9bc9b9ee36061d99d4/wandb/integration/keras/keras.py#L607-L608">View source</a>

<pre><code>on_test_batch_end(
    batch, logs=None
)</code></pre>

Called at the end of a batch in <code>evaluate</code> methods.

Also called at the end of a validation batch in the <code>fit</code>
methods, if validation data is provided.

Subclasses should override for any actions to run.

Note that if the <code>steps_per_execution</code> argument to <code>compile</code> in
<code>tf.keras.Model</code> is set to <code>N</code>, this method will only be called every <code>N</code>
batches.

<!-- Tabular view -->
<table>
<tr><th>Args</th></tr>

<tr>
<td>
<code>batch</code>
</td>
<td>
Integer, index of batch within the current epoch.
</td>
</tr><tr>
<td>
<code>logs</code>
</td>
<td>
Dict. Aggregated metric results up until this batch.
</td>
</tr>
</table>



<h3 id="on_test_begin"><code>on_test_begin</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/94c226afc4925535e6301c9bc9b9ee36061d99d4/wandb/integration/keras/keras.py#L598-L599">View source</a>

<pre><code>on_test_begin(
    logs=None
)</code></pre>

Called at the beginning of evaluation or validation.

Subclasses should override for any actions to run.

<!-- Tabular view -->
<table>
<tr><th>Args</th></tr>

<tr>
<td>
<code>logs</code>
</td>
<td>
Dict. Currently no data is passed to this argument for this method
but that may change in the future.
</td>
</tr>
</table>



<h3 id="on_test_end"><code>on_test_end</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/94c226afc4925535e6301c9bc9b9ee36061d99d4/wandb/integration/keras/keras.py#L601-L602">View source</a>

<pre><code>on_test_end(
    logs=None
)</code></pre>

Called at the end of evaluation or validation.

Subclasses should override for any actions to run.

<!-- Tabular view -->
<table>
<tr><th>Args</th></tr>

<tr>
<td>
<code>logs</code>
</td>
<td>
Dict. Currently the output of the last call to
<code>on_test_batch_end()</code> is passed to this argument for this method
but that may change in the future.
</td>
</tr>
</table>



<h3 id="on_train_batch_begin"><code>on_train_batch_begin</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/94c226afc4925535e6301c9bc9b9ee36061d99d4/wandb/integration/keras/keras.py#L527-L528">View source</a>

<pre><code>on_train_batch_begin(
    batch, logs=None
)</code></pre>

Called at the beginning of a training batch in <code>fit</code> methods.

Subclasses should override for any actions to run.

Note that if the <code>steps_per_execution</code> argument to <code>compile</code> in
<code>tf.keras.Model</code> is set to <code>N</code>, this method will only be called every <code>N</code>
batches.

<!-- Tabular view -->
<table>
<tr><th>Args</th></tr>

<tr>
<td>
<code>batch</code>
</td>
<td>
Integer, index of batch within the current epoch.
</td>
</tr><tr>
<td>
<code>logs</code>
</td>
<td>
Dict, contains the return value of <code>model.train_step</code>. Typically,
the values of the <code>Model</code>'s metrics are returned.  Example:
`{'loss': 0.2, 'accuracy': 0.7}`.
</td>
</tr>
</table>



<h3 id="on_train_batch_end"><code>on_train_batch_end</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/94c226afc4925535e6301c9bc9b9ee36061d99d4/wandb/integration/keras/keras.py#L530-L537">View source</a>

<pre><code>on_train_batch_end(
    batch, logs=None
)</code></pre>

Called at the end of a training batch in <code>fit</code> methods.

Subclasses should override for any actions to run.

Note that if the <code>steps_per_execution</code> argument to <code>compile</code> in
<code>tf.keras.Model</code> is set to <code>N</code>, this method will only be called every <code>N</code>
batches.

<!-- Tabular view -->
<table>
<tr><th>Args</th></tr>

<tr>
<td>
<code>batch</code>
</td>
<td>
Integer, index of batch within the current epoch.
</td>
</tr><tr>
<td>
<code>logs</code>
</td>
<td>
Dict. Aggregated metric results up until this batch.
</td>
</tr>
</table>



<h3 id="on_train_begin"><code>on_train_begin</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/94c226afc4925535e6301c9bc9b9ee36061d99d4/wandb/integration/keras/keras.py#L551-L593">View source</a>

<pre><code>on_train_begin(
    logs=None
)</code></pre>

Called at the beginning of training.

Subclasses should override for any actions to run.

<!-- Tabular view -->
<table>
<tr><th>Args</th></tr>

<tr>
<td>
<code>logs</code>
</td>
<td>
Dict. Currently no data is passed to this argument for this method
but that may change in the future.
</td>
</tr>
</table>



<h3 id="on_train_end"><code>on_train_end</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/94c226afc4925535e6301c9bc9b9ee36061d99d4/wandb/integration/keras/keras.py#L595-L596">View source</a>

<pre><code>on_train_end(
    logs=None
)</code></pre>

Called at the end of training.

Subclasses should override for any actions to run.

<!-- Tabular view -->
<table>
<tr><th>Args</th></tr>

<tr>
<td>
<code>logs</code>
</td>
<td>
Dict. Currently the output of the last call to <code>on_epoch_end()</code>
is passed to this argument for this method but that may change in
the future.
</td>
</tr>
</table>



<h3 id="set_model"><code>set_model</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/94c226afc4925535e6301c9bc9b9ee36061d99d4/wandb/integration/keras/keras.py#L446-L455">View source</a>

<pre><code>set_model(
    model
)</code></pre>




<h3 id="set_params"><code>set_params</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/94c226afc4925535e6301c9bc9b9ee36061d99d4/wandb/integration/keras/keras.py#L443-L444">View source</a>

<pre><code>set_params(
    params
)</code></pre>






