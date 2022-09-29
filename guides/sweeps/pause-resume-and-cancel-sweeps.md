# Pause, resume and cancel sweeps

Pause, resume, and cancel a W\&B Sweep with the CLI. Stopping a sweep tells the W\&B Sweep agent that the hyperparameter search is over. Pausing a W\&B Sweep tells the W\&B agent that new W\&B Runs should not be executed until the Sweep is resumed. Stopping a W\&B Sweep tells the W\&B Sweep agent to stop creating or executing new W\&B Runs.

In each case, provide the W\&B Sweep ID that was generated when you initialized a W\&B Sweep. Optionally open a new terminal window to execute the proceeding commands. A new terminal window makes it easier to execute a command if a W\&B Sweep is printing output statements to your current terminal window.

Use the following guidance to pause, resume, and cancel sweeps.&#x20;

### Pause sweeps

Pause a W\&B Sweep so it temporarily stops executing new W\&B Runs. Use the `wandb sweep --pause` command to pause a W\&B Sweep. Provide the W\&B Sweep ID that you want to pause.&#x20;

```bash
wandb sweep --pause entity/project/sweep_ID
```

### Resume sweeps

Resume a paused W\&B Sweep with the `wandb sweep --resume` command. Provide the W\&B Sweep ID that you want to resume:

```
wandb sweep --resume entity/project/sweep_ID
```

### Cancel sweeps

Cancel a sweep to kill all running runs and stop running new runs. Use the `wandb sweep --cancel` command to cancel a W\&B Sweep. Provide the W\&B Sweep ID that you want to cancel.

```bash
wandb sweep --cancel entity/project/sweep_ID
```

For a full list of CLI command options, see the [wandb sweep](https://docs.wandb.ai/ref/cli/wandb-sweep) CLI Reference Guide.
