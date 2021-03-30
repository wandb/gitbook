---
description: How to use W&B with multiprocessing
---

# Distributed Training

In distributed training, models are trained using multiple GPUs in parallel, for example with PyTorch DDP. To track distributed training using Weights & Biases, here are two patterns we support:

1. **One Process**: Only call `wandb.init()` and `wandb.log()` from the rank0 process, or a dedicated process for logging. This is the most common solution for logging with PyTorch DDP. In some cases, users funnel data over from other processes using a multiprocessing queue \(or another communication primitive\) to the main logging process.
2. **All Processes**: In every process, call `wandb.init()` and set the `group` parameter to the shared experiment name. [See details below](distributed-training.md#how-to-track-all-processes-with-grouping).

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

## Example of distributed training with W&B

Sometimes a single GPU is insufficient for training large deep learning models on huge amounts of data, so we use distributed training on multiple GPUS in parallel. PyTorch DDP \(`torch.nn.DistributedDataParallel`\) is a popular library for distributed training. In this walkthrough we'll show how to track metrics with Weights & Biases using PyTorch DDP for training on multiple parallel GPUs.

### Method 1: `wandb.init` on `rank0` process

In multi-GPU training, the `rank0` process is the main process and coordinates the other processes. Often, it's useful to just track this single process as a W&B run, using `wandb.init()` in just the `rank0` process and only calling `wandb.log()` there, not in any sub-processes.

In this example, we launch multiple processes with `torch.distributed.launch`. With this module, we can intercept the rank of the process with `--local_rank` argument. Now that we have the local-rank of the process we can set up conditional tracking in the `train()` function.

```text
if __name__ == "__main__":
    # get args
    args = parse_args()

    if args.local_rank == 0:
        # Initialize wandb run
        run = wandb.init(
            entity=args.entity,
            project=args.project,
        )
        # Train model with DDP
        train(args, run)
    else:
        train(args)
```

In the [W&B dashboard](https://wandb.ai/ayush-thakur/DDP/runs/1s56u3hc) you can see that both GPUs were tracked in this single run.

![](../.gitbook/assets/image%20%2869%29.png)

![](../.gitbook/assets/image%20%2864%29.png)

### Method 2: `wandb.init` on all processes

In this process, we track each process in the job, calling `wandb.init()` and `wandb.log()` from each individual process. It's also useful to call `wandb.finish()` at the end of training, to mark that the run has completed so all processes exit properly.

To organize each sub-process job into a larger group, set the `group` parameter in `wandb.init()`. This is useful because we can track every metric, including system utilization, for each process individually. These results will be grouped together on the **group page** in the W&B UI to keep things organized.

```text
if __name__ == "__main__":
    # get args
    args = parse_args()
    # Initialize run
    run = wandb.init(
        entity=args.entity,
        project=args.project,
        group = "DDP",
    )
    # Train model with DDP
    train(args, run)
```

In the [W&B UI](https://wandb.ai/ayush-thakur/DDP), you can see the two runs are grouped together in the sidebar. You can click on this group to get to the dedicated group page for your experiment.

![](../.gitbook/assets/image%20%2863%29.png)

