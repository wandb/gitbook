---
description: 'Log, visualize, dynamically query, and understand your data with W&B Tables'
---

# Visualize & Analyze Tables

Use W&B Tables to log and visualize data and model predictions. Interactively explore your data:

* Compare changes precisely across models, epochs, or individual examples
* Understand higher-level patterns in your data
* Capture and communicate your insights with visual samples

## Save your view

Tables you interact with in the run workspace, project workspace, or a report will automatically save their view state. If you apply any Table operations then close your browser, the Table will retain the last viewed configuration when you next navigate to the table.

Tables you interact with in the artifact context will remain stateless.

To save a Table from a workspace in a particular state, export it to a Report. You can do this from the three dot menu in the top right corner of any workspace visualization panel \(three dots → "Share panel" or "Add to report"\).

![Share panel creates a new report, Add to report lets you append to an existing report.](../../.gitbook/assets/screen-shot-2021-04-30-at-11.16.36-am.png)

## Table interactions

### Table operations 

Customize a single Table to answer specific questions, such as what is the precision of this model's predictions and what are the true labels of the mistakes? These operations are 

* **stateless in an artifact context**: any Table logged alongside an artifact version will reset to its default state after you close the browser window 
* **stateful in a workspace or report context**: any changes you make to a Table in a single run workspace, multi-run project workspace, or Report will persist 

![7s confused for 2s are the most frequent error in this view.](../../.gitbook/assets/screen-shot-2021-04-30-at-10.54.30-am.png)

[Try these yourself → ](https://wandb.ai/stacey/mnist-viz/artifacts/predictions/baseline/d888bc05719667811b23/files/predictions.table.json)

#### Sort

Sort all rows in a Table by the value in a given column. Hover over the header, click on the three dot menu, and choose "Sort Asc" or "Sort Desc". 

![See the digits for which the model most confidently guessed &quot;0&quot;.](../../.gitbook/assets/screen-shot-2021-04-30-at-10.27.16-am.png)

#### Filter

Filter all rows by an expression via the Filter button in the top left. The expression editor shows a list of options for each term using autocomplete on column names and logical predicate structure. You can connect multiple logical predicates into one expression using "and" or "or" \(and sometimes parentheses\).

![See only examples which the model gets wrong.](../../.gitbook/assets/screen-shot-2021-04-30-at-10.33.27-am.png)

#### Group

Group all rows by the value in a particular column \(three dot menu in column header → "Group by"\). By default, this will turn other numeric columns into histograms showing the distribution of values for that column across the group. Grouping is helpful for understanding higher-level patterns in your data.

![The truth distribution shows small errors: 8s and 2s are confused for 7s and 9s for 2s. ](../../.gitbook/assets/screen-shot-2021-04-30-at-10.50.45-am.png)

### Changing the columns

#### Add columns

From the three-dot menu on any column, you can insert a new column to the left or right. Edit the cell expression to compute a new column using references to existing columns, mathematical and logical operators, and aggregation functions when a row is grouped \(like average, sum, min/max\). Optionally give the column a new name below the expression editor.

![The closed\_loop\_score column sums the confidence scores for digits with typical loops \(0, 6, 8, 9\).](../../.gitbook/assets/screen-shot-2021-04-30-at-2.46.03-pm.png)

#### Edit columns and display settings

Tables render column data based on the type of the values logged in that column. By clicking on the column name or "Column settings" from the three-dot menu, you can modify

* **the contents** of the column by editing "Cell expression":  select a different field to show, or build a logical predicate expression as described above, including adding a function like count\(\) or avg\(\), etc to apply to the contents.
* **the column type**: convert between a histogram, an array of values, a number, text, etc. W&B will try to guess the type based on the data contents.
* **the pagination**: select how many objects to view at once in a grouped row
* **the display name** in the column header

#### Remove columns

Select "Remove" to delete a column.

## Table comparison

All the operations described above also work in the context of Table comparison. 

![Left: mistakes after 1 training epochs, Right: mistakes after 5 epochs](../../.gitbook/assets/screen-shot-2021-04-30-at-2.51.46-pm.png)

### From the UI

To compare two Tables, start by viewing one Table logged alongside an artifact. Here I've logged a model's predictions on MNIST validation data after each of five epochs \([interactive example →](https://wandb.ai/stacey/mnist-viz/artifacts/predictions/baseline/d888bc05719667811b23/files/predictions.table.json)\)

![Click on &quot;predictions&quot; to view the Table](../../.gitbook/assets/preds_mnist.png)

Next, select a different artifact version for comparison—for example, "v4" to compare to MNIST predictions made by the same model after 5 epochs of training. Hover over the second artifact version in the sidebar and click "Compare" when it appears.

![Preparing to compare model predictions after training for 1 epoch \(v0, shown here\) vs 5 epochs \(v4\)](../../.gitbook/assets/preds_2.png)

#### Merged view

[Live example → ](https://wandb.ai/stacey/mnist-viz/artifacts/predictions/baseline/d888bc05719667811b23/files/predictions.table.json#7dd0cd845c0edb469dec)

Initially you will see both Tables merged together. The first Table selected has index 0 and a blue highlight, and the second Table has index 1 and a yellow highlight. 

![In the merged view, numerical columns will appear as histograms by default](../../.gitbook/assets/merged_view.png)

From the merged view, you can

* **choose the join key**: use the dropdown at the top left to set the column to use as the join key for the two tables. Typically this will be the unique identifier of each row, such as the file name of a specific example in your dataset or an incrementing index on your generated samples. Note that it's currently possible to select _any_ column, which may yield illegible Tables and slow queries.
* **concatenate instead of join**: select "concatenating all tables" in this dropdown to _union all the rows_ from both Tables into one larger Table instead of joining across their columns
* **reference each Table explicitly**: use 0, 1, and \* in the filter expression to explicitly specify a column in one or both Table instances
* **visualize detailed numerical differences as histograms**: compare the values in any cell at a glance

#### Side-by-side view

To view the two Tables side-by-side, change the first dropdown from "WBTableFile" to "Row → TableFile". Here the first Table selected is on the left with a blue highlight, and the second one on the right with a yellow highlight. 

![In the side-by-side view, Table rows are independent of each other.](../../.gitbook/assets/screen-shot-2021-05-03-at-2.31.43-pm.png)

From the side-by-side view, you can

* **compare the Tables at a glance**: apply any operations \(sort, filter, group\) to both Tables in tandem and spot any changes or differences quickly. For example, view the incorrect predictions grouped by guess, the hardest negatives overall, the confidence score distribution by true label, etc.
* **explore two Tables independently**: scroll through and focus on the side/rows of interest

### Compare across time

To analyze model performance over training time, log a Table in an artifact context for each meaningful step of training: at the end of every validation step, after every 50 epochs of training, or any frequency that makes sense for your pipeline. Use the side-by-side view to visualize changes in model predictions.

![For each label, the model makes fewer mistakes after 5 training epochs \(R\) than after 1 \(L\)](../../.gitbook/assets/screen-shot-2021-05-03-at-5.31.10-pm.png)

For a more detailed walkthrough of visualizing predictions across training time, [see this report](https://wandb.ai/stacey/mnist-viz/reports/Visualize-Predictions-over-Time--Vmlldzo1OTQxMTk) and this interactive [notebook example →  ](http://wandb.me/tables-quickstart)

### Compare across model variants

To analyze model performance across different configurations \(hyperparameters, base architectures, etc\), compare two artifact versions logged at the same step for two different models. For example, compare predictions between a `baseline` and a new model variant, `2x_layers_2x_lr`, where the first convolutional layer doubles from 32 to 64, the second from 128 to 256, and the learning rate from 0.001 to 0.002. From [this live example](https://wandb.ai/stacey/mnist-viz/artifacts/predictions/baseline/d888bc05719667811b23/files/predictions.table.json#2bb3b1d40aa777496b5d$2x_layers_2x_lr), use the side-by-side view and filter down to the incorrect predictions after 1 \(left tab\) versus 5 training epochs \(right tab\). 

This is a toy example of model comparison, but it illustrates the ease, flexibility, and depth of the exploratory analysis you can do with Tables—without rerunning any of your code, writing new one-off scripts, generating new charts, etc.

{% tabs %}
{% tab title="1 training epoch" %}
![After 1 epoch, performance is mixed: precision improves for some classes and worsens for others.](../../.gitbook/assets/screen-shot-2021-05-03-at-5.46.46-pm.png)
{% endtab %}

{% tab title="5 training epochs" %}
![After 5 epochs, the &quot;double&quot; variant is catching up to the baseline.](../../.gitbook/assets/screen-shot-2021-05-03-at-5.47.19-pm%20%281%29.png)
{% endtab %}
{% endtabs %}

