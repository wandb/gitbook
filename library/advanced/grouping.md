# Grouping

W&B provides the ability to group runs up to two levels. This is useful for distributed training or combining multiple process types. 

After you've run a run, our web interface lets you select any **config** variable and group runs that share the same value in that column. 

If you'd like to specify grouping before you launch experiments, you have a couple options.

1. [Environment Variable](environment-variables.md): Use the`WANDB_RUN_GROUP` environment variable
2. Pass arguments to [wandb.init](../init.md):
   * For a single level of grouping, set the **group** argument
   * For a second level of grouping, set the **job\_type** argument

This is how grouping appears in the table:

![](../../.gitbook/assets/image%20%2857%29.png)

Expand a grouped row in the dropdown menu to see the runs in that group.

![](../../.gitbook/assets/image%20%2834%29.png)

Here's the project page with the sidebar collapsed. Runs appear as grouped lines on the graphs, defaulting to showing a line for the group mean and a shaded region for the variance.

![](../../.gitbook/assets/image%20%2840%29.png)

Click the edit button in the upper right corner of the graph and select the **Advanced** tab to change the line and shading. You can select the mean, minimum, or maximum value to for the line in each group. For the shading, you can turn off shading, show the min and max, the standard deviation, and the standard error.

![](../../.gitbook/assets/image%20%2835%29.png)



