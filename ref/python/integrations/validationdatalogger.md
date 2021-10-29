# ValidationDataLogger



[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/a71719bdde474b8048d942c5b1be20afadaef59a/wandb/sdk/integration_utils/data_logging.py#L18-L209)



ValidationDataLogger helps to develop integrations which log  model predictions.

```python
ValidationDataLogger(
    inputs: Union[Sequence, Dict[str, Sequence]],
    targets: Optional[Union[Sequence, Dict[str, Sequence]]] = None,
    indexes: Optional[List['_TableIndex']] = None,
    validation_row_processor: Optional[Callable] = None,
    prediction_row_processor: Optional[Callable] = None,
    input_col_name: str = "input",
    target_col_name: str = "target",
    table_name: str = "wb_validation_data",
    artifact_type: str = "validation_dataset",
    class_labels: Optional[List[str]] = None,
    infer_missing_processors: bool = (True)
) -> None
```




ValidationDataLogger is intended to be used inside of library integrations
in order to facilitate the process of optionally building a validation dataset
and logging periodic predictions against such validation data using WandB best
practices.

| Args |  |
| :--- | :--- |
|  inputs (Sequence | Dict[str, Sequence]): a list of input vectors or dictionary of lists of input vectors (used if the model has multiple named inputs) targets (Sequence | Dict[str, Sequence], optional): a list of target vectors or dictionary of lists of target vectors (used if the model has multiple named targets/putputs). Defaults to None. `targets` and `indexes` cannot both be None indexes (List[wandb.data_types._TableIndex], optional): An ordered list of wandb.data_types._TableIndex mapping the input items to their source table. This is most commonly retrieved by using indexes = my_data_table.get_index().Defaults to None. `targets` and `indexes` cannot both be None. validation_row_processor (Callable, optional): a function to apply to the validation data, commonly used to visualize the data. The function will receive an ndx (int) and a row (dict). If `inputs` is a list, then row["input"] will be the input data for the row. Else, it will be keyed based on the name of the input slot (corresponding to `inputs`). If `targets` is a list, then row["target"] will be the target data for the row. Else, it will be keyed based on `targets`. For example, if your input data is a single ndarray, but you wish to visualize the data as an Image, then you can provide `lambda ndx, row: {"img": wandb.Image(row["input"])}` as the processor. If None, we will try to guess the appropriate processor. Ignored if log_evaluation is False or val_keys are present. Defaults to None. prediction_row_processor (Callable, optional): same as validation_row_processor, but applied to the model's output. `row["output"]` will contain the results of the model output. Defaults to None. input_col_name (str, optional): the name to use for the input column. Defaults to "input". target_col_name (str, optional): the name to use for the target column. Defaults to "target". table_name (str, optional): the name to use for the validation table. Defaults to "wb_validation_data". artifact_type (str, optional): the artifact type to use for the validation data. Defaults to "validation_dataset". class_labels (List[str], optional): Optional list of lables to use in the inferfed processesors. If the model's `target` or `output` is inferred to be a class, we will attempt to map the class to these labels. Defaults to None. infer_missing_processors (bool, optional): Determines if processors are inferred if they are missing. Defaults to True. |



## Methods

<h3 id="log_predictions"><code>log_predictions</code></h3>

[View source](https://www.github.com/wandb/client/tree/a71719bdde474b8048d942c5b1be20afadaef59a/wandb/sdk/integration_utils/data_logging.py#L162-L209)

```python
log_predictions(
    predictions: Union[Sequence, Dict[str, Sequence]],
    prediction_col_name: str = "output",
    val_ndx_col_name: str = "val_row",
    table_name: str = "validation_predictions",
    commit: bool = (True)
) -> wandb.data_types.Table
```

Logs a set of predictions.


#### Intended usage:



vl.log_predictions(vl.make_predictions(self.model.predict))

| Args |  |
| :--- | :--- |
|  predictions (Sequence | Dict[str, Sequence]): A list of prediction vectors or dictionary of lists of prediction vectors prediction_col_name (str, optional): the name of the prediction column. Defaults to "output". val_ndx_col_name (str, optional): The name of the column linking prediction table to the validation ata table. Defaults to "val_row". table_name (str, optional): name of the prediction table. Defaults to "validation_predictions". commit (bool, optional): determines if commit should be called on the logged data. Defaults to False. |



<h3 id="make_predictions"><code>make_predictions</code></h3>

[View source](https://www.github.com/wandb/client/tree/a71719bdde474b8048d942c5b1be20afadaef59a/wandb/sdk/integration_utils/data_logging.py#L148-L160)

```python
make_predictions(
    predict_fn: Callable
) -> Union[Sequence, Dict[str, Sequence]]
```

Produces predictions by passing `validation_inputs` to `predict_fn`.


| Args |  |
| :--- | :--- |
|  predict_fn (Callable): Any function which can accept `validation_inputs` and produce a list of vectors or dictionary of lists of vectors |



| Returns |  |
| :--- | :--- |
|  (Sequence | Dict[str, Sequence]): The returned value of predict_fn |





