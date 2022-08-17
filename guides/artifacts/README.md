---
description: >-
  Dataset versioning, model versioning, pipeline tracking with flexible and
  lightweight building blocks
---

# Data + Model Versioning

Use W\&B Artifacts for dataset versioning, model versioning, and tracking dependencies and results across machine learning pipelines. Think of an artifact as a versioned folder of data. You can store entire datasets directly in artifacts, or use artifact references to point to data in other systems like S3, GCP, or your own system.

## Artifacts Quickstart

The easiest way to log an artifact is passing a path to your data files. Remember to also specify a name and an artifact type.

```
wandb.log_artifact(file_path, name='new_artifact', type='my_dataset') 
```

This will create a new artifact in your project's workspace:

![](<../../.gitbook/assets/Screen Shot 2021-11-05 at 1.23.04 PM.png>)

### Log a new version

If you log again, we'll checksum the artifact, identify that something changed, and track the new version. If nothing changes, we don't re-upload any data or create a new version.

```
artifact = wandb.Artifact('new_artifact', type='my_dataset')
artifact.add_dir('nature_100/')
run.log_artifact(artifact)
```

![In your Artifact page, click on the Compare button to see a new folder appears in the new version](<../../.gitbook/assets/Screen Shot 2021-11-05 at 1.34.26 PM.png>)

### Use your artifact

In a separate run, you can retrieve and download a specific version of an artifact to a local path:

```
artifact = run.use_artifact('user_name/project_name/new_artifact:v1', type='my_dataset')
artifact_dir = artifact.download()
```

### [![](https://colab.research.google.com/assets/colab-badge.svg)](http://wandb.me/artifacts-quickstart)

Looking for a longer example with real model training? Try our [Guide to W\&B Artifacts](https://wandb.ai/wandb/arttest/reports/Guide-to-W-B-Artifacts--VmlldzozNTAzMDM).

![](<../../.gitbook/assets/keras example.png>)

## How it works

Using our Artifacts API, you can log artifacts as outputs of W\&B runs, or use artifacts as input to runs.

![](<../../.gitbook/assets/simple artifact diagram 2 (1).png>)

Since a run can use another run’s output artifact as input, artifacts and runs together form a directed graph. You don’t need to define pipelines ahead of time. Just use and log artifacts, and we’ll stitch everything together.

Here's an [example artifact](https://app.wandb.ai/shawn/detectron2-11/artifacts/model/run-1cxg5qfx-model/4a0e3a7c5bff65ff4f91/graph) where you can see the summary view of the DAG, as well as the zoomed-out view of every execution of each step and every artifact version.

![](<../../.gitbook/assets/2020-09-03 15.59.43.gif>)

## Artifacts resources

Learn more about using artifacts for data and model versioning:

1. [Artifacts Core Concepts](artifacts-core-concepts.md)
2. [Artifacts Walkthrough](api.md)
3. [Dataset Versioning](dataset-versioning.md)
4. [Model Versioning](model-versioning.md)
5. [Artifacts FAQs](artifacts-faqs.md)
6. [Artifacts Examples](examples.md)
7. [Artifact reference docs](https://docs.wandb.ai/ref/python/artifact)

## Video tutorial for W\&B Artifacts

Follow along with our [tutorial video](http://wandb.me/artifacts-video) and [interactive colab](http://wandb.me/artifacts-colab) and learn how to track your machine learning pipeline with W\&B Artifacts.

{% embed url="http://wandb.me/artifacts-video" %}
