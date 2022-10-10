---
description: Hyperparameter search and model optimization with W&B Sweeps
---

# Tune Hyperparameters

Use Weights & Biases Sweeps to automate hyperparameter search and explore the space of possible models. Create a sweep with a few lines of code. Sweeps combines the benefits of automated hyperparameter search with our visualization-rich, interactive experiment tracking. Pick from popular search methods such as Bayesian, grid search, and random to search the hyperparameter space.  Scale and parallelize Sweep jobs across one or more machines.&#x20;

![Draw insights from large hyperparameter tuning experiments with interactive dashboards.](<../../.gitbook/assets/image (114).png>)

### How it works

There are two components to Weights & Biases Sweeps: a _controller_ and one or more _agents_. The controller picks out new hyperparameter combinations. [Typically the Sweep server is managed on the Weights & Biases server](https://docs.wandb.ai/guides/sweeps/local-controller).

Agents query the Weights & Biases server for hyperparameters and use them to run model training. The training results are then reported back to the Sweep server. Agents can run one or more processes on one or more machines. The flexibility of agents to run multiples processes across multiples machines makes it easy to parallelize and scale Sweeps. For more information on how to scale sweeps, see [Parallelize agents](https://docs.wandb.ai/guides/sweeps/parallelize-agents).

Create a W\&B Sweep with the following steps:

1. **Add W\&B to your code:** In your Python script, add a couple lines of code to log hyperparameters and output metrics from your script. See [Add W\&B to your code](https://docs.wandb.ai/guides/sweeps/add-w-and-b-to-your-code) for more information.
2. **Define the sweep configuration**: Define the variables and ranges to sweep over. Pick a search strategy— we support grid, random, and Bayesian search, plus techniques for faster iterations like early stopping. See [Define sweep configuration](https://docs.wandb.ai/guides/sweeps/define-sweep-configuration) for more information.
3. **Initialize sweep**: Start the Sweep server. We host this central controller and coordinate between the agents that execute the sweep. See [Initialize sweeps](https://docs.wandb.ai/guides/sweeps/initialize-sweeps) for more information.
4. **Start sweep**: Run a single-line command on each machine you'd like to use to train models in the sweep. The agents ask the central sweep server what hyperparameters to try next, and then they execute the runs. See [Start sweep agents](https://docs.wandb.ai/guides/sweeps/start-sweep-agents) for more information.&#x20;
5. **Visualize results (optional)**: Open our live dashboard to see all your results in one central place.

### How to get started

Depending on your use case, explore the following resources to get started with Weights & Biases Sweeps:

* If this is your first time hyperparameter tuning with Weights & Biases Sweeps, we recommend you read the [Quickstart](https://docs.wandb.ai/guides/sweeps/quickstart). The Quickstart walks you through setting up your first W\&B Sweep.
* Explore topics about Sweeps in the Weights and Biases Developer Guide such as:
  * [Add W\&B to your code](https://docs.wandb.ai/guides/sweeps/add-w-and-b-to-your-code)
  * [Define sweep configuration](https://docs.wandb.ai/guides/sweeps/define-sweep-configuration)
  * [Initialize sweeps](https://docs.wandb.ai/guides/sweeps/initialize-sweeps)
  * [Start sweep agents](https://docs.wandb.ai/guides/sweeps/start-sweep-agents)
  * [Visualize sweep results](https://docs.wandb.ai/guides/sweeps/visualize-sweep-results)
* Try our [Organizing Hyperparameter Sweeps in PyTorch](https://colab.research.google.com/github/wandb/examples/blob/master/colabs/pytorch/Organizing\_Hyperparameter\_Sweeps\_in\_PyTorch\_with\_W%26B.ipynb#scrollTo=e43v8-9MEoYk) Google Colab Jupyter notebook for an example of how to create sweeps using the PyTorch framework in a Jupyter notebook.
* Explore a [curated list of Sweep experiments](https://docs.wandb.ai/guides/sweeps/useful-resources#reports-with-sweeps) that explore hyperparameter optimization with W\&B Sweeps. Results are stored in W\&B Reports.
* Read the [Weights & Biases SDK Reference Guide](https://docs.wandb.ai/ref).&#x20;

For a step-by-step video, see: [Tune Hyperparameters Easily with W\&B Sweeps](https://www.youtube.com/watch?v=9zrmUIlScdY\&ab\_channel=Weights%26Biases):

{% embed url="http://wandb.me/sweeps-video" %}
