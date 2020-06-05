---
description: 'Versioned data, models and results across your pipelines'
---

# Artifacts

Artifacts is the latest addition to the Weights & Biases toolkit. We're iteratively building this new product with feedback from our users. Reach out in the [W&B Forum](http://bit.ly/wandb-forum) with questions or suggestions.

Artifacts are used to store and keep track datasets, models, and evaluation results across machine learning pipelines.

Think of an artifact as "a versioned folder of data". You can store entire datasets directly in artifacts, or use artifact references to point to data in other systems.

 Using our Artifacts API, you can log artifacts as outputs of W&B runs, or use artifacts as input to runs.

![](../.gitbook/assets/simple-artifact-diagram-2.png)

Since a run can use another run’s output artifact as input, artifacts and runs together form a directed graph. You don’t need to define pipelines ahead of time. Just use and log artifacts, and we’ll stitch everything together.

![](../.gitbook/assets/artifact2.png)

