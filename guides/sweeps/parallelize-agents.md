# Parallelize agents

Parallelize your W\&B Sweep agents on a multi-core or multi-GPU machine. Before you get started, ensure you have initialized your W\&B Sweep. For more information on how to initialized a W\&B Sweep, see [Initialize sweeps](https://docs.wandb.ai/guides/sweeps/initialize-sweeps).&#x20;

### Parallelize on a multi-CPU machine

Depending on your use case, explore the proceeding tabs to learn how to parallelize W\&B Sweep agents using the CLI or within a Jupyter Notebook.

{% tabs %}
{% tab title="CLI" %}
Follow the procedure outlined below to parallelize your W\&B Sweep agent across multiple CPUs with the terminal:

1. Open more than one terminal window on your local machine.
2. Copy and past the W\&B Sweep ID across multiple terminals to parallelize a W\&B Sweep. The Sweep ID is generated when you initialize a Sweep.
{% endtab %}

{% tab title="Jupyter Notebook" %}
Follow the procedure outlined to parallelize your W\&B Sweep agent across multiple CPUs within Jupyter Notebooks.

1. Open more than one Jupyter Notebook.
2. Copy and past the W\&B Sweep ID across multiple Jupyter Notebooks to parallelize a W\&B Sweep. The Sweep ID is generated when you initialize a Sweep.
{% endtab %}
{% endtabs %}

### Parallelize on a multi-GPU machine

Follow the procedure outlined to parallelize your W\&B Sweep agent across multiple GPUs with a terminal using CUDA Toolkit:

1. Open more than one terminal window on your local machine.
2. Specify the GPU instance to use with `CUDA_VISIBLE_DEVICES` when you start a W\&B Sweep job ([`wandb agent`](https://docs.wandb.ai/ref/cli/wandb-agent)). Assign `CUDA_VISIBLE_DEVICES` an integer value corresponding to the GPU instance to use.&#x20;

For example, suppose you have two NVIDIA GPUs on your local machine. Open a terminal window and set `CUDA_VISIBLE_DEVICES` to `0` (`CUDA_VISIBLE_DEVICES=0`). Replace `sweep_ID` in the proceeding example with the W\&B Sweep ID that is returned when you initialized a W\&B Sweep:

Terminal 1

```bash
CUDA_VISIBLE_DEVICES=0 wandb agent sweep_ID
```

Open a second terminal window. Set `CUDA_VISIBLE_DEVICES` to `1` (`CUDA_VISIBLE_DEVICES=1`). Paste the same W\&B Sweep ID for the `sweep_ID` mentioned in the proceeding code snippet:

Terminal 2

```bash
CUDA_VISIBLE_DEVICES=1 wandb agent sweep_ID
```
