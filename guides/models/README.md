---
description: Manage the model lifecycle from training to production
---

# Model Management

Use the W\&B Model Registry as a central system of record for models.

## Model Registry

* Create Collections to organize your best model versions for a given task
* Track a model moving into staging and production
* See a history of all changes, including who moved a model to production

![](<../../.gitbook/assets/image (1).png>)

### Model Versioning

Iterate to get the best model version for a task, and catalog all the changes along the way.

* Track every model version in a central repository
* Browse and compare model versions
* Capture training metrics and hyperparameters

### Model Lineage

Document and reproduce the complete pipeline of model training and evaluation.

* Identify the exact dataset version the model trained on
* Restore the training code, including git commit and diff patch
* Get back to the model’s hyperparameters and other metadata for reproducibility
* Dig in to upstream jobs that can affect model performance

### Model Lifecycle

Manage the process as a model moves from training through staging to production.

* Highlight the best model versions that are being evaluated for production
* Communicate where a model version is in the process — staging, production etc
* Review the history of model versions that moved through each stage

## Up Next

Dig into the details of how to use Weights & Biases for model management:

{% content-ref url="model-management-concepts.md" %}
[model-management-concepts.md](model-management-concepts.md)
{% endcontent-ref %}

{% content-ref url="walkthrough.md" %}
[walkthrough.md](walkthrough.md)
{% endcontent-ref %}
