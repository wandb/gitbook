# WandbCallback



[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/v0.10.30/wandb/integration/keras/keras.py#L214-L885)




WandbCallback automatically integrates keras with wandb.

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

```
model.fit(X_train, y_train,  validation_data=(X_test, y_test),
    callbacks=[WandbCallback()])
```


WandbCallback will automatically log history data from any
    metrics collected by keras: loss and anything passed into keras_model.compile() 

WandbCallback will set summary metrics for the run associated with the "best" training
    step, where "best" is defined by the <code>monitor</code> and <code>mode</code> attribues.  This defaults
    to the epoch with the minimum val_loss. WandbCallback will by default save the model 
    associated with the best epoch..

WandbCallback can optionally log gradient and parameter histograms. 

WandbCallback can optionally save training and validation data for wandb to visualize.

<!-- Tabular view -->
<table>
<tr><th>Arguments</th></tr>
<tr>
<td>
monitor (str): name of metric to monitor.  Defaults to val_loss.
mode (str): one of {"auto", "min", "max"}.
"min" - save model when monitor is minimized
"max" - save model when monitor is maximized
"auto" - try to guess when to save the model (default).
</td>
</tr>
<tr>
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
(boolean): if True save model graph to wandb (default: True).
save_weights_only (boolean): if True, then only the model's weights will be
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
(tuple) Same format (X,y) as passed to model.fit.  This is needed 
for calculating gradients - this is mandatory if <code>log_gradients</code> is <code>True</code>.
</td>
</tr><tr>
<td>
<code>validate_data</code>
</td>
<td>
(tuple) Same format (X,y) as passed to model.fit.  A set of data 
for wandb to visualize.  If this is set, every epoch, wandb will
make a small number of predictions and save the results for later visualization.
generator (generator): a generator that returns validation data for wandb to visualize.  This
generator should return tuples (X,y).  Either validate_data or generator should
be set for wandb to visualize specific data examples.
validation_steps (int): if <code>validation_data</code> is a generator, how many
steps to run the generator for the full validation set.
labels (list): If you are visualizing your data with wandb this list of labels 
will convert numeric output to understandable string if you are building a
multiclass classifier.  If you are making a binary classifier you can pass in
a list of two labels ["label for false", "label for true"].  If validate_data
and generator are both false, this won't do anything.
predictions (int): the number of predictions to make for visualization each epoch, max 
is 100.
input_type (string): type of the model input to help visualization. can be one of:
("image", "images", "segmentation_mask").
output_type (string): type of the model output to help visualziation. can be one of:
("image", "images", "segmentation_mask").  
log_evaluation (boolean): if True, save a Table containing validation data and the 
model's preditions at each epoch. See <code>validation_indexes</code>, 
<code>validation_row_processor</code>, and <code>output_row_processor</code> for additional details.
class_colors ([float, float, float]): if the input or output is a segmentation mask, 
an array containing an rgb tuple (range 0-1) for each class.
log_batch_frequency (integer): if None, callback will log every epoch.
If set to integer, callback will log training metrics every log_batch_frequency 
batches.
log_best_prefix (string): if None, no extra summary metrics will be saved.
If set to a string, the monitored metric and epoch will be prepended with this value
and stored as summary metrics.
validation_indexes ([wandb.data_types._TableLinkMixin]): an ordered list of index keys to associate 
with each validation example.  If log_evaluation is True and <code>validation_indexes</code> is provided,
then a Table of validation data will not be created and instead each prediction will
be associated with the row represented by the TableLinkMixin. The most common way to obtain
such keys are is use Table.get_index() which will return a list of row keys.
validation_row_processor (Callable): a function to apply to the validation data, commonly used to visualize the data. 
The function will receive an ndx (int) and a row (dict). If your model has a single input,
then row["input"] will be the input data for the row. Else, it will be keyed based on the name of the
input slot. If your fit function takes a single target, then row["target"] will be the target data for the row. Else,
it will be keyed based on the name of the output slots. For example, if your input data is a single ndarray,
but you wish to visualize the data as an Image, then you can provide `lambda ndx, row: {"img": wandb.Image(row["input"])}`
as the processor. Ignored if log_evaluation is False or <code>validation_indexes</code> are present.
output_row_processor (Callable): same as validation_row_processor, but applied to the model's output. `row["output"]` will contain
the results of the model output.
infer_missing_processors (bool): Determines if validation_row_processor and output_row_processor 
should be inferred if missing. Defaults to True. If <code>labels</code> are provided, we will attempt to infer classification-type
processors where appropriate.
</td>
</tr>
</table>



## Methods

<h3 id="on_batch_begin"><code>on_batch_begin</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.30/wandb/integration/keras/keras.py#L511-L512">View source</a>

<pre><code>on_batch_begin(
    batch, logs=None
)</code></pre>

A backwards compatibility alias for <code>on_train_batch_begin</code>.


<h3 id="on_batch_end"><code>on_batch_end</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.30/wandb/integration/keras/keras.py#L515-L522">View source</a>

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

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.30/wandb/integration/keras/keras.py#L454-L508">View source</a>

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

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.30/wandb/integration/keras/keras.py#L613-L614">View source</a>

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

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.30/wandb/integration/keras/keras.py#L616-L617">View source</a>

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

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.30/wandb/integration/keras/keras.py#L607-L608">View source</a>

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

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.30/wandb/integration/keras/keras.py#L610-L611">View source</a>

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

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.30/wandb/integration/keras/keras.py#L601-L602">View source</a>

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

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.30/wandb/integration/keras/keras.py#L604-L605">View source</a>

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

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.30/wandb/integration/keras/keras.py#L595-L596">View source</a>

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

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.30/wandb/integration/keras/keras.py#L598-L599">View source</a>

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

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.30/wandb/integration/keras/keras.py#L524-L525">View source</a>

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

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.30/wandb/integration/keras/keras.py#L527-L534">View source</a>

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

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.30/wandb/integration/keras/keras.py#L548-L590">View source</a>

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

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.30/wandb/integration/keras/keras.py#L592-L593">View source</a>

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

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.30/wandb/integration/keras/keras.py#L443-L452">View source</a>

<pre><code>set_model(
    model
)</code></pre>




<h3 id="set_params"><code>set_params</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.30/wandb/integration/keras/keras.py#L440-L441">View source</a>

<pre><code>set_params(
    params
)</code></pre>






