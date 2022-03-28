---
description: Guide to using Artifacts for model versioning
---

# Model Versioning

W\&B Artifacts help you save and organize machine learning models throughout a project's lifecycle.

### Common Use Cases

1. [**Version and store reliably**](model-versioning.md#version-and-store-reliably), transferring models across machines
2. [**Explore ideas in branches**](model-versioning.md#explore-ideas-in-branches), keeping different model ideas separate
3. [**Compare models precisely**](model-versioning.md#compare-models-precisely), across many variants
4. [**Manage a model ecosystem**](model-versioning.md#manage-a-model-ecosystem), even as the species multiply
5. [**Visualize & share your workflow**](model-versioning.md#visualize-and-easily-share-your-workflow), keeping all your work in one place

### Flexible tracking and hosting

Beyond these common scenarios, you can use core Artifact features to upload, version, alias, compare, and download models, supporting any custom model training and management process on local or remote filesystems, via S3, GCP, or https.

For more detail on these features, check out [Artifacts Core Concepts](artifacts-core-concepts.md).

## Core Artifacts features

W\&B Artifacts support model management through these basic features:

1. **Upload**: Save any model (as a directory or file in any format) with`run.log_artifact()`. You can also track datasets in a remote filesystem (e.g. cloud storage in S3 or GCP) [by reference](https://docs.wandb.ai/artifacts/api#adding-references), using a link or URI instead of the raw contents.
2. **Version**: Define an artifact by giving it a type (`"resnet50"`, `"bert"`, `"stacked_lstm"`) and a name (`"my_resnet50_variant_with_attention"`). When you log the same name again, W\&B automatically creates a new version of the artifact with the latest contents. You can use artifact versions to checkpoint models during training — just log a new model file to the same name at each checkpoint.
3. **Alias**: Set an alias like `"baseline"`, `"best"`, or `"production"` to highlight the important versions in a lineage of experiments and developed models.
4. **Compare**: Select any two versions to browse the contents side-by-side. We also have tools for visualizing model inputs and outputs, [learn more here →](https://docs.wandb.ai/datasets-and-predictions)
5. **Download**: Obtain a local copy of the model (e.g. for inference) or verify the artifact contents by reference.

## Version and store reliably

With automatic saving and versioning, each experiment you run stores the most recently trained model artifact to W\&B. You can scroll through all these model versions, annotating and renaming as necessary while maintaining the development history. Know exactly which experiment code and configuration generated which weights and architecture. You and your team can download and restore any of your model checkpoints—across projects, hardware, and dev environments.

Train a model on a local machine and log it as an artifact. Each training run will create a new version of the model named `inceptionV3`.

![](<../../.gitbook/assets/image (74).png>)

Load the same model by name for inference in another machine, e.g. via Google Colab, using the "latest" version to get the most recent one. You can also refer to any other version by index or other custom alias.

![](<../../.gitbook/assets/image (75).png>)

## Explore ideas in branches

To test a new hypothesis or start a set of experiments—say changing the core architecture or varying a key hyperparameter—create a new name and optionally artifact type for your model. Types could correspond to broader differences (`"cnn_model"` vs `"rnn_model"`, `"ppo_agent"` vs `"dqn_agent"`) while names could capture more detail (`"cnn_5conv_2fc"`, `"ppo_lr_3e4_lmda_0.95_y_0.97"`, etc). Checkpoint your model as versions of the artifact under the same name to easily organize and track your work. From your code or browser, you can associate individual checkpoints with descriptive notes or tags and access any experiment runs which use that particular model checkpoint (for inference, fine-tuning, etc). When creating the artifact, you can also upload associated metadata as a key-value dictionary (e.g. hyperparameter values, experiment settings, or longer descriptive text).

On any model version, you can take notes, add descriptive tags and arbitrary metadata, and view all the experiments which loaded in this version of the model.

![](<../../.gitbook/assets/image (76).png>)

A partial view of an artifact tree showing two versions of an Inception-based CNN, iv3. A model checkpoint is saved before starting training (with pre-existing ImageNet weights) and after finishing training (suffix \_trained). The rightmost nodes show various inference runs which loaded the iv3\_trained:v2 model checkpoint and the test data in inat\_test\_data\_10:v0 (bottom right).

![](<../../.gitbook/assets/image (77).png>)

A partial view of a complex artifact tree focusing on two training runs (prefixed train), named beyond roads iou 0.48 (top left square node) and fastai baseline (bottom left square node). Each experiment produces many artifacts: sample predictions of the model on training and validation images after every epoch. In the right half of the image, you can see some test runs (prefixed test) which load in the model checkpoints of training runs (out of visible frame) and store predictions on the test data as artifacts (prefixed test\_preds).

![](<../../.gitbook/assets/image (26) (11).png>)

## Compare models precisely

Compare your models by logged or derived metrics (e.g. loss, accuracy, mean intersection over union) or by their predictions on the same set of data (e.g. test or validation). You can visualize different model variants, trace their lineage, and ascertain they're using identical dataset versions via the Artifacts compute graph (first image below). You can select versions of an artifact to see notes you or a colleague left, dive deep into the details, chase the connections to compute runs and other artifacts (second image) or enter a visual side-by-side diff mode of model predictions with [Tables](../data-vis/). You can also use the W\&B workspace as a dashboard to organize and query the runs in your project, then locate the model artifacts associated with a particular run for download, fine-tuning, or further analysis (last image).

This artifact tree shows 12 model variants (bottom left), creating two sets of predictions from the test\_dataset: 14 entry\_predictions and 2 predictions. These are all evaluated to produce 19 result artifacts (computed metrics and ground truth annotations on images).

![](<../../.gitbook/assets/image (79).png>)

Select versions across names (here, model entries to a competitive benchmark from different teams) to browse details and connected experiment runs. You can compare contents side-by-side when you select two versions (check out [Tables](../data-vis/) for visual comparison).

![](<../../.gitbook/assets/image (80).png>)

Each experiment run visible in the workspace links to its associated artifacts. Find a particular run—here the top mean\_class\_iou by team name "Daenerys"—and download the corresponding model.

![](<../../.gitbook/assets/image (81).png>)

## Manage a model ecosystem

The artifacts graph records and makes traceable the evolution of your models across datasets, training and evaluation code repositories, projects, and teammates. To help organize the proliferation of models, you can

* **use aliases to designate particular models** as `"baseline"`, `"production"`, `"ablation"`, or any other custom tag, from the W\&B UI or [from your code](https://docs.wandb.ai/artifacts/api#updating-artifacts). You can also add longer notes or dictionary-style metadata elsewhere.
* leverage the [artifacts API](https://docs.wandb.ai/artifacts/api#updating-artifacts) to **traverse the artifacts graph** and script pipelines, e.g. to automatically evaluate new models once they're finished training
* **create dynamically-updating** [**reports**](https://docs.wandb.ai/reports) **and dashboards** to show the top-performing models for your target metrics and **deep-link to the relevant model** artifacts for downstream use
* **maintain lineages of models** via fixed artifact types and only save models which improve on the best performance, such that the `"latest"` alias always points to the best model version of that type
* refer to fixed model artifacts by name and alias (or version) when running experiments, such that across individuals and teams **all projects** **use an identical copy of the model**

From the project dashboard, see which runs are prod\_ready and find the corresponding model artifacts for download by clicking on the run name.

![](<../../.gitbook/assets/image (82).png>)

## Visualize & share your workflow

Artifacts let you see and formalize the stages of your model development, keeping all the model variants reliably accessible and organized in helpful ways **for your entire team,** giving you one shared source of truth for each

* **meaningful type of model your team creates**: use the artifact type to group different named artifacts together in the compute graph (e.g. `"resnet_model"` vs `"inceptionV3_model"`, `"a2c_agent"` vs `"a3c_agent"`). Different model artifacts within the type then have different names. For a given named model artifact, we recommend that the artifact's versions correspond to consecutive model checkpoints \[1]
* **hypothesis or exploration branch your team tries: e**asily track which parameter or code changes in your experiments led to which model checkpoints. Interact with all the connections between your data, training code, and resulting models as you explore the artifact graph (input artifact(s) → script or job → output artifact(s)). Click "explode" on the compute graph to see all the versions for each artifact or all the runs of each script by job type. Click individual nodes to see further details in a new tab (file contents or code, annotations/metadata, config, timestamp, parent/child nodes, etc).
* **meaningful instance pointer or alias your team needs**: use an alias like `"prod_ready"`, `"SOTA"`, or "`baseline"` to standardize models across your team. These will reliably return the same model checkpoint files, facilitating more scalable and reproducible workflows across file systems, environments, hardware, user accounts, etc.

With artifacts, you can iterate confidently, knowing that the models resulting from all of your experiments will be saved, versioned, and organized for easy retrieval. Cleanup of unused artifacts is straightforward through the browser or [API](https://docs.wandb.ai/artifacts/api#cleaning-up-unused-versions).

Below is a walkthrough of a combination of these features for visualizing a workflow.

## Longer example: Compute graph exploration

[Follow along here →](https://wandb.ai/stacey/evalserver\_answers\_2/workspace?workspace=user-stacey)

Let's find the best results across experiments: here, evaluations of candidate models entered into a comparison benchmark. Swept-water-5 (boxed in green) has the highest mean class IOU. Click on the run name to see the input and output artifacts.

![](<../../.gitbook/assets/image (83).png>)

This view shows the input and output artifacts of the experiment run "swept-water-5". This run read in a labeled test dataset and a model entry's predictions on that data, evaluated the correctness of the predictions based on the ground truth labels, and saved the results as an artifact. Click on "entry\_predictions" to see how they were generated.

![](<../../.gitbook/assets/image (84).png>)

These model predictions were one entry from a long list of submissions to the benchmark by different teams shown in the sidebar. They were generated by the run "skilled-tree-6".

![](<../../.gitbook/assets/image (85).png>)

View the model used to generate these predictions.

![](<../../.gitbook/assets/image (86).png>)

View the training details of this model and all the runs using it for evaluation.

![](<../../.gitbook/assets/image (87).png>)

View all the predictions saved after each epoch on a random subset of training images and a fixed subset of validation images.

![](<../../.gitbook/assets/image (88).png>)

The model itself—attempt v3 of a resnet18 architecture—appears as an output artifact at the end of this list.

![](<../../.gitbook/assets/image (89).png>)

**Endnotes**

\[1] For a short experiment, the versions can also be distinct model variants (e.g. various learning rates on the same model architecture). This may get unwieldy after a handful of numerically indexed versions, as you may forget what exactly changed between v4 and v6 in terms of learning rate. Aliases help here, and we suggest using a new meaningful artifact name once you're juggling many substantially different model variants under one name.
