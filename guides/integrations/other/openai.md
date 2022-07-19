---
description: Fine-tune OpenAI GPT-3 on your own data and track results with W&B
---

# OpenAI API

{% hint style="info" %}
**Beta Integration**: This is a new feature, and we're actively working on making this better. Please reach out if you have any feedback — contact@wandb.com
{% endhint %}

OpenAI’s API gives practitioners access to GPT-3, an incredibly powerful natural language model that can be applied to virtually any task that involves understanding or generating natural language.

If you use OpenAI's API to [fine-tune GPT-3](https://beta.openai.com/docs/guides/fine-tuning), you can now use the W\&B integration to track experiments, models, and datasets in your central dashboard.

![](<../../../.gitbook/assets/image (171) (1).png>)

All it takes is one line: `openai wandb sync`

## :sparkles: Check out interactive examples

* [Demo Colab](http://wandb.me/openai-colab)
* [Report - GPT-3 Exploration and Fine-Tuning Tips](http://wandb.me/openai-report)

## :tada: Sync your fine-tunes with one line!

Make sure you are using latest version of openai and wandb.

```shell-session
$ pip install --upgrade openai wandb
```

Then sync your results from the command line or from your script.

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
from openai.wandb_logger import WandbLogger

# one line command
WandbLogger.sync()

# passing optional parameters
WandbLogger.sync(
    id=None,
    n_fine_tunes=None,
    project="GPT-3",
    entity=None,
    force=False,
    **kwargs_wandb_init
)
```
{% endtab %}
{% endtabs %}

We scan for new completed fine-tunes and automatically add them to your dashboard.

![](<../../../.gitbook/assets/image (173) (1).png>)

In addition your training and validation files are logged and versioned, as well as details of your fine-tune results. This let you interactively explore your training and validation data.

![](<../../../.gitbook/assets/image (172).png>)

## :gear: Optional arguments

| Argument                 | Description                                                                                                               |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------------- |
| -i ID, --id ID           | The id of the fine-tune (optional)                                                                                        |
| -n N, --n\_fine\_tunes N | Number of most recent fine-tunes to log when an id is not provided. By default, every fine-tune is synced.                |
| --project PROJECT        | Name of the project where you're sending runs. By default, it is "GPT-3".                                                 |
| --entity ENTITY          | Username or team name where you're sending runs. By default, your default entity is used, which is usually your username. |
| --force                  | Forces logging and overwrite existing wandb run of the same fine-tune.                                                    |
| \*\*kwargs\_wandb\_init  | In python, any additional argument is directly passed to [`wandb.init()`](../../../ref/python/init.md)                    |

## 🔍 Inspect sample predictions

Use [Tables](../../data-vis/) to better visualize sample predictions and compare models.

![](<../../../.gitbook/assets/image (162).png>)

Create a new run:

```python
run = wandb.init(project="GPT-3", job_type="eval")
```

Retrieve a model id for inference.

You can use automatically logged artifacts to retrieve your latest model:

```python
artifact_job = run.use_artifact("ENTITY/PROJECT/fine_tune_details:latest")
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
                    data=list(zip(my_prompts, results)))
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

### How can I access my fine-tune details?

Fine-tune details are logged to W\&B as artifacts and can be accessed with:

```python
import wandb

artifact_job = wandb.run.use_artifact('USERNAME/PROJECT/job_details:VERSION')
```

where `VERSION` is either:

* a version number such as `v2`
* the fine-tune id such as `ft-xxxxxxxxx`
* an alias added automatically such as `latest` or manually

You can then access fine-tune details through `artifact_job.metadata`. For example, the fine-tuned model can be retrieved with `artifact_job.metadata[`"`fine_tuned_model"]`.

### What if a fine-tune was not synced successfully?

You can always call again `openai wandb sync` and we will re-sync any run that was not synced successfully.

If needed, you can call `openai wandb sync --id fine_tune_id --force` to force re-syncing a specific fine-tune.

### Can I track my datasets with W\&B?

Yes, you can integrate your entire pipeline to W\&B through Artifacts, including creating your dataset, splitting it, training your models and evaluating them!

This will allow complete traceability of your models.

![](<../../../.gitbook/assets/image (169).png>)

## :books: Resources

* [OpenAI Fine-tuning Documentation](https://beta.openai.com/docs/guides/fine-tuning) is very thorough and contains many useful tips
* [Demo Colab](http://wandb.me/openai-colab)
* [Report - GPT-3 Exploration & Fine-tuning Tips](http://wandb.me/openai-report)
