---
description: Visualize highly dimensional data across your machine learning experiments
---

# Parallel Coordinates

Here's an example of a parallel coordinates plot. Each axis represents a different In this case I've chosen four vertical axes. In this case I'm visualizing the relationship between different hyperparameters and the final accuracy of my model.

* **Axes**: Different hyperparameters from [wandb.config](https://docs.wandb.ai/library/config) and metrics from [wandb.log\(\)](https://docs.wandb.ai/library/log)​
* **Lines**: Each line represents a single run. Mouse over a line to see a tooltip with details about the run. All lines that match the current filters will be shown, but if you turn off the eye, lines will be grayed out.

**Panel Settings**

Configure these features in the panel settings— click the edit button in the upper right corner of the panel.

* **Tooltip**: On hover, a legend shows up with info on each run
* **Titles**: Edit the axis titles to be more readable
* **Gradient**: Customize the gradient to be any color range you like
* **Log scale**: Each axis can be set to view on a log scale independently
* **Flip axis**: Switch the axis direction— this is useful when you have both accuracy and loss as columns

  ​[See it live →](https://app.wandb.ai/example-team/sweep-demo/reports/Zoom-in-on-Parallel-Coordinates-Charts--Vmlldzo5MTQ4Nw)​

[  
](https://docs.wandb.ai/app/features/panels/code)

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-M5xuU0oX-LvRy7dSwR6%2F-M5xukhBPmcayMQ4aM91%2F2020-04-27%2016.11.43.gif?alt=media&token=f3e78351-3eff-4137-a9a3-0dc30355165c)

