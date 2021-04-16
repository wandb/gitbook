---
description: >-
  W&B integration with the awesome NLP library Hugging Face, which has
  pre-trained models, scripts, and datasets
---

# Hugging Face

A Weights & Biases integration for Hugging Face's [Transformers library](https://github.com/huggingface/transformers): solving NLP, one logged run at a time!

{% hint style="warning" %}
[**Click here**](https://discuss.huggingface.co/t/weights-biases-supporting-wave2vec2-finetuning/4839) **to see how Weights & Biases can help you with the Hugging Face Wav2vec2-XLSR Community Challenge or check out our** [**XLSR colab**](https://colab.research.google.com/drive/1oqMZAtRYkurKqePGxFKpnU9n6L8Qk9hM?usp=sharing) **to see a full, end to end example of using Weights & Biases with Hugging Face**
{% endhint %}

## Just show me the code!

* Sure, here you go:  [**W&B and Hugging Face Google Colab Demo**](http://wandb.me/hf)\*\*\*\*

### Quick and Easy Hugging Face Model Tracking with Weights & Biases

Below is an example comparing [BERT vs DistilBERT](https://app.wandb.ai/jack-morris/david-vs-goliath/reports/Does-model-size-matter%3F-Comparing-BERT-and-DistilBERT-using-Sweeps--VmlldzoxMDUxNzU) — it's easy to see how different architectures effect the evaluation accuracy throughout training with automatic line plot visualizations. To see how easy it is to track and save your own Hugging Face models, keep reading!

![](../../.gitbook/assets/gif-for-comparing-bert.gif)

Above is an example comparing [BERT vs DistilBERT](https://app.wandb.ai/jack-morris/david-vs-goliath/reports/Does-model-size-matter%3F-Comparing-BERT-and-DistilBERT-using-Sweeps--VmlldzoxMDUxNzU) — it's easy to see how different architectures effect the evaluation accuracy throughout training with automatic line plot visualizations. To see how easy it is to track and save your own Hugging Face models, keep reading!

## In This Guide

This section covers:

* **Example notebooks** using W&B and Hugging Face Transformers
* **Getting Started**: Track and Save your Models with  W&B and Transformers 
* **Advanced W&B settings**
  * Additional Settings 
  * Customising wandb.init

## Example Notebooks

We've created a few examples for you to see how the W&B integration works:

* \*\*\*\*[**W&B and Hugging Face demo**](http://wandb.me/hf) ****in Google Colab with model logging
* [**Huggingtweets** ](https://wandb.ai/wandb/huggingtweets/reports/HuggingTweets-Train-a-Model-to-Generate-Tweets--VmlldzoxMTY5MjI)- Train a GPT-2 Hugging Face model to generate tweets
* [**Does model size matter?**](https://app.wandb.ai/jack-morris/david-vs-goliath/reports/Does-model-size-matter%3F-A-comparison-of-BERT-and-DistilBERT--VmlldzoxMDUxNzU) - A comparison of BERT and DistilBERT

## Getting Started: Track and Save your Models 

### Under the Hood

W&B logging can be set up by passing just a few arguments to the Transformers `Trainer` class. Under the hood, `Trainer` uses `WandbCallback` , which will take care of all our logging, but `WandbCallback` won't need to be modified at all for most use cases.  Note that the steps below work for both for Hugging Face Transformers' pytorch `Trainer` and tensorflow `TFTrainer`

### **1\)** **Install the `wandb`Library and Log-In**

_If you are using a python script:_

```text
pip install wandb
wandb login
```

_Or if you are using a Jupyter or Google Colab notebook:_

```python
!pip install wandb

import wandb
wandb.login()
```

### **2\) Name Your Project**

A project is where all of the charts, data and models logged from your runs is stored. Naming your project helps you organise your work and keep all your runs related to a single project in one place. 

To add a run to a project simply set the `WANDB_PROJECT` environment variable to the name of your project. The `WandbCallback` will pick up this project name environment variable and use it when setting up your run.

_If using a python script, set the following before initialising_  `Trainer`_:_

```python
import os
os.environ['WANDB_PROJECT'] = 'amazon_sentiment_analysis'
```

_If you are using a Jupyter or Google Colab notebook, set the following before initialising_  `Trainer`_:_

```text
%env WANDB_PROJECT = amazon_sentiment_analysis
```

If a project name is not specified the project name defaults to "huggingface"

### **3\) \[Optional\] Save Model Weights to W&B Flag**

Using [Weights & Biases' Artifacts](https://docs.wandb.ai/artifacts) you can use up to 100GB of storage for free to store your Models or Datasets. Logging your Hugging Face model to W&B Artifacts can be done by setting a W&B environment variable which will then be picked up by the `WandbCallback` that `Trainer` uses. You can later easily reload this model if you wanted to do additional inference or do additional training.

**NOTE**: Your model will be saved to W&B Artifacts as "run-run\_name". `run_name`  will be specified in the training arguments section next.

_If using a python script, set the following before initialising_  `Trainer`_:_

```python
os.environ['WANDB_LOG_MODEL'] = 'true'
```

_If using a Notebook, set the following before initialising_  `Trainer`_:_

```python
%env WANDB_LOG_MODEL = true 
```

Now when training using `Trainer` your trained model will be uploaded to your W&B project. Your model file will be viewable through the W&B Artifacts UI.  See the [Weights & Biases' Artifacts](https://docs.wandb.ai/artifacts) documentation for more about how to use Artifacts for model and dataset versioning.

#### **Save Best Model**

If `load_best_model_at_end = True` is passed to `Trainer` then W&B will save the best performing model checkpoint to Artifacts instead of the final checkpoint.

### **4\)** Turn On Logging with W&B

**Set** `report_to`**and** `run_name` **in the Training Arguments**

When defining your `Trainer` training arguments, set **`report_to`** to **`"wandb"`** in order enable logging with Weights & Biases.

**\[Optional\]** You can also pass a **name** to the training run that W&B will log using the **`run_name`** argument. If you are saving your model to W&B artifacts your saved model will also be given this name. If not specified then a machine generated name will be given to your run, which you can later rename in the W&B workspace.

_If using a python script:_

```text
python run_glue.py \
  --report_to wandb \    # enable logging to W&B
  --run_name bert-base-high-lr \    # name of the W&B run (optional)
  ...
```

_Or if you are using a Jupyter or Google Colab notebook:_

```python
from transformers import TrainingArguments, Trainer

args = TrainingArguments(
    ...
    report_to = 'wandb',        # enable logging to W&B
    run_name = 'bert-base-high-lr'    # name of the W&B run (optional)
)

trainer = Trainer(
    ...
    args = args,    # your training args
)

trainer.train()    # start training and logging to W&B
```

### **5\) Train**

You're models will now train while logging losses, evaluation metrics, model topology and gradients to Weights & Biases!

### 6\) Finish Your W&B Run \(Notebook only\)

If you are using a Jupyter or Google Colab notebook, your training run is finished and you do not want to log any more to your run it is a good idea to "finish" the W&B run. Calling `wandb.finish()` will close the W&B python process. This should also be done if you would like to start another, new run \(after modifying some hyperparameters for example\).

```python
wandb.finish() 
```

### 7 \[Optional\] Loading a Saved Model

If you saved your model to W&B Artifacts with `WANDB_LOG_MODEL` you can download your model weights for additional training or to run inference on it. You can load it back into the same Hugging Face architecture that you used before like so:

If your original model was first created using Hugging Face's `AutoModelForSequenceClassification` class for example, then that same class can be used to re-load your trained model weights that you saved in Artifacts into.

```python
# Example: Loading a Hugging Face model, before training
model = AutoModelForSequenceClassification.from_pretrained(
                'distilbert-base-uncased', 
                num_labels=num_labels)

# Do Training + save model to Artifacts
...
```

Then when you would like to use the trained model in a new run, you can load from Artifacts like so:

```python
# Make sure you are logged in to wandb first
wandb.login()

# Create a run object in your project
run = wandb.init(project="amazon_sentiment_analysis")

# Connect an Artifact to your run
my_model_artifact = run.use_artifact('run-bert-base-high-lr:v0')

# Download model weights to a folder and return the path
model_dir = my_model_artifact.download()

# Load your Hugging Face model from that folder, e.g. SequenceClassification model
model = AutoModelForSequenceClassification.from_pretrained(
                model_dir, 
                num_labels=num_labels)

# Do additional training, or run inference 
...

# When you are finished with your run (notebook only)
run.finish()
```

See the  [Weights & Biases' Artifacts](https://docs.wandb.ai/artifacts) documentation for more details on using and versioning models and datasets with Artifacts

## Advanced

### Additional W&B Settings

Advanced configuration of what is logged with `Trainer` is possible by setting environment variables, see code example below for how to do this. 

<table>
  <thead>
    <tr>
      <th style="text-align:left">Environment Variable</th>
      <th style="text-align:left">Options</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">WANDB_PROJECT</td>
      <td style="text-align:left">Give your project a name</td>
    </tr>
    <tr>
      <td style="text-align:left">WANDB_LOG_MODEL</td>
      <td style="text-align:left">Log the model as artifact at the end of training (<b>false</b> by default)</td>
    </tr>
    <tr>
      <td style="text-align:left">WANDB_WATCH</td>
      <td style="text-align:left">
        <p>Set whether you&apos;d like to log your models gradients, parameters or
          neither</p>
        <ul>
          <li><b>gradients</b> (default): Log histograms of the gradients</li>
          <li><b>all</b>: Log histograms of gradients and parameters</li>
          <li><b>false</b>: No gradient or parameter logging</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">WANDB_DISABLED</td>
      <td style="text-align:left">Set to <b>true</b> to disable logging entirely (<b>false</b> by default)</td>
    </tr>
    <tr>
      <td style="text-align:left">WANDB_SILENT</td>
      <td style="text-align:left">Set to <b>true </b>to silence the output printed by wandb (<b>false</b> by
        default)</td>
    </tr>
  </tbody>
</table>

A full list of W&B environment variables [**can be found here**](https://docs.wandb.ai/library/environment-variables). 

#### Examples: Setting Environment Variables

_If using a python script or Notebook, set the following before initialising your_ `Trainer`_:_

```python
import os
os.environ['WANDB_WATCH'] = 'all'
os.environ['WANDB_SILENT'] = 'true'
```

_If using a Notebook, set the following before initialising your_ `Trainer`_:_

```python
%env WANDB_WATCH = 'all'
%env WANDB_SILENT = 'true'
```

### Custom wandb Init

The `WandbCallback` that `Trainer` uses will call `wandb.init` under the hood when `Trainer` is initialised. You can alternatively set up your runs manually by calling `wandb.init(**optional_args)` before `Trainer` is initialised. 

An example of arguments you might want to pass to init is below.  The full list of arguments for `wandb.init` [can be found here](https://docs.wandb.ai/ref/run/init).

```python
wandb.init(project="amazon_sentiment_analysis", 
                name = "bert-base-high-lr",
                tags = ['baseline','high-lr'],
                group = "bert")
                
```

## Issues, Questions, Feature Requests

For any issues, question or feature request with this Hugging Face W&B integration, feel free to post  ****[**in this thread**](https://discuss.huggingface.co/t/logging-experiment-tracking-with-w-b/498) on the Hugging Face forums or open an issue on the Hugging Face [**Transformers github repo**](%20https://github.com/huggingface/transformers%20%20%20)\*\*\*\*

## Visualize Results

Once you have logged your training results you can explore your results dynamically in the W&B Dashboard. It's easy to look across dozens of experiments, zoom in on interesting findings, and visualize highly dimensional data.

![](../../.gitbook/assets/hf-gif-15%20%282%29%20%282%29%20%283%29%20%283%29%20%283%29%20%281%29%20%283%29.gif)



