# Distributed Training

In distributed training, models are trained using multiple GPUs in parallel, for example with PyTorch DDP. To track distributed training using Weights & Biases, here are two patterns we support:

1. **One Process**: Only call `wandb.init()` and `wandb.log()` from the rank0 process, or a dedicated process for logging. This is the most common solution for logging with PyTorch DDP. In some cases, users funnel data over from other processes using a multiprocessing queue \(or another communication primitive\) to the main logging process.
2. **All Processes**: In every process, call `wandb.init()` and set the `group` parameter to the shared experiment name, like `wandb.init(group="experiment_1")`. You can also set group from the environment variable WANDB\_RUN\_GROUP. In the UI, this will automatically group individual jobs into the larger group and provide you a dedicated Group Page.

## Common issues

### Hanging at the beginning of training

If launching the wandb process hangs, set the WANDB\_START\_METHOD environment variable to "thread" to have us not use multiprocessing.

### Hanging at the end of training

Is your process hanging at the end of training? The wandb.init\(\) process might not know it needs to exit, and cause your job to hang. In this case, call `wandb.finish()` at the end of your script to mark the run as finished and cause wandb to exit.

