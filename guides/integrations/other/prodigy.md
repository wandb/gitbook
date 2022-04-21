# Prodigy

[Prodigy](https://prodi.gy) is an annotation tool for creating training and evaluation data for machine learning models, error analysis, data inspection & cleaning. [W\&B Tables](../../data-vis/tables-quickstart.md) allow you to log, visualize, analyze, and share datasets (and more!) inside W\&B.

The [W\&B integration with Prodigy](https://github.com/wandb/client/blob/master/wandb/integration/prodigy/prodigy.py) adds simple and easy-to-use functionality to upload your Prodigy-annotated dataset directly to W\&B for use with Tables.

Run a few lines of code, like these:

```python
import wandb
from wandb.integration.prodigy import upload_dataset

with wandb.init(project="prodigy"):
    upload_dataset("news_headlines_ner")
```

and get visual, interactive, shareable tables like this one:

![](../../../.gitbook/assets/screenshot-from-2021-08-25-13-04-57.png)

## Quickstart

Use `wandb.integration.prodigy.upload_dataset` to upload your annotated prodigy dataset directly from the local Prodigy database to W\&B in our [Table](https://docs.wandb.ai/ref/python/data-types/table) format. For more information on Prodigy, including installation & setup, please refer to the [Prodigy documentation](https://prodi.gy/docs/).

W\&B will automatically try to convert images and named entity fields to [`wandb.Image`](https://docs.wandb.ai/ref/python/data-types/image) and [`wandb.Html`](https://docs.wandb.ai/ref/python/data-types/html)respectively. Extra columns may be added to the resulting table to include these visualizations.

## Read through a detailed example

This W\&B Report demonstrates visualizations generated using the W\&B Prodigy integration:

{% embed url="https://wandb.ai/kshen/prodigy/reports/Visualizing-Prodigy-Datasets-Using-W-B-Tables--Vmlldzo5NDE2MTc" %}

## Also using spaCy?

W\&B also has an integration with spaCy, see the [docs here](https://docs.wandb.ai/guides/integrations/spacy).
