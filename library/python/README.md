---
description: Overview of our client library.
---

# Python Library

Use our python library to instrument your machine learning model and track experiments. Setup should only take a few lines of code. If you're using a popular framework, we have a number of integrations to make setting up wandb easy.

We have more detailed docs generated from the code in [Reference](../reference/).

{% page-ref page="../frameworks/" %}

### **Instrumenting a model**

* wandb.init — initialize a new run at the top of your training script
* wandb.config — track hyperparameters
* wandb.log — log metrics over time within your training loop
* wandb.save — save files in association with your run, like model weights
* wandb.restore — restore the state of your code when you ran a given run

{% page-ref page="init.md" %}

{% page-ref page="config.md" %}

{% page-ref page="log.md" %}

{% page-ref page="save.md" %}

{% page-ref page="restore.md" %}



