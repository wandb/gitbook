---
description: >-
  A Weights & Biases integration for Hugging Face's Transformers library:
  solving NLP, one logged run at a time!
---

# Hugging Face Transformers

The [Hugging Face Transformers](https://huggingface.co/transformers/) library makes state-of-the-art NLP models like BERT and training techniques like mixed precision and gradient checkpointing easy to use. The [W\&B integration](https://huggingface.co/transformers/main\_classes/callback.html#transformers.integrations.WandbCallback) adds rich, flexible experiment tracking and model versioning to interactive centralized dashboards without compromising that ease of use.

## 🤗 Next-level logging in 2 lines

```python
from transformers import TrainingArguments, Trainer

args = TrainingArguments(... , report_to="wandb")
trainer = Trainer(... , args=args)
```

![Explore your experiment results in the W\&B interactive dashboard](../../.gitbook/assets/huggingface\_gif.gif)

## This guide covers

* how to [**get started using W\&B with Hugging Face Transformers**](huggingface.md#getting-started-track-and-save-your-models) to track your NLP experiments and
* how to use [**advanced features of the W\&B Hugging Face integration**](../track/advanced/) to get the most out of experiment tracking.

{% hint style="info" %}
If you'd rather dive straight into working code, check out this [Google Colab](https://wandb.me/hf).
{% endhint %}

## Getting started: track experiments

### **1)** **Install the `wandb` library and log in**

{% tabs %}
{% tab title="Notebook" %}
```python
!pip install wandb

import wandb
wandb.login()
```
{% endtab %}

{% tab title="Command Line" %}
```bash
pip install wandb
wandb login
```
{% endtab %}
{% endtabs %}

### **2) Name the project**

A [Project](../../ref/app/pages/project-page.md) is where all of the charts, data, and models logged from related runs are stored. Naming your project helps you organize your work and keep all the information about a single project in one place.

To add a run to a project simply set the `WANDB_PROJECT` environment variable to the name of your project. The `WandbCallback` will pick up this project name environment variable and use it when setting up your run.

{% tabs %}
{% tab title="Notebook" %}
```python
%env WANDB_PROJECT=amazon_sentiment_analysis
```
{% endtab %}

{% tab title="Command Line" %}
```bash
WANDB_PROJECT=amazon_sentiment_analysis
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Make sure you set the project name _before_ you initialize the `Trainer`.
{% endhint %}

If a project name is not specified the project name defaults to "huggingface".

### **3)** Log your training runs to W\&B

This is **the most important step:** when defining your `Trainer` training arguments, either inside your code or from the command line, set `report_to` to `"wandb"` in order enable logging with Weights & Biases.

You can also give a name to the training run using the `run_name` argument.

{% hint style="info" %}
Using TensorFlow? Just swap the PyTorch `Trainer` for the TensorFlow `TFTrainer`.
{% endhint %}

That's it! Now your models will log losses, evaluation metrics, model topology, and gradients to Weights & Biases while they train.

{% tabs %}
{% tab title="Notebook" %}
```python
from transformers import TrainingArguments, Trainer

args = TrainingArguments(
    # other args and kwargs here
    report_to="wandb",  # enable logging to W&B
    run_name="bert-base-high-lr"  # name of the W&B run (optional)
)

trainer = Trainer(
    # other args and kwargs here
    args=args,  # your training args
)

trainer.train()  # start training and logging to W&B
```
{% endtab %}

{% tab title="Command Line" %}
```python
python run_glue.py \     # run your Python script
  --report_to wandb \    # enable logging to W&B
  --run_name bert-base-high-lr \   # name of the W&B run (optional)
  # other command line arguments here
```
{% endtab %}
{% endtabs %}

#### (Notebook only) Finish your W\&B Run

If your training is encapsulated in a Python script, the W\&B run will end when your script finishes.

If you are using a Jupyter or Google Colab notebook, you'll need to tell us when you're done with training by calling `wandb.finish()`.

{% tabs %}
{% tab title="Notebook" %}
```python
trainer.train()  # start training and logging to W&B

# post-training analysis, testing, other logged code

wandb.finish()
```
{% endtab %}
{% endtabs %}

### 4) Visualize your results

Once you have logged your training results you can explore your results dynamically in the[ W\&B Dashboard](../track/app.md). It's easy to compare across dozens of runs at once, zoom in on interesting findings, and coax insights out of complex data with flexible, interactive visualizations.

![](<../../.gitbook/assets/hf-gif-15 (2) (2) (3) (3) (3) (1) (1) (1) (1) (1) (3) (1) (1) (1) (1) (1) (1) (1) (1) (1) (6) (1) (3) (1) (1) (2) (8).gif>)

## Advanced features

### **Turn on model versioning**

Using [Weights & Biases' Artifacts](https://docs.wandb.ai/artifacts), you can store up to 100GB of models and datasets. Logging your Hugging Face model to W\&B Artifacts can be done by setting a W\&B environment variable called `WANDB_LOG_MODEL` to `true`.

{% tabs %}
{% tab title="Notebook" %}
```python
%env WANDB_LOG_MODEL=true
```
{% endtab %}

{% tab title="Command Line" %}
```python
WANDB_LOG_MODEL=true
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Your model will be saved to W\&B Artifacts as `run-{run_name}`.
{% endhint %}

Any `Trainer` you initialize from now on will upload models to your W\&B project. Your model file will be viewable through the W\&B Artifacts UI. See the [Weights & Biases' Artifacts guide](https://docs.wandb.ai/artifacts) for more about how to use Artifacts for model and dataset versioning.

#### **How do I save the best model?**

If `load_best_model_at_end=True` is passed to `Trainer`, then W\&B will save the best performing model checkpoint to Artifacts instead of the final checkpoint.

### Loading a saved model

If you saved your model to W\&B Artifacts with `WANDB_LOG_MODEL`, you can download your model weights for additional training or to run inference. You just load them back into the same Hugging Face architecture that you used before.

```python
# Create a new run
with wandb.init(project="amazon_sentiment_analysis") as run:

  # Connect an Artifact to the run
  my_model_name = "run-bert-base-high-lr:latest"
  my_model_artifact = run.use_artifact(my_model_name)

  # Download model weights to a folder and return the path
  model_dir = my_model_artifact.download()

  # Load your Hugging Face model from that folder
  #  using the same model class
  model = AutoModelForSequenceClassification.from_pretrained(
      model_dir, num_labels=num_labels)

  # Do additional training, or run inference
```

### Additional W\&B settings

Further configuration of what is logged with `Trainer` is possible by setting environment variables. A full list of W\&B environment variables [can be found here](https://docs.wandb.ai/library/environment-variables).

| Environment Variable | Usage                                                                                                                                                                                                                                                                                                  |
| -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `WANDB_PROJECT`      | Give your project a name                                                                                                                                                                                                                                                                               |
| `WANDB_LOG_MODEL`    | Log the model as artifact at the end of training (`false` by default)                                                                                                                                                                                                                                  |
| `WANDB_WATCH`        | <p>Set whether you'd like to log your models gradients, parameters or neither</p><ul><li><code>gradients</code>: Log histograms of the gradients (default)</li><li><code>all</code>: Log histograms of gradients and parameters</li><li><code>false</code>: No gradient or parameter logging</li></ul> |
| `WANDB_DISABLED`     | Set to `true` to disable logging entirely (`false` by default)                                                                                                                                                                                                                                         |
| `WANDB_SILENT`       | Set to `true` to silence the output printed by wandb (`false` by default)                                                                                                                                                                                                                              |

{% tabs %}
{% tab title="Notebook" %}
```python
%env WANDB_WATCH=all
%env WANDB_SILENT=true
```
{% endtab %}

{% tab title="Command Line" %}
```bash
WANDB_WATCH=all
WANDB_SILENT=true
```
{% endtab %}
{% endtabs %}

### Customize `wandb.init`

The `WandbCallback` that `Trainer` uses will call `wandb.init` under the hood when `Trainer` is initialized. You can alternatively set up your runs manually by calling `wandb.init` before the`Trainer` is initialized. This gives you full control over your W\&B run configuration.

An example of what you might want to pass to `init` is below. For more details on how to use `wandb.init`, [check out the reference documentation](../../ref/python/init.md).

```python
wandb.init(project="amazon_sentiment_analysis", 
           name="bert-base-high-lr",
           tags=["baseline", "high-lr"],
           group="bert")
```

### Custom logging

Logging to Weights & Biases via the [Transformers `Trainer` ](https://huggingface.co/transformers/main\_classes/trainer.html)is taken care of by the `WandbCallback` ([reference documentation](https://huggingface.co/transformers/main\_classes/callback.html#transformers.integrations.WandbCallback)) in the Transformers library. If you need to customize your Hugging Face logging you can modify this callback.

## Issues, questions, feature requests

For any issues, questions, or feature requests for the Hugging Face W\&B integration, feel free to post in [this thread on the Hugging Face forums](https://discuss.huggingface.co/t/logging-experiment-tracking-with-w-b/498) or open an issue on the Hugging Face [Transformers GitHub repo](https://github.com/huggingface/transformers).
