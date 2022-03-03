---
description: How to use W&B when training with multiple GPUs
---

# Distributed Training

In distributed training, models are trained using multiple GPUs in parallel. To track distributed training using Weights & Biases, here are two patterns we support:

1. **One Process**: Only call `wandb.init()` and `wandb.log()` from a single process, e.g. the rank0 process. This is the most common solution for logging with PyTorch DDP. In some cases, users funnel data over from other processes using a multiprocessing queue (or another communication primitive) to the main logging process.
2. **All Processes**: In every process, call `wandb.init()`. These are effectively separate experiments, so use the `group` parameter to set a shared experiment name and group the logged values together in the UI.

Below, you'll find a more thorough description of these two patterns, based on a[ code example](https://github.com/wandb/examples/tree/master/examples/pytorch/pytorch-ddp) from our repository of examples. Check out the "Common Issues" section at the bottom of this guide for some gotchas.

## Logging distributed training experiments with W\&B

{% hint style="info" %}
Check out the code behind these examples in our examples repository [here](https://github.com/wandb/examples/tree/master/examples/pytorch/pytorch-ddp).
{% endhint %}

Sometimes a single GPU is insufficient for training large deep learning models on huge amounts of data, so we distribute our training runs across multiple GPUs. [PyTorch DDP](https://pytorch.org/tutorials/intermediate/ddp\_tutorial.html) (`DistributedDataParallel` in`torch.nn`) is a popular library for distributed training. In this walkthrough, we'll show how to track metrics with Weights & Biases using PyTorch DDP on two GPUs on a single machine. The basic principles apply to any distributed training setup, but the details of implementation may differ.

### Method 1: `wandb.init` on `rank0` process

In multi-GPU training, the `rank0` process is the main process and coordinates the other processes. Often, it's useful to just track this single process as a W\&B run, using `wandb.init()` in just the `rank0` process and only calling `wandb.log()` there, not in any sub-processes.

This method is simple and robust, but it means that model metrics from other processes (e.g. loss values or inputs from their batches) are not logged. System metrics, like usage and memory, are still logged for all GPUs, since that information is available to all processes.

{% hint style="info" %}
**Use this method if the metrics you care about are available from a single process**. Typical examples include GPU/CPU utilization, behavior on a shared validation set, gradients and parameters, and loss values on representative data examples.
{% endhint %}

In [our example](https://github.com/wandb/examples/tree/master/examples/pytorch/pytorch-ddp#method-1-log-from-a-single-process) of this method, we launch multiple processes with `torch.distributed.launch`. With this module, we can determine the rank of the process from the `--local_rank` argument. Now that we have the rank of the process, we can set up `wandb` logging conditionally in the `train()` function.

```python
if __name__ == "__main__":
    # Get args
    args = parse_args()

    if args.local_rank == 0:  # only on main process
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

If you want to see what the outputs look like for this method, check out an example dashboard [here](https://wandb.ai/ayush-thakur/DDP/runs/1s56u3hc/system). There, you can see that system metrics, like temperature and utilization, were tracked for both GPUs.

![](<../../../.gitbook/assets/image (69).png>)

The epoch-wise and batch-wise loss values, however, are only logged from a single GPU.

![](<../../../.gitbook/assets/image (68) (1).png>)

### Method 2: `wandb.init` on all processes

In this method, we track each process in the job, calling `wandb.init()` and `wandb.log()` from each process separately. It's also useful to call `wandb.finish()` at the end of training, to mark that the run has completed so that all processes exit properly.

The benefit of this method is that more information is accessible for logging and that logging doesn't need to be made conditional on process rank in the code. However, it results in information from a single experiment being reported from multiple runs in the W\&B UI.

{% hint style="info" %}
**Use this method if you care about the private metrics of individual processes**. Typical examples include the data and predictions on each node (for debugging data distribution) and metrics on individual batches outside of the main node. This method is not necessary to get system metrics from all nodes nor to get summary statistics available on the main node.
{% endhint %}

In order to keep track of which runs correspond to which experiments, we use the [grouping](grouping.md) feature of Weights & Biases. It's as simple as setting the `group` parameter in `wandb.init()`. These results will be shown together on a group page in the W\&B UI, so our experiments stay organized.

```python
if __name__ == "__main__":
    # Get args
    args = parse_args()
    # Initialize run
    run = wandb.init(
        entity=args.entity,
        project=args.project,
        group="DDP",  # all runs for the experiment in one group
    )
    # Train model with DDP
    train(args, run)
```

If you want to see what the outputs look like for this method, check out an example dashboard [here](https://wandb.ai/ayush-thakur/DDP). You'll see two runs grouped together in the sidebar. You can click on this group to get to the dedicated group page for the experiment, which displays metrics from each process separately.

![](<../../../.gitbook/assets/image (63).png>)

## Common issues

### Hanging at the beginning of training

If launching the `wandb` process hangs, it could be because the `wandb` multiprocessing is interfering with the multiprocessing from distributed training. Try setting the `WANDB_START_METHOD` environment variable to `"thread"` to use multithreading instead.

### Hanging at the end of training

Is your process hanging at the end of training? The `wandb` process might not know it needs to exit, and that will cause your job to hang. In this case, call `wandb.finish()` at the end of your script to mark the run as finished and cause `wandb` to exit.
