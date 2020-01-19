# Common Issues

### **Sweeps agents stop after the first runs finish**

One common reason for this is that the metric your are optimizing in your configuration YAML file is not a metric that you are logging. For example, you could be optimizing the metric **f1**, but logging **validation\_f1**. Double check that you're logging the exact metric name that you're optimizing.

### Entity and project name not set

`wandb: ERROR Error while calling W&B API: entityName and projectName required () Error: entityName and projectName required`

Your **entity** needs to be set to an existing team or username, and your **project** is the destination where your runs will be logged. If you're getting this error, please run `wandb login` on the machine where you're training your model.

Another option is to set the environment variables **WANDB\_ENTITY** and **WANDB\_PROJECT**.

