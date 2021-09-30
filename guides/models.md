---
description: >-
  Catalog and version models, standardize model evaluation, and promote the best
  models to production
---

# \[Beta\] Model Management

_We are actively building out the model registry and model evaluation use cases for W&B. Please contact us with questions and suggestions at support@wandb.com._

Use W&B for **Model Management** to track and report on the complete lifecycle of a model. Weights & Biases can log and capture:

1. **Dataset**: The exact version of the dataset a model trained on
2. **Code**: The code used in model training
3. **Model**: The weights of the trained model itself
4. **Metrics**: The evaluation results of a model on different golden datasets
5. **Status**: Where each model is in the pipeline \(ex. "staging" or "production"\)

### [Model Registry Demo](https://wandb.ai/timssweeney/model_registry_example/reports/MNIST-Model-Status--Vmlldzo4OTIyNTA)

![](../.gitbook/assets/image%20%28151%29.png)

Above you can see a sample table of models with:

1. **Model link**: A link to the registered model artifact in the app
2. **Version**: A unique version number for each registered model
3. **Status**: A label to indicate key model versions, like `production` 
4. **Loss @ 10k**: Metric calculated on an evaluation set of 10k
5. **Loss @ 1k:** Model metric calculated on an evaluation set of 1k

## Core features for model management

There are a few key features you can use to achieve the above Model Registry:

1. \*\*\*\*[**Runs**](track/): Track a job execution in your ML pipeline — ex. model training, model evaluation
2. \*\*\*\*[**Artifacts**](artifacts/): Track job inputs and outputs — ex. datasets, trained models
3. \*\*\*\*[**Tables**](data-vis/): Track and visualize tabular data — ex. evaluation datasets, model predictions
4. \*\*\*\*[**Weave**](../ref/app/features/panels/weave.md): Query and visualize logged data — ex. a list of trained models
5. \*\*\*\*[**Reports**](reports.md): Organize and visualize results — ex. charts, tables, and notes



