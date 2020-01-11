---
description: Use W&B Sweeps to manage hyperparameter searches
---

# Sweeps Overview

Hyperparameter sweeps can help you optimally tune an existing model or efficiently sample a model configuration space for promising regions and ideas. 

Instead of manually tracking variables, launch commands, and results, write a short config for the hyperparameter ranges of interest \(as a YAML file or Python dictionary\). Pick a search strategy \(grid, random, or Bayes\) and start the sweep in two commands, with optional early stopping. Our Python API will automatically schedule, launch, and store all the runs in your sweep. As the sweep runs, we'll visualize the relative importance of different hyperparameters and organize all the details of your experiments in your browser.

{% page-ref page="quickstart.md" %}

{% page-ref page="add-to-existing.md" %}

{% page-ref page="../configuration.md" %}

{% page-ref page="../local-controller.md" %}

{% page-ref page="../python-api.md" %}

