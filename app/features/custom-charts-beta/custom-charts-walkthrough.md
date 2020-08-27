---
description: Tutorial of using the custom charts feature in the Weights & Biases UI
---

# Custom Charts Walkthrough

## Custom W&B Plots with Vega

Weights & Biases offers a broad range of built-in visualizations for machine learning research: scatter plots, histograms, parallel coordinates charts, photo/audio/video/3D objects, [and much more](https://docs.wandb.com/library/log#logging-objects). What if I need to log something custom? In this example, I explain how to create a custom visualization type using the W&B query editor and the [Vega visualization grammar](https://vega.github.io/vega/).

### Log your data to W&B

The first step is to log the data you want to visualize to W&B. You can do this via the standard wandb.log\(\) command. A particularly useful format for custom visualizations is wandb.Table\(\), as this lets you log data as a custom 2D array:`my_custom_data = [[x1, y1, z1], [x2, y2, z2]]wandb.log({“custom_data_table” : wandb.Table(data=my_custom_data,                                 columns = ["x", "y", "z"])})`As a simple example, here is [a demo script in Colab](https://colab.research.google.com/drive/1g-gNGokPWM2Qbc8p1Gofud0_5AoZdoSD?usp=sharing) which logs some per-class metrics \(precision, recall, true/false positive rates\) while transfer learning from an InceptionV3 base trained on ImageNet to a small new dataset of nature photos from [iNaturalist 2017](https://github.com/visipedia/inat_comp). For this demo, I will use this data to log two commonly used visualizations: per-class [precision-recall curves](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_recall_curve.html) and [ROC curves](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_curve.html), computed via sklearn. Soon I hope to add much more elaborate plots. For your machine learning use cases and data, your imagination is the only limit, aside from the Vega grammar.  We also recommend logging fewer than 10K data points per plot for optimal performance.

### Start from an existing Vega 2 builtin

Once you’ve logged a run with a wandb.Table, navigate to your project’s workspace \(e.g. mine is here—spoiler alert for the finished product! [https://app.wandb.ai/stacey/custom\_vega\_demo](https://app.wandb.ai/stacey/custom_vega_demo)\) and click on the “+” button in the top right to add a visualization. Select “Vega 2” from the bottom of the left column. You will see a modal that currently looks like this:This scatter plot is close enough to the kind of curves I want to log.  From here, 

![](https://paper-attachments.dropbox.com/s_5FCA7E5A968820ADD0CD5402B4B0F71ED90882B3AC586103C1A96BF845A0EAC7_1597440887681_Screen+Shot+2020-08-14+at+2.34.33+PM.png)

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

Histograms can visualize numerical distributions to help us understand larger datasets. Composite histograms show multiple distributions across the same bins, letting us compare two or more metrics across different models or different classes within our model. For a semantic segmentation model detecting objects in driving scenes, we might compare the effectiveness of optimizing for accuracy versus intersection over union or see how different models perform on detecting cars \(large, common regions in the data\) versus traffic signs \(much smaller, less common regions\).![](https://paper-attachments.dropbox.com/s_5FCA7E5A968820ADD0CD5402B4B0F71ED90882B3AC586103C1A96BF845A0EAC7_1598310765041_Screen+Shot+2020-08-24+at+4.09.52+PM.png)![](https://paper-attachments.dropbox.com/s_5FCA7E5A968820ADD0CD5402B4B0F71ED90882B3AC586103C1A96BF845A0EAC7_1598310765047_Screen+Shot+2020-08-24+at+4.08.17+PM.png)  
In the demo Colab provided, you can compare the confidence scores for two of the ten classes of living things.   
To create your own version of the custom composite histogram panel:

1. Create a new Vega panel in your workspace or report \(by adding a “Vega 2” visualization\). Hit the “Edit” button in the top right  to modify the Vega spec starting from any built-in panel type.
2. Replace that built-in Vega spec with my [MVP code for a composite histogram in Vega](https://gist.github.com/staceysv/9bed36a2c0c2a427365991403611ce21). You can modify the main title, axis titles, input domain, and any other details directly in this Vega spec [using Vega syntax](https://vega.github.io/) \(you could change the colors or even add a third histogram :\)
3. Modify the query in the right hand side to load the correct data from your wandb logs. Add the field “summaryTable” and set the corresponding “tableKey” to “class\_scores” to fetch the wandb.Table logged by your run. This will let you populate the two histogram bin sets \(“red\_bins” and “blue\_bins”\) via the dropdown menus with the columns of the wandb.Table logged as “class\_scores”. For my example, I chose the “animal” class prediction scores for the red bins and “plant” for the blue bins.
4. You can keep making changes to the Vega spec and query until you’re happy with the plot you see in the preview rendering. Once you’re done, click “Save as” in the top and give your custom plot a name so you can reuse it. Then click “Apply from panel library” to finish your plot \[1\].

  
Here’s what my \(very fast\) results look like: training on only 1000 examples for one epoch yields a model that’s very confident that most images are not plants and very uncertain about which images might be animals.\[1\] Note: in the initial release, you may need to re-enter just the query details before you can see the final plot.

![](https://paper-attachments.dropbox.com/s_5FCA7E5A968820ADD0CD5402B4B0F71ED90882B3AC586103C1A96BF845A0EAC7_1598376315319_Screen+Shot+2020-08-25+at+10.24.49+AM.png)

![](https://paper-attachments.dropbox.com/s_5FCA7E5A968820ADD0CD5402B4B0F71ED90882B3AC586103C1A96BF845A0EAC7_1598376160845_Screen+Shot+2020-08-25+at+10.08.11+AM.png)

