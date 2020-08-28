---
description: Tutorial of using the custom charts feature in the Weights & Biases UI
---

# Custom Charts Walkthrough

Going beyond the built-in charts in Weights & Biases, use the new **Custom Charts** feature to control the details of exactly what data you're loading in to a panel and how you visualize that data.

**Overview**

1. Log data to W&B
2. Customize a Vega chart
3. Create a new Vega chart
4. Try a composite histogram

## 1. Log data to W&B

First, log data in your script. Use [wandb.config](../../../library/config.md) for single points set at the beginning of training, like hyperparameters. Use [wandb.log\(\)](../../../library/log.md) for multiple points over time, and log custom 2D arrays with wandb.Table\(\). We recommend logging up to 10,000 data points per logged key.

```python
# Logging a custom table of data
my_custom_data = [[x1, y1, z1], [x2, y2, z2]]
wandb.log({“custom_data_table”: wandb.Table(data=my_custom_data,
                                columns = ["x", "y", "z"])})
```

[Try a quick example notebook](https://bit.ly/custom-charts-colab) to log the data tables, and in the next step we'll set up custom charts.

## 2. Customize a Vega chart

Once you've logged data to visualize, go to your project page and click the **`+`** button to add a new panel, then select **Custom Chart**. 

![A new custom chart](https://paper-attachments.dropbox.com/s_5FCA7E5A968820ADD0CD5402B4B0F71ED90882B3AC586103C1A96BF845A0EAC7_1597440887681_Screen+Shot+2020-08-14+at+2.34.33+PM.png)

* click on the “+” below the name dropdown to add a “createdAt” query filter placeholder
* from the dropdown on “createdAt”, select “historyTable”
* enter the key under which you logged the wandb.Table in the previous step as the tableKey: from my code snippet above, this would be “my\_custom\_table”. In my colab example, I use “pr\_curve” and “roc\_curve” as the table keys.

  
This lets me access the columns of my logged wandb.Table as Vega fields.To create a PR curve, I set the following fields using this dropdown:

![](https://paper-attachments.dropbox.com/s_5FCA7E5A968820ADD0CD5402B4B0F71ED90882B3AC586103C1A96BF845A0EAC7_1597441569984_Screen+Shot+2020-08-14+at+2.45.34+PM.png)

* x-axis = projects\_runs\_historyTable\_r \(recall\)
* y-axis = projects\_runs\_historyTable\_p \(precision\)
* color = projects\_runs\_historyTable\_c \(class label\)

Note: do not set the “size” field as that will misconfigure the plot.  
to yield something like this:

![](https://paper-attachments.dropbox.com/s_5FCA7E5A968820ADD0CD5402B4B0F71ED90882B3AC586103C1A96BF845A0EAC7_1597441855957_Screen+Shot+2020-08-14+at+2.50.41+PM.png)

### Creating your own visualization type

This already looks pretty good, but I want to polish the details and easily reuse this chart type in the future. Hover over the top right corner of the chart and click on the pencil to edit it. Next, hit the edit button to the right of the visualization type dropdown.You can now edit the JSON in the Vega spec directly. To make my PR curve prettier, I make the following changes:

![](https://paper-attachments.dropbox.com/s_5FCA7E5A968820ADD0CD5402B4B0F71ED90882B3AC586103C1A96BF845A0EAC7_1597442115525_Screen+Shot+2020-08-14+at+2.52.24+PM.png)

* add custom plot, legend, x-axis, and y-axis titles \(set “title” for each field\)
* change the value of “mark” from “point” to “line”
* remove the unused “size” field

You can of course keep making your own changes, including to the query itself.Here is my final Vega spec:  
 Now I can hit “Save as…” to make myself a new custom plot type, say “pr\_curve\_per\_class”, which I can reuse throughout this project \(and eventually share beyond the project\). Here it is in all its glory, along with an ROC curve created with the same process:  I hope this helps you create your own custom plots! We’re releasing this as a beta and plan to keep smoothing out the details. We can’t wait to see the collective data-visualizing art of our community. 

![](https://paper-attachments.dropbox.com/s_5FCA7E5A968820ADD0CD5402B4B0F71ED90882B3AC586103C1A96BF845A0EAC7_1597442868347_Screen+Shot+2020-08-14+at+3.07.30+PM.png)

![](https://paper-attachments.dropbox.com/s_5FCA7E5A968820ADD0CD5402B4B0F71ED90882B3AC586103C1A96BF845A0EAC7_1597442424763_Screen+Shot+2020-08-14+at+2.56.38+PM.png)

### P.S. Bonus: Composite histograms

Histograms can visualize numerical distributions to help us understand larger datasets. Composite histograms show multiple distributions across the same bins, letting us compare two or more metrics across different models or different classes within our model. For a semantic segmentation model detecting objects in driving scenes, we might compare the effectiveness of optimizing for accuracy versus intersection over union or see how different models perform on detecting cars \(large, common regions in the data\) versus traffic signs \(much smaller, less common regions\).  
In the demo Colab provided, you can compare the confidence scores for two of the ten classes of living things.   
To create your own version of the custom composite histogram panel:

![](../../../.gitbook/assets/screen-shot-2020-08-28-at-7.19.47-am.png)

1. Create a new Vega panel in your workspace or report \(by adding a “Vega 2” visualization\). Hit the “Edit” button in the top right  to modify the Vega spec starting from any built-in panel type.
2. Replace that built-in Vega spec with my [MVP code for a composite histogram in Vega](https://gist.github.com/staceysv/9bed36a2c0c2a427365991403611ce21). You can modify the main title, axis titles, input domain, and any other details directly in this Vega spec [using Vega syntax](https://vega.github.io/) \(you could change the colors or even add a third histogram :\)
3. Modify the query in the right hand side to load the correct data from your wandb logs. Add the field “summaryTable” and set the corresponding “tableKey” to “class\_scores” to fetch the wandb.Table logged by your run. This will let you populate the two histogram bin sets \(“red\_bins” and “blue\_bins”\) via the dropdown menus with the columns of the wandb.Table logged as “class\_scores”. For my example, I chose the “animal” class prediction scores for the red bins and “plant” for the blue bins.
4. You can keep making changes to the Vega spec and query until you’re happy with the plot you see in the preview rendering. Once you’re done, click “Save as” in the top and give your custom plot a name so you can reuse it. Then click “Apply from panel library” to finish your plot \[1\].

  
Here’s what my \(very fast\) results look like: training on only 1000 examples for one epoch yields a model that’s very confident that most images are not plants and very uncertain about which images might be animals.\[1\] Note: in the initial release, you may need to re-enter just the query details before you can see the final plot.

![](https://paper-attachments.dropbox.com/s_5FCA7E5A968820ADD0CD5402B4B0F71ED90882B3AC586103C1A96BF845A0EAC7_1598376315319_Screen+Shot+2020-08-25+at+10.24.49+AM.png)

![](https://paper-attachments.dropbox.com/s_5FCA7E5A968820ADD0CD5402B4B0F71ED90882B3AC586103C1A96BF845A0EAC7_1598376160845_Screen+Shot+2020-08-25+at+10.08.11+AM.png)

