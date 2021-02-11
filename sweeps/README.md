---
description: Hyperparameter search and model optimization
---

# Sweeps

Use Weights & Biases Sweeps to automate hyperparameter optimization and explore the space of possible models.

## Benefits of using W&B Sweeps

1. **Quick setup**: With just a few lines of code you can run W&B sweeps.
2. **Transparent**: We cite all the algorithms we're using, and [our code is open source](https://github.com/wandb/client/tree/master/wandb/sweeps).
3. **Powerful**: Our sweeps are completely customizable and configurable. You can launch a sweep across dozens of machines, and it's just as easy as starting a sweep on your laptop.

## Common Use Cases

1. **Explore**: Efficiently sample the space of hyperparameter combinations to discover promising regions and build an intuition about your model.
2. **Optimize**:  Use sweeps to find a set of hyperparameters with optimal performance.
3. **K-fold cross validation**: Here's [a brief code example](https://github.com/wandb/examples/tree/master/examples/wandb-sweeps/sweeps-cross-validation) of k-fold cross validation with W&B Sweeps.

## Approach

1. **Add wandb**: In your Python script, add a couple lines of code to log hyperparameters and output metrics from your script. [Get started now →](quickstart.md)
2. **Write config**: Define the variables and ranges to sweep over. Pick a search strategy— we support grid, random, and Bayesian search, as well as early stopping. Check out some example configs [here](https://github.com/wandb/examples/tree/master/examples/keras/keras-cnn-fashion).
3. **Initialize sweep**: Launch the sweep server. We host this central controller and coordinate between the agents that execute the sweep.
4. **Launch agent\(s\)**: Run this command on each machine you'd like to use to train models in the sweep. The agents ask the central sweep server what hyperparameters to try next, and then they execute the runs.
5. **Visualize results**: Open our live dashboard to see all your results in one central place.

![](../.gitbook/assets/central-sweep-server-3%20%282%29%20%282%29%20%283%29%20%283%29%20%282%29.png)

{% page-ref page="quickstart.md" %}

{% page-ref page="existing-project.md" %}

{% page-ref page="configuration.md" %}

{% page-ref page="local-controller.md" %}

{% page-ref page="python-api.md" %}

{% page-ref page="sweeps-examples.md" %}

