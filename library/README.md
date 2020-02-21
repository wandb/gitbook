---
description: Overview of our client library
---

# Python Library

Use our python library to instrument your machine learning model and track experiments. Setup should only take a few lines of code. If you're using a popular framework, we have a number of integrations to make setting up wandb easy.

We have more detailed docs generated from the code in [Reference](reference/).

{% page-ref page="frameworks/" %}

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

## Common Questions

### Multiple wandb users on shared machines



If you're using a shared machine and another person is a wandb user, it's easy to make sure your runs are always logged to the proper account. Set the [WANDB\_API\_KEY environment variable](advanced/environment-variables.md) to authenticate. If you source it in your env, when you log in you'll have the right credentials, or you can set the environment variable from your script.

### Organization best practices <a id="best-practices"></a>

We provide a very flexible and customizable tool. You're free to use our tools however you'd like, but here are some guidelines for how to think about our tools.

Here's an example of setting up a run:

```python
import wandb

config = dict (
  learning_rate = 0.01,
  momentum = 0.2,
  architecture = "CNN",
  dataset_id = "peds-0192",
  infra = "AWS",
)

wandb.init(
  project="detect-pedestrians",
  notes="tweak baseline",
  tags=["baseline", "paper1"],
  config=config,
)
```

**Suggested usage**

1. **Config**: Track hyperparameters, architecture, dataset, and anything else you'd like to use to reproduce your model. These will show up in columns— use config columns to group, sort, and filter runs dynamically in the app.
2. **Project**: A project is a set of experiments you can compare together. Each project gets a dedicated dashboard page, and you can easily turn on and off different groups of runs to compare different model versions.
3. **Notes**: A quick commit message to yourself, the note can be set from your script and is editable in the table. We suggest using the notes field instead of overwriting the generated run name.
4. **Tags**: Identify baseline runs and favorite runs. You can filter runs using tags, and they're editable in the table.

Here's what this looks like on the run page and project page:

![Example overview tab on the run page](../.gitbook/assets/image%20%2811%29.png)

![Example table on the project page](../.gitbook/assets/image%20%2831%29.png)

