---
description: >-
  Catalog and version models, track metadata and lineage, promote the best
  models to production, and report on evaluation analytics.
---

# Model Management Walkthrough

![](<../../.gitbook/assets/Screen Shot 2022-06-21 at 10.22.27 AM.png>)

In this walkthrough you'll learn how to use Weights & Biases for Model Management. Track, visualize, and report on the complete production model workflow.

1. **Model Versioning**: Save and restore every version of your model & learned parameters - organize versions by use case and objective. Track training metrics, assign custom metadata, and document rich markdown descriptions of your models.
2. **Model Lineage:** Track the exact code, hyperparameters, & training dataset used to produce the model. Enable model reproducibility.
3. **Model Lifecycle:** Promote promising models to positions like "staging" or "production" - allowing downstream users to fetch the best model automatically. Communicate progress collaboratively in Reports.

_We are actively building new Model Management features. Please reach out with questions or suggestions at support@wandb.com._

{% hint style="info" %}
Please see the[ Artifact Tab](https://docs.wandb.ai/ref/app/pages/project-page#artifacts-tab) details for a discussion of all content available in the Model Registry!
{% endhint %}

## Workflow

Now we will walk through a canonical workflow for producing, organizing, and consuming trained models:

1. [Create a new Model Collection](walkthrough.md#1.-create-a-new-model-portfolio)
2. [Train & log Model Versions](walkthrough.md#2.-train-and-log-model-versions)
3. [Link Model Versions to the Collection](walkthrough.md#3.-link-model-versions-to-the-portfolio)
4. [Using a Model Version](walkthrough.md#4.-use-a-model-version)
5. [Evaluate Model Performance](walkthrough.md#5.-evaluate-model-performance)
6. [Promote a Version to Production](walkthrough.md#6.-promote-a-version-to-production)
7. [Use the Production Model for Inference](walkthrough.md#7.-consume-the-production-model)
8. [Build a Reporting Dashboard](walkthrough.md#8.-build-a-reporting-dashboard)

{% hint style="success" %}
**A** [**companion colab notebook**](https://colab.research.google.com/drive/1wjgr9AHICOa3EM1Ikr\_Ps\_MAm5D7QnCC) **is provided which covers step 2-3 in the first code block and steps 4-6 in the second code block.**
{% endhint %}

![](<../../.gitbook/assets/Screen Shot 2022-06-21 at 10.24.10 AM.png>)

### 1. Create a new Model Collection

First, create a Model Collection to hold all the candidate models for your particular modeling task. In this tutorial, we will use the classic [MNIST Dataset](https://pytorch.org/vision/stable/generated/torchvision.datasets.MNIST.html#torchvision.datasets.MNIST) - 28x28 grayscale input images with output classes from 0-9. The video below demonstrates how to create a new Collection:1.&#x20;

{% tabs %}
{% tab title="Using Model Registry" %}
1\. Visit your Model Registry at [wandb.ai/registry/model](https://wandb.ai/registry/model) (linked from homepage).

![](<../../.gitbook/assets/Screen Shot 2022-06-21 at 10.14.10 AM.png>)

![](<../../.gitbook/assets/Screen Shot 2022-06-21 at 10.18.28 AM.png>)

2\. Click the `Create Model Collection` button at the top of the Model Registry.

![](<../../.gitbook/assets/Screen Shot 2022-06-21 at 10.17.24 AM.png>)

3\. Select `Type: model`, `Style: Collection`, and enter a name. In our case `MNIST Grayscale 28x28`. Remember, a Collection should map to a modeling task - enter a unique name that describes the use case.

![](<../../.gitbook/assets/Screen Shot 2022-06-21 at 10.20.23 AM.png>)
{% endtab %}

{% tab title="Using Artifact Browser" %}
1. Visit your Project's Artifact Browser: `wandb.ai/<entity>/<project>/artifacts`
2. Click the `+` icon on the bottom of the Artifact Browser Sidebar
3. Select `Type: model`, `Style: Collection`, and enter a name. In our case `MNIST Grayscale 28x28`. Remember, a Collection should map to a modeling task - enter a unique name that describes the use case.

![](<../../.gitbook/assets/2022-05-17 14.20.36.gif>)


{% endtab %}
{% endtabs %}



### 2. Train & log Model Versions

Next, you will log a model from your training script:

1. (Optional) Declare your dataset as a dependency so that it is tracked for reproducibility and audibility
2. **Serialize** your model to disk periodically (and/or at the end of training) using the serialization process provided by your modeling library (eg [PyTorch](https://pytorch.org/tutorials/beginner/saving\_loading\_models.html) & [Keras](https://www.tensorflow.org/guide/keras/save\_and\_serialize)).
3. **Add** your model files to an Artifact of type "model"
   * Note: We use the name `f'mnist-nn-{wandb.run.id}'`. While not required, it is advisable to name-space your "draft" Artifacts with the Run id in order to stay organized
4. **Log** your model
   * Note: If you are logging multiple versions, it is advisable to add an alias of "best" to your Model Version when it outperforms the prior versions. This will make it easy to find the model with peak performance - especially when the tail end of training may overfit!
5. (Optional) Log training metrics associated with the performance of your model during training.
   * Note: The data logged immediately after logging your Model Version will automatically be associated with that version.

By default, you should use the native W\&B Artifacts API to log your serialized model. However, since this pattern is so common, we have provided a single method which combines serialization, Artifact creation, and logging. See the "(Beta) Using `log_model`" tab for details.

{% tabs %}
{% tab title="Using Artifacts" %}
```python
import wandb

# Always initialize a W&B run to start tracking
wandb.init()

# (Optional) Declare an upstream dataset dependency
# see the `Declare Dataset Dependency` tab for
# alternative examples.
dataset = wandb.use_artifact("mnist:latest")

# At the end of every epoch (or at the end of your script)...
# ... Serialize your model
model.save("path/to/model.pt")
# ... Create a Model Version
art = wandb.Artifact(f'mnist-nn-{wandb.run.id}', type="model")
# ... Add the serialized files
art.add_file("path/to/model.pt", "model.pt")
# ... Log the Version
if model_is_best:
    # If the model is the best model so far, add "best" to the aliases
    wandb.log_artifact(art, aliases=["latest", "best"])
else:
    wandb.log_artifact(art)
    
# (optional) Log training metrics
wandb.log({"train_loss": 0.345, "val_loss": 0.456})
```
{% endtab %}

{% tab title="(Beta) Using `log_model`" %}
{% hint style="warning" %}
The following code snippet leverages actively developed `beta` APIs and therefore is subject to change and not guaranteed to be backwards compatible.
{% endhint %}

```python
from wandb.beta.workflows import log_model

# (Optional) Declare an upstream dataset dependency
# see the `Declare Dataset Dependency` tab for
# alternative examples.
dataset = wandb.use_artifact("mnist:latest")

# This one method will serialize the model, start a run, create a version
# add the files to the version, and log the version. You can override
# the default name, project, aliases, metadata, and more!
log_model(model, "mnist-nn", aliases=["best"] if model_is_best else [])

# (optional) Log training metrics
wandb.log({"train_loss": 0.345, "val_loss": 0.456})
```

Note: you may want to define custom serialization and deserialization strategies. You can do so by subclassing the [`_SavedModel` class](https://github.com/wandb/wandb/blob/9dfa60b14599f2716ab94dd85aa0c1113cb5d073/wandb/sdk/data\_types/saved\_model.py#L73), similar to the [`_PytorchSavedModel` class](https://github.com/wandb/wandb/blob/9dfa60b14599f2716ab94dd85aa0c1113cb5d073/wandb/sdk/data\_types/saved\_model.py#L358). All subclasses will automatically be loaded into the serialization registry. _As this is a beta feature, please reach out to support@wandb.com with questions or comments._
{% endtab %}

{% tab title="Declare Dataset Dependency" %}
If you would like to track your training data, you can declare a dependency by calling `wandb.use_artifact` on your dataset. Here are 3 examples of how you can declare a dataset dependency:

***

**Dataset stored in W\&B**

```python
dataset = wandb.use_artifact("[[entity/]project/]name:alias")
```

***

**Dataset stored on Local Filesystem**

```python
art = wandb.Artifact("dataset_name", "dataset")
art.add_dir("path/to/data") # or art.add_file("path/to/data.csv")
dataset = wandb.use_artifact(art)
```

***

**Dataset stored on Remote Bucket**

```python
art = wandb.Artifact("dataset_name", "dataset")
art.add_reference("s3://path/to/data")
dataset = wandb.use_artifact(art)
```
{% endtab %}
{% endtabs %}

After logging 1 or more Model Versions, you will notice that your will have a new Model Artifact in your Artifact Browser. Here, we can see the results of logging 5 versions to an artifact named `mnist_nn-1r9jjogr`.

![](<../../.gitbook/assets/Screen Shot 2022-06-21 at 10.25.13 AM.png>)

If you are following along the example notebook, you should see a Run Workspace with charts similar to the image below

![](<../../.gitbook/assets/Screen Shot 2022-05-12 at 11.42.12 AM.png>)

### 3. Link Model Versions to the Collection

Now, let's say that we are ready to link one of our Model Versions to the Model Collection. We can accomplish this manually as well as via an API.

{% tabs %}
{% tab title="Manual Linking" %}
The following video below demonstrates how to manually link a Model Version to your newly created Collection:

1. Navigate to the Model Version of interest
2. Click the link icon
3. Select the target Collection
4. (optional): Add additional aliases

![](<../../.gitbook/assets/2022-05-11 15.13.48.gif>)
{% endtab %}

{% tab title="Programatic Linking" %}
While manual linking is useful for one-off Models, it is often useful to programmatically link Model Versions to a Collection - consider a nightly job or CI pipeline that wants to link the best Model Version from every training job. Depending on your context and use case, you may use one of 3 different linking APIs:

**Fetch Model Artifact from Public API:**

```python
import wandb

# Fetch the Model Version via API
art = wandb.Api().artifact(...)

# Link the Model Version to the Model Collection
art.link("[[entity/]project/]collectionName")
```

**Model Artifact is "used" by the current Run:**

```python
import wandb

# Initialize a W&B run to start tracking
wandb.init()

# Obtain a reference to a Model Version
art = wandb.use_artifact(...)

# Link the Model Version to the Model Collection
art.link("[[entity/]project/]collectionName")
```

**Model Artifact is logged by the current Run:**

```python
import wandb

# Initialize a W&B run to start tracking
wandb.init()

# Create an Model Version
art = wandb.Artifact(...)

# Log the Model Version
wandb.log_artifact(art)

# Link the Model Version to the Collection
wandb.run.link_artifact(art, "[[entity/]project/]collectionName")
```
{% endtab %}

{% tab title="(Beta) Using `link_model`" %}
{% hint style="warning" %}
The following code snippet leverages actively developed `beta` APIs and therefore is subject to change and not guaranteed to be backwards compatible.
{% endhint %}

In the case that you logged a model with the beta `log_model` discussed above, then you can use it's companion method: `link_model`

```python
from wandb.beta.workflows import log_model, link_model

# Obtain a Model Version
model_version = wb.log_model(model, "mnist_nn")

# Link the Model Version
link_model(model_version, "[[entity/]project/]collectionName")
```
{% endtab %}
{% endtabs %}

After you link the Model Version, you will see hyperlinks connecting the Version in the Collection to the source Artifact and visa versa.

![](../../.gitbook/assets/13\_edit.png)

### 4. Use a Model Version

Now we are ready to consume a Model - perhaps to evaluate its performance, make predictions against a dataset, or use in a live production context. Similar to logging a Model, you may choose to use the raw Artifact API or the more opinionated beta APIs.

{% tabs %}
{% tab title="Using Artifacts" %}
You can load in a Model Version using the `use_artifact` method.

```python
import wandb

# Always initialize a W&B run to start tracking
wandb.init()

# Download your Model Version files
path = wandb.use_artifact("[[entity/]project/]collectionName:latest").download()

# Deserialize your model (this will be your custom code to load in
# a model from disk - reversing the serialization process used in step 2)
model = make_model_from_data(path)
```
{% endtab %}

{% tab title="(Beta) Using `use_model`" %}
{% hint style="warning" %}
The following code snippet leverages actively developed `beta` APIs and therefore is subject to change and not guaranteed to be backwards compatible.
{% endhint %}

Directly manipulating model files and handling deserialization can be tricky - especially if you were not the one who serialized the model. As a companion to `log_model`, `use_model` automatically deserializes and reconstructs your model for use.

```python
from wandb.beta.workflows import use_model

model = use_model("[[entity/]project/]collectionName").model_obj()
```
{% endtab %}
{% endtabs %}

### 5. Evaluate Model Performance

After training many Models, you will likely want to evaluate the performance of those models. In most circumstances you will have some held-out data which serves as a test dataset, independent of the dataset your models have access to during training. To evaluate a Model Version, you will want to first complete step 4 above to load a model into memory. Then:

1. (Optional) Declare a data dependency to your evaluation data
2. **Log** metrics, media, tables, and anything else useful for evaluation

```python
# ... continuation from 4

# (Optional) Declare an upstream evaluation dataset dependency
dataset = wandb.use_artifact("mnist-evaluation:latest")

# Evaluate your model in whatever way makes sense for your
loss, accuracy, predictions = evaluate_model(model, dataset)

# Log out metrics, images, tables, or any data useful for evaluation.
wandb.log({"loss": loss, "accuracy": accuracy, "predictions": predictions})
```

If you are executing similar code, as demonstrated in the notebook, you should see a workspace similar to the image below - here we even show model predictions against the test data!

![](<../../.gitbook/assets/Screen Shot 2022-05-12 at 11.45.09 AM.png>)

### 6. Promote a Version to Production

Next, you will likely want to denote which version in the Collection is intended to be used for Production. Here, we use the concept of aliases. Each Collection can have any aliases which make sense for your use case - however we often see `production` as the most common alias. Each alias can only be assigned to a single Version at a time.

{% tabs %}
{% tab title="via UI Interface" %}
![](<../../.gitbook/assets/Screen Shot 2022-06-06 at 7.50.27 AM.png>)
{% endtab %}

{% tab title="via API" %}
Follow steps in [Part 3. Link Model Versions to the Collection](walkthrough.md#3.-linking-model-versions-to-the-portfolio) and add the aliases you want to the `aliases` parameter.
{% endtab %}
{% endtabs %}

The image below shows the new `production` alias added to v1 of the Collection!

![](<../../.gitbook/assets/Screen Shot 2022-05-12 at 11.46.43 AM.png>)

### 7. Consume the Production Model

Finally, you will likely want to use your production Model for inference. To do so, simply follow the steps outlined in [Part 4. Using a Model Version](walkthrough.md#4.-evaluate-model-performance), with the `production` alias. For example:

```python
wandb.use_artifact("[[entity/]project/]collectionName:production")
```

You can reference a Version within the Collection using different alias strategies:

* `latest` - which will fetch the most recently linked Version
* `v#` - using `v0`, `v1`, `v2`, ... you can fetch a specific version in the Collection
* `production` - you can use any custom alias that you and your team have assigned

### 8. Build a Reporting Dashboard

Using Weave Panels, you can display any of the Model Registry/Artifact views inside of Reports! See a [demo here](https://wandb.ai/timssweeney/model\_management\_docs\_official\_v0/reports/MNIST-Grayscale-28x28-Model-Dashboard--VmlldzoyMDI0Mzc1). Below is a full-page screenshot of an example Model Dashboard.

![](<../../.gitbook/assets/Screenshot 2022-06-21 at 10-42-44 Weights & Biases.png>)
