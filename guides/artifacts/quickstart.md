# Quickstart

The proceeding quick start demonstrates how to create, track, and use a dataset artifact. Ensure you have a Weights & Biases account before you begin.&#x20;

The following procedure lists how to construct and use an artifact. Steps 1 and 2 are not unique to W\&B Artifacts.

1. [Log into Weights & Biases.](quickstart.md#log-into-weights-and-biases)
2. [Initialize a Run.](quickstart.md#initialize)
3. [Create an artifact object.](quickstart.md#create-an-artifact-object)
4. [Add the dataset to the artifact.](quickstart.md#add-a-file)
5. [Log the dataset.](quickstart.md#log-the-file)
6. [Download and use the artifact.](quickstart.md#download-and-use-the-artifact)

### Log into Weights & Biases

Import the Weights & Biases library and log in to W\&B. You will need to sign up for a free W\&B account if you have not done so already.

```python
import wandb

wandb.login()
```

### Initialize a run

Use the [`wandb.init()`](https://docs.wandb.ai/ref/python/init) API to generate a background process to sync and log data as a W\&B Run. Provide a project name and a job type:

```python
# Create a W&B Run. Here we specify 'dataset' as the job type since this example
# shows how to create a dataset artifact.
run = wandb.init(project="artifacts-example", job_type='dataset')
```

### Create an artifact object

Create an artifact object with the [`wandb.Artifact()`](https://docs.wandb.ai/ref/python/artifact) API. Provide a name for the artifact and a description of the file type for the `name` and `type` parameters, respectively.

For example, the following code snippet demonstrates how to create an artifact called `‘bicycle-dataset’` with a `‘dataset’` label:

```python
artifact = wandb.Artifact(name='bicycle-dataset', type='dataset')
```

For more information about how to construct an artifact, see [Construct artifacts](https://docs.wandb.ai/guides/artifacts/construct-an-artifact).

### Add the dataset to the artifact

Add a file to the artifact. Common file types include models and datasets. The following example adds a dataset named `dataset.h5` that is saved locally on our machine to the artifact:

```python
# Add a file to the artifact's contents
artifact.add_file(local_path='dataset.h5')
```

Replace the filename `dataset.h5` in the preceding code snippet with the path to the file you want to add to the artifact.

### Log the dataset

Use the W\&B run objects `log_artifact()` method to both save your artifact version and declare the artifact as an output of the run.

```python
# Save the artifact version to W&B and mark it as the output of this run
run.log_artifact(artifact)
```

A `'latest'` alias is created by default when you log an artifact. For more information about artifact aliases and versions, see [Create a custom alias](https://docs.wandb.ai/guides/artifacts/create-a-custom-alias) and [Create new artifact versions](https://docs.wandb.ai/guides/artifacts/create-a-new-artifact-version), respectively.

### Download and use the artifact

The following code example demonstrates the steps you can take to use an artifact you have logged and saved to the Weights & Biases servers.

1. First, initialize a new run object with **`wandb.init()`.**
2. Second, use the run objects [`use_artifact()`](https://docs.wandb.ai/ref/python/run#use\_artifact) method to tell Weights & Biases what artifact to use. This returns an artifact object.
3. Third, use the artifacts [`download()`](https://docs.wandb.ai/ref/python/artifact#download) method to download the contents of the artifact.

```python
# Create a W&B Run. Here we specify 'training' for 'type' because
# we will use this run to track training.
run = wandb.init(project="artifacts-example", job_type='training')

# Query W&B for an artifact and mark it as input to this run
artifact = run.use_artifact('bicycle-dataset:latest')

# Download the artifact's contents
artifact_dir = artifact.download()
```

Alternatively, you can use the Public API (`wandb.Api`) to export (or update data) data already saved in a Weights & Biases outside of a Run. See [Track external files](https://docs.wandb.ai/guides/artifacts/track-external-files) for more information.
