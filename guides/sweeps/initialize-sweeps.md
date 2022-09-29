# Initialize sweeps

Weights & Biases uses a _Sweep Controller_ to manage sweeps on the cloud (standard), locally (local) across one or more machines. After a run completes, the sweep controller will issue a new set of instructions describing a new run to execute. These instructions are picked up by _agents_ who actually perform the runs. In a typical W\&B Sweep, the controller lives on the Weights & Biases server. Agents live on _your_ machine(s).



The following code snippets demonstrate how to initialize sweeps with the CLI and within a Jupyter Notebook or Python script.

{% hint style="warning" %}
1. Before you initialize a sweep, make sure you have a sweep configuration defined either in a YAML file or a nested Python dictionary object in your script. For more information see, [Define sweep configuration](https://docs.wandb.ai/guides/sweeps/define-sweep-configuration).
2. Both the W\&B Sweep and the W\&B Run must be in the same project. Therefore, the name you provide when you initialize Weights & Biases ([`wandb.init`](https://docs.wandb.ai/ref/python/init)) must match the name of the project you provide when you initialize a W\&B Sweep ([`wandb.sweep`](https://docs.wandb.ai/ref/python/sweep)).
{% endhint %}

{% tabs %}
{% tab title="Python script or Jupyter Notebook" %}
Use the Weights & Biases SDK to initialize a sweep. Pass the sweep configuration dictionary to the `sweep` parameter. Optionally provide the name of the project for the project parameter (`project`) where you want the output of the W\&B Run to be stored.  If the project is not specified, the run is put in an "Uncategorized" project.

```python
import wandb

# Example sweep configuration
sweep_configuration = {
    'method': 'random',
    'name': 'sweep',
    'metric': {
        'goal': 'maximize', 
        'name': 'val_acc'
		},
    'parameters': {
        'batch_size': {'values': [16, 32, 64]},
        'epochs': {'values': [5, 10, 15]},
        'lr': {'max': 0.1, 'min': 0.0001}
     }
}

sweep_id = wandb.sweep(sweep=sweep_configuration, project="project-name")
```

The [`wandb.sweep`](https://docs.wandb.ai/ref/python/sweep) function returns the sweep ID. The sweep ID includes the entity name and the project name. Make a note of the sweep ID.
{% endtab %}

{% tab title="CLI" %}
Use the Weights & Biases CLI to initialize a sweep. Provide the name of your configuration file. Optionally provide the name of the project for the `project` flag.  If the project is not specified, the W\&B Run is put in an "Uncategorized" project.&#x20;

Use the [`wandb sweep`](https://docs.wandb.ai/ref/cli/wandb-sweep) command to initialize a sweep. The proceeding code example initializes a sweep for a `sweeps_demo` project and uses a `config.yaml` file for the configuration.

```bash
wandb sweep --project sweeps_demo config.yaml
```

This command will print out a sweep ID. The sweep ID includes the entity name and the project name. Make a note of the sweep ID.
{% endtab %}
{% endtabs %}
