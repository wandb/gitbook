---
description: >-
  A Weights & Biases integration for the spaCy library: industrial strength NLP,
  logged with W&B
---

# spaCy

[spaCy](https://spacy.io/) is a NLP library used across industry. As of spaCy v3, Weights and Biases can now be used with [spaCy train](https://spacy.io/api/cli#train) to track your spaCy model's training metrics as well as save and versioning your models and datasets

## Getting Started: Track and Save your Models

### **1\)** **Install the `wandb` Library and Log in**

{% tabs %}
{% tab title="Notebook" %}
```python
!pip install wandb

import wandb
wandb.login()
```
{% endtab %}

{% tab title="Command Line" %}
```python
pip install wandb
wandb login
```
{% endtab %}
{% endtabs %}

### **2\) Add the `WandbLogger` to your spaCy Config File**

If you are unfamiliar with how spaCy training config files work, please refer to [spaCy's documentation](https://spacy.io/usage/training) for more info

```python
[training.logger]
@loggers = "spacy.WandbLogger.v2"
project_name = "my_spacy_project"
remove_config_values = ["paths.train", "paths.dev", "corpora.train.path", "corpora.dev.path"]
log_dataset_dir = "./corpus"
model_log_interval = 1000
```

| ARGUMENT | DESCRIPTION |
| :--- | :--- |
| `project_name` | The name of the Weights & Biases project. The project will be created automatically if it doesn’t exist yet.`str` |
| `remove_config_values` | A list of values to exclude from the config before it is uploaded to W&B \(`[]` by default\).`List[str]` |
| `model_log_interval` | If set, model checkpointing to [Weights & Biases' Artifacts](https://docs.wandb.ai/artifacts)  will be enabled. Pass in the number of steps to wait between logging model checkpoints \(`Optional int`,`None` by default\) |
| `log_dataset_dir` | If passed a path, the dataset at that directory path will be logged and versioned in  [Weights & Biases' Artifacts](https://docs.wandb.ai/artifacts) at the beginning of training \(`Optional str, None` by default\) |

### 3\) Start Training

Once you have added the `WandbLogger` to your spaCy training config you can run `spacy train` as usual:

{% tabs %}
{% tab title="Notebook" %}
```python
!python -m spacy train \
    config.cfg \
    --output ./output \
    --paths.train ./train \
    --paths.dev ./dev
```
{% endtab %}

{% tab title="Command Line" %}
```python
!python -m spacy train \
    config.cfg \
    --output ./output \
    --paths.train ./train \
    --paths.dev ./dev
```
{% endtab %}
{% endtabs %}

When training begins, a link to your W&B training run will be output which will take you to this run's experiment tracking dashboard in the Weights & Biases web UI

