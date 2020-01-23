---
description: 'Take notes about your research with markdown, visualizations, and images'
---

# Report Page

Use reports to describe your process developing models. Report sections each have their own run sets, so you can show comparisons of different runs from different projects in the same page. Panels in reports include visualizations, markdown, and images. Here's an [example report](https://app.wandb.ai/stacey/estuary/reports?view=stacey%2FDistributed%20Training) on distributed training.

![](https://downloads.intercomcdn.com/i/o/142935299/e49a7b19a392df6dd0ab3501/image.png)

## Add markdown

Click **Add a visualization** and select **Markdown** to add a text panel to a report.

![](../../.gitbook/assets/bug-markdown.gif)

## Add visualizations

Click the "Add a visualization" button to add a new graph.

Select what kind of visualization you'd like to use. We'll start with a line plot.

![](https://downloads.intercomcdn.com/i/o/142935595/d1422f30460a39b8b4868885/image.png)

There are plenty of settings to customize your line plot. Select a metric to compare across runs.

![](https://downloads.intercomcdn.com/i/o/142935671/6a21c9df8a95ea9bd033e80d/image.png)

## Sections

If you'd like to compare a different set of runs, create a new section. Each section's graphs are controlled by the table in that section.

![](https://downloads.intercomcdn.com/i/o/142935919/23983a0d2d1190260e48fb2c/image.png)

Duplicate a section to copy the settings from your first section.

![](../../.gitbook/assets/howto-duplicate-section%20%281%29.gif)

## Static and dynamic run sets

* **Dynamic run sets**: If you start from "Visualize all" and filter or deselect runs to visualize, the run set will automatically update to show any new runs that match the filters.
* **Static run sets**: If you start from "Visualize none" and select the runs you want to include in your run set, you will only ever get those runs in the run set. No new runs will be added.

Here's a gif of me clicking "Visualize none" and then selecting the runs I want to include in the run set. This run set will not update as I add more runs to my project.

![](../../.gitbook/assets/no-auto-refresh-on-the-run-sets.gif)

