---
description: >-
  Fine-tune OpenAI GPT-3 and other models on your own data and track your runs
  with W&B.
---

# \[Beta] OpenAI API

{% hint style="info" %}
**Beta Integration**: This is a new feature, and we're actively working on making this better. Please reach out if you have any feedback — contact@wandb.com
{% endhint %}

OpenAI provides an [API to fine-tune GPT-3](https://beta.openai.com/docs/guides/fine-tuning) and obtain better inference results than with prompt engineering.

The W\&B integration adds rich & flexible experiment tracking as well as model & dataset versioning through interactive centralized dashboards.

## :sparkles: Check out interactive examples

* [Demo Colab](https://colab.research.google.com/drive/1Tr9dYlwBKk6-LgLKGO8KYZULnguVA992?usp=sharing)
* [Report - GPT-3 Exploration and Fine-Tuning Tips](https://wandb.ai/borisd13/gpt-3-backup/reports/GPT-3-exploration-fine-tuning-tips--VmlldzoxMTAxNDE2)

## :tada: Sync your fine-tune runs with one line!

You currently need to install a custom branch:

```shell-session
$ pip install --upgrade git+https://github.com/borisdayma/openai-python.git@feat-wandb
```

One line is all it takes to sync your runs to W\&B!

{% tabs %}
{% tab title="Command Line" %}
```shell-session
$ # one line command
$ openai wandb sync

$ # passing optional parameters
$ openai wandb sync --help
```
{% endtab %}

{% tab title="Python" %}
```python
from openai.logger import Logger

# one line command
Logger.sync()

# passing optional parameters
Logger.sync(
    id=None,
    n_jobs=None,
    project="GPT-3",
    entity=None,
    force=False,
    **kwargs_wandb_init
)
```
{% endtab %}
{% endtabs %}

We scan for new completed runs and automatically add them to your workspace!

![](<../../../.gitbook/assets/image (168).png>)

In addition your training & validation files are logged and versioned, as well as details of your fine-tune results.

This let you interactively explore your training/validation data.

![](<../../../.gitbook/assets/image (167).png>)

## :gear: Optional arguments

| Argument                      | Description                                                                                                               |
| ----------------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| -i ID, --id ID                | The id of the fine-tune job (optional)                                                                                    |
| -n N\_JOBS, --n\_jobs N\_JOBS | Number of most recent fine-tune jobs to log when an id is not provided. By default, every fine-tune is synced.            |
| --project PROJECT             | Name of the project where you're sending runs. By default, it is "GPT-3".                                                 |
| --entity ENTITY               | Username or team name where you're sending runs. By default, your default entity is used, which is usually your username. |
| --force                       | Forces logging and overwrite existing wandb run of the same finetune job.                                                 |
| \*\*kwargs\_wandb\_init       | In python, any additional argument is directly passed to [`wandb.init()`](../../../ref/python/init.md)``                  |

## 🔍 Inspect sample predictions

Use [Tables](../../data-vis/) to better visualize sample predictions and compare models.

![](<../../../.gitbook/assets/image (161).png>)

Create a new run:

```python
run = wandb.init(project="GPT-3", job_type="eval")
```

Retrieve a model id for inference.

You can use automatically logged artifacts to retrieve your latest model:

```python
artifact_job = run.use_artifact("ENTITY/PROJECT/job_details:latest")
fine_tuned_model = artifact_job.metadata["fine_tuned_model"]
```

You can also retrieve your validation file:

```python
artifact_valid = run.use_artifact("ENTITY/PROJECT/FILENAME:latest")
valid_file = artifact_valid.get_path("FILENAME").download()
```

Perform some inferences using OpenAI API:

```python
# perfom inference and record results
my_prompts = ["PROMPT_1", "PROMPT_2"]
results = []
for prompt in my_prompts:
    res = openai.Completion.create(model=fine_tuned_model,
                                   prompt=prompt,
                                   ...)
    results.append(res["choices"][0]["text"])
```

Log your results with a Table:

```python
table = wandb.Table(columns=['prompt', 'completion'],
                    data=list(zip(prompts, completions)))
```

## :question:Frequently Asked Questions

### How do I share runs with my team?

Sync all your runs to your team account with:

```shell-session
$ openai wandb sync --entity MY_TEAM_ACCOUNT
```

### How can I organize my runs?

Your runs are automatically organized and can be filtered/sorted based on any configuration parameter such as job type, base model, learning rate, training filename and any other hyper-parameter.

In addition, you can rename your runs, add notes or create tags to group them.

Once you’re satisfied, you can save your workspace and use it to create report, importing data from your runs and saved artifacts (training/validation files).

### How can I access my fine-tune job details?

Job files are logged to W\&B as artifacts and can be accessed with:

```python
import wandb

artifact_job = wandb.run.use_artifact('USERNAME/PROJECT/job_details:VERSION')
```

&#x20;where `VERSION` is either:

* a version number such as `v2`
* the fine-tune id such as `ft-xxxxxxxxx`
* an alias added automatically such as `latest` or manually

You can then access job details through `artifact_job.metadata`. For example, the fine-tuned model can be retrieved with `artifact_job.metadata[`"`fine_tuned_model"]`.

### What if a fine-tune run was not synced successfully?

You can always call again `openai wandb sync` and we will resync any run that was not synced successfully.

If needed, you can call `openai wandb sync --id fine_tune_id --force` to force resyncing a specific fine-tune run.

### Can I track my datasets with W\&B?

Yes, you can integrate your entire pipeline to W\&B through Artifacts, including creating your dataset, splitting it, training your models and evaluating them!

This will allow complete traceability of your models.

![](<../../../.gitbook/assets/image (165).png>)

## :books: Resources

* [OpenAI Fine-tuning Documentation](https://beta.openai.com/docs/guides/fine-tuning) is very thorough and contains many useful tips
* [Demo Colab](https://colab.research.google.com/drive/1xu2CDJdIybC\_65PRNeGP5ZmBbnY3PYDB?usp=sharing)
* [Report - GPT-3 Exploration & Fine-tuning Tips](https://wandb.ai/borisd13/gpt-3-backup/reports/GPT-3-exploration-fine-tuning-tips--VmlldzoxMTAxNDE2)
