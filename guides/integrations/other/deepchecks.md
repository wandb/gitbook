# DeepChecks

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/deepchecks/deepchecks/blob/0.5.0-1-g5380093/docs/source/examples/guides/export\_outputs\_to\_wandb.ipynb)

DeepChecks helps you validate your machine learning models and data, such as verifying your data’s integrity, inspecting its distributions, validating data splits, evaluating your model and comparing between different models, all with with minimal effort.&#x20;

[Read more about DeepChecks and the wandb integration ->](https://docs.deepchecks.com/en/stable/examples/guides/export\_outputs\_to\_wandb.html)

## Getting Started

To use DeepChecks with Weights & Biases you will first need to sign up for a Weights & Biases account [here](https://wandb.ai/site).  With the Weights & Biases integration in DeepChecks you can quickly get started like so:

```python
import wandb
wandb.login()

# import your check from deepchecks
from deepchecks.checks import ModelErrorAnalysis

# run your check
result = ModelErrorAnalysis()...

# push that result to wandb
result.to_wandb()
```

You can also log an entire DeepChecks test suite to Weights & Biases

```python
import wandb
wandb.login()

# import your full_suite tests from deepchecks
from deepchecks.suites import full_suite

# create and run a DeepChecks test suite
suite_result = full_suite().run(...)

# push thes results to wandb
# here you can pass any wandb.init configs and arguments you need
suite_result.to_wandb(
    project='my-suite-project', 
    config={'suite-name': 'full-suite'}
)
```

## Example

``[**This Report**](https://wandb.ai/cayush/deepchecks/reports/Validate-your-Data-and-Models-with-Deepchecks-and-W-B--VmlldzoxNjY0ODc5) shows off the power of using DeepChecks and Weights & Biases&#x20;

![](<../../../.gitbook/assets/Screenshot 2022-03-16 at 12.45.31.png>)

