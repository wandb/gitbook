---
description: Tutorial of using the custom charts feature in the Weights & Biases UI
---

# Custom Charts Walkthrough

To go beyond the built-in charts in Weights & Biases, use the new **Custom Charts** feature to control the details of exactly what data you're loading in to a panel and how you visualize that data.

**Overview**

1. Log data to W&B
2. Create a query
3. Customize the chart

## 1. Log data to W&B

First, log data in your script. Use [wandb.config](../../../library/config.md) for single points set at the beginning of training, like hyperparameters. Use [wandb.log\(\)](../../../library/log.md) for multiple points over time, and log custom 2D arrays with wandb.Table\(\). We recommend logging up to 10,000 data points per logged key.

```python
# Logging a custom table of data
my_custom_data = [[x1, y1, z1], [x2, y2, z2]]
wandb.log({“custom_data_table”: wandb.Table(data=my_custom_data,
                                columns = ["x", "y", "z"])})
```

[Try a quick example notebook](https://bit.ly/custom-charts-colab) to log the data tables, and in the next step we'll set up custom charts. See what the resulting charts look like in the [live report](https://app.wandb.ai/demo-team/custom-charts/reports/Custom-Charts--VmlldzoyMTk5MDc).

## 2. Create a query

Once you've logged data to visualize, go to your project page and click the **`+`** button to add a new panel, then select **Custom Chart**. You can follow along in [this workspace](https://app.wandb.ai/demo-team/custom-charts).

![A new, blank custom chart ready to be configured](../../../.gitbook/assets/screen-shot-2020-08-28-at-7.41.37-am.png)

### **Add a query**

1. Click `summary` and select `historyTable` to set up a new query pulling data from the run history. 
2. Type in the key where you logged the **wandb.Table\(\)**. In the code snippet above, it was `my_custom_table` . In the [example notebook](https://bit.ly/custom-charts-colab), the keys are `pr_curve` and `roc_curve`.

### Set Vega fields

Now that the query is loading in these columns, they're available as options to select in the Vega fields dropdown menus:

![Pulling in columns from the query results to set Vega fields](../../../.gitbook/assets/screen-shot-2020-08-28-at-8.04.39-am.png)

* **x-axis:** runSets\_historyTable\_r \(recall\)
* **y-axis:** runSets\_historyTable\_p \(precision\)
* **color:** runSets\_historyTable\_c \(class label\)

## 3. Customize the chart

Now that looks pretty good, but I'd like to switch from a scatter plot to a line plot. Click **Edit** to change the Vega spec for this built in chart. Follow along in [this workspace](https://app.wandb.ai/demo-team/custom-charts).

![](https://paper-attachments.dropbox.com/s_5FCA7E5A968820ADD0CD5402B4B0F71ED90882B3AC586103C1A96BF845A0EAC7_1597442115525_Screen+Shot+2020-08-14+at+2.52.24+PM.png)

I updated the Vega spec to customize the visualization:

* add titles for the plot, legend, x-axis, and y-axis \(set “title” for each field\)
* change the value of “mark” from “point” to “line”
* remove the unused “size” field

![](../../../.gitbook/assets/customize-vega-spec-for-pr-curve.png)

To save this as a preset that you can use elsewhere in this project, click **Save as** at the top of the page. Here's what the result looks like, along with an ROC curve:

![](https://paper-attachments.dropbox.com/s_5FCA7E5A968820ADD0CD5402B4B0F71ED90882B3AC586103C1A96BF845A0EAC7_1597442868347_Screen+Shot+2020-08-14+at+3.07.30+PM.png)

Thanks for following along! Message Carey \(c@wandb.com\) with questions and feedback [😊](https://emojipedia.org/smiling-face-with-smiling-eyes/)

## Bonus: Composite Histograms

Histograms can visualize numerical distributions to help us understand larger datasets. Composite histograms show multiple distributions across the same bins, letting us compare two or more metrics across different models or across different classes within our model. For a semantic segmentation model detecting objects in driving scenes, we might compare the effectiveness of optimizing for accuracy versus intersection over union \(IOU\), or we might want to know how well different models detect cars \(large, common regions in the data\) versus traffic signs \(much smaller, less common regions\). In the[ demo Colab](https://bit.ly/custom-charts-colab), you can compare the confidence scores for two of the ten classes of living things.

![](../../../.gitbook/assets/screen-shot-2020-08-28-at-7.19.47-am.png)

To create your own version of the custom composite histogram panel:

1. Create a new Custom Chart panel in your Workspace or Report \(by adding a “Custom Chart” visualization\). Hit the “Edit” button in the top right  to modify the Vega spec starting from any built-in panel type.
2. Replace that built-in Vega spec with my [MVP code for a composite histogram in Vega](https://gist.github.com/staceysv/9bed36a2c0c2a427365991403611ce21). You can modify the main title, axis titles, input domain, and any other details directly in this Vega spec [using Vega syntax](https://vega.github.io/) \(you could change the colors or even add a third histogram :\)
3. Modify the query in the right hand side to load the correct data from your wandb logs. Add the field “summaryTable” and set the corresponding “tableKey” to “class\_scores” to fetch the wandb.Table logged by your run. This will let you populate the two histogram bin sets \(“red\_bins” and “blue\_bins”\) via the dropdown menus with the columns of the wandb.Table logged as “class\_scores”. For my example, I chose the “animal” class prediction scores for the red bins and “plant” for the blue bins.
4. You can keep making changes to the Vega spec and query until you’re happy with the plot you see in the preview rendering. Once you’re done, click “Save as” in the top and give your custom plot a name so you can reuse it. Then click “Apply from panel library” to finish your plot.

Here’s what my results look like from a very brief experiment: training on only 1000 examples for one epoch yields a model that’s very confident that most images are not plants and very uncertain about which images might be animals.

![](https://paper-attachments.dropbox.com/s_5FCA7E5A968820ADD0CD5402B4B0F71ED90882B3AC586103C1A96BF845A0EAC7_1598376315319_Screen+Shot+2020-08-25+at+10.24.49+AM.png)

![](https://paper-attachments.dropbox.com/s_5FCA7E5A968820ADD0CD5402B4B0F71ED90882B3AC586103C1A96BF845A0EAC7_1598376160845_Screen+Shot+2020-08-25+at+10.08.11+AM.png)

