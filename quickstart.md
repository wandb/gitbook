---
description: Start tracking machine learning experiments in 5 minutes
---

# Quickstart

Build better models more efficiently with Weights & Biases experiment tracking.

### [Run a quick example project →](http://wandb.me/intro)

Try this short Google Colab to see Weights & Biases in action, no code installation required!

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](http://wandb.me/intro)

![](<.gitbook/assets/wandb demo - experiments gif.gif>)

### 1. Set up wandb

[Sign up](https://wandb.ai/site) for a free account, then from the command line install the wandb library in a Python 3 environment. To login, you'll need to be signed in to you account at www.wandb.ai, then you will find your API key on the [Authorize page](https://wandb.ai/authorize).

{% tabs %}
{% tab title="Command Line" %}
```
pip install wandb

wandb login
```
{% endtab %}

{% tab title="Notebook" %}
```python
!pip install wandb

wandb.login()
```
{% endtab %}
{% endtabs %}

### 2. Start a new run

Initialize a new run in W\&B in your Python script or notebook. `wandb.init()` will start tracking system metrics and console logs, right out of the box. Run your code, put in [your API key](https://wandb.ai/authorize) when prompted, and you'll see the new run appear in W\&B.[\
More about wandb.init() →](guides/track/launch.md)

```python
import wandb
wandb.init(project="my-awesome-project")
```

### 3. Track metrics

Use `wandb.log()` to track metrics or a framework [integration](guides/integrations/) for easy instrumentation.\
[More about wandb.log() →](guides/track/log/)

```python
wandb.log({'accuracy': train_acc, 'loss': train_loss})
```

![](<.gitbook/assets/wandb demo - logging metrics.png>)

### 4. Track hyperparameters

Save hyperparameters so you can quickly compare experiments.\
[More about wandb.config →](guides/track/config.md)

```python
wandb.config.dropout = 0.2
```

![](<.gitbook/assets/wandb demo - logging config.png>)

### 5. Get alerts

Get notified via Slack or email if your W\&B Run has crashed or whether a custom trigger, such as your loss going to NaN or a step in your ML pipeline has completed, has been reached. See the [Alerts docs](https://docs.wandb.ai/guides/track/alert) for a full setup.

[More about wandb.alert() →](https://docs.wandb.ai/guides/track/alert)

1. Turn on Alerts in your W\&B [User Settings](https://wandb.ai/settings)
2. Add `wandb.alert()` to your code

```python
wandb.alert(
    title="Low accuracy", 
    text=f"Accuracy {acc} is below the acceptable threshold {thresh}"
)
```

Then see W\&B Alerts messages in Slack (or your email):

![W\&B Alerts in a Slack channel](<.gitbook/assets/Screenshot 2022-02-17 at 16.26.15 (1).png>)

## What next?

1. [**Collaborative Reports**](guides/reports/): Snapshot results, take notes, and share findings
2. [**Data + Model Versioning**](guides/artifacts/): Track dependencies and results in your ML pipeline
3. [**Data Visualization**](guides/data-vis/)**:** Visualize and query datasets and model evaluations
4. [**Hyperparameter Tuning**](guides/sweeps/): Quickly automate optimizing hyperparameters
5. ****[**Private-Hosting**](guides/self-hosted/): The enterprise solution for private cloud or on-prem hosting of W\&B

## Common Questions

**Where do I find my API key?**\
Once you've signed in to www.wandb.ai, the API key will be on the [Authorize page](https://wandb.ai/authorize).

**How do I use W\&B in an automated environment?**\
If you are training models in an automated environment where it's inconvenient to run shell commands, such as Google's CloudML, you should look at our guide to configuration with [Environment Variables](guides/track/advanced/environment-variables.md).

**Do you offer local, on-prem installs?**\
Yes, you can [privately host W\&B](guides/self-hosted/) locally on your own machines or in a private cloud, try [this quick tutorial notebook](http://wandb.me/intro) to see how. Note, to login to wandb local server you can [set the host flag](https://docs.wandb.ai/guides/self-hosted/quickstart#4.-modify-training-code-to-log-to-wandb-local-server) to the address of the local instance.  **** &#x20;

**How do I turn off wandb logging temporarily?**\
If you're testing code and want to disable wandb syncing, set the environment variable [`WANDB_MODE=offline`](guides/track/advanced/environment-variables.md).
