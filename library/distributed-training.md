# Distributed Training

In distributed training, models are trained using multiple GPUs in parallel, for example with PyTorch DDP. To track distributed training using Weights & Biases, here are two patterns we support:

1. **One Process**: Only call `wandb.init()` and `wandb.log()` from the rank0 process, or a dedicated process for logging. This is the most common solution for logging with PyTorch DDP. In some cases, users funnel data over from other processes using a multiprocessing queue \(or another communication primitive\) to the main logging process.
2. \*\*\*\*[**All Processes**](distributed-training.md#how-to-track-all-processes-with-grouping): In every process, call `wandb.init()` and set the `group` parameter to the shared experiment name, like `wandb.init(group="experiment_1")`. You can also set group from the environment variable WANDB\_RUN\_GROUP. In the UI, this will automatically group individual jobs into the larger group and provide you a dedicated Group Page.

## Common issues

### Hanging at the beginning of training

If launching the wandb process hangs, set the WANDB\_START\_METHOD environment variable to "thread" to have wandb not use multiprocessing.

### Hanging at the end of training

Is your process hanging at the end of training? The wandb.init\(\) process might not know it needs to exit, and cause your job to hang. In this case, call `wandb.finish()` at the end of your script to mark the run as finished and cause wandb to exit.

## How to track all processes with grouping

There are a three ways to set grouping. Here's a quick, easy example of setting grouping:

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](http://wandb.me/grouping)

### **1. Set group in your script**

Pass an optional group and job\_type to wandb.init\(\). This gives you a dedicated group page for each experiment, which contains the individual runs. For example:`wandb.init(group="experiment_1", job_type="eval")`

### **2. Set a group environment variable**

Use `WANDB_RUN_GROUP` to specify a group for your runs as an environment variable. For more on this, check our docs for [**Environment Variables**](environment-variables.md)**. Group** should be unique within your project and shared by all runs in the group.  You can use `wandb.util.generate_id()` to generate a unique 8 character string to use in all your processes— for example:`os.environ["WANDB_RUN_GROUP"] = "experiment-" + wandb.util.generate_id()`

### **3. Toggle grouping in the UI**

You can dynamically group by any config column. For example, if you use `wandb.config` to log batch size or learning rate, you can then group by those hyperparameters dynamically in the web app. 

## Distributed training with grouping

If you set grouping in `wandb.init()` , we will group runs by default in the UI. You can toggle this on and off by clicking the purple Group button at the top of the table. Here's an [example project](https://wandb.ai/carey/group-demo?workspace=user-carey) generated from [sample code](http://wandb.me/grouping) where we set grouping. You can click on each "Group" row in the sidebar to get to a dedicated group page for that experiment.

![](../.gitbook/assets/image%20%2850%29.png)

From the project page above, you can click a **Group** in the left sidebar to get to a dedicated page like [this one](https://wandb.ai/carey/group-demo/groups/exp_5?workspace=user-carey):

![](../.gitbook/assets/image%20%2851%29.png)

