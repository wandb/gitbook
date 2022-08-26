# Artifacts

Use Weights and Biases Artifacts to track datasets, models, dependencies, and results through each step of your machine learning pipeline. Artifacts make it easy to get a complete and auditable history of changes to your files.

Artifacts can be thought of as a versioned directory. Artifacts are either an input of a run or an output of a run. Common artifacts include entire training sets and models. Store datasets directly into artifacts, or use artifact references to point to data in other systems like Amazon S3, GCP, or your own system.

<figure><img src="../../.gitbook/assets/image (185).png" alt=""><figcaption><p>Artifacts can be an input or an output of a given run.</p></figcaption></figure>

Artifacts and runs form a directed graph because a given Weights and Biases run can use another run’s output artifact as input. You do not need to define pipelines ahead of time. Weights and Biases will create the DAG for you when you use and log artifacts.&#x20;

The following animation demonstrates an example artifacts DAG as seen in the Weights and Biases App UI.&#x20;

![Example artifact DAG](<../../.gitbook/assets/2020-09-03 15.59.43.gif>)

For more information about exploring an artifacts graph, see[ Explore and traverse an artifact graph](https://app.gitbook.com/o/-Lr2SEfv2R3GSuF1kZCt/s/-Lqya5RvLedGEWPhtkjU-1972196547/\~/changes/j1B9n6G73J5mTKwAVy6u/guides/artifacts/explore-and-traverse-an-artifact-graph).

### How it works

An artifact is like a directory of data. Each entry is either an actual file stored in the artifact, or a reference to an external URI. You can nest folders inside an artifact just like a regular filesystem. You can store any data, including: datasets, models, images, HTML, code, audio, raw binary data and more.

Every time you change the contents of this directory, Weights and Biases will create a new version of your artifact instead overwriting the previous contents.

As an example, assume we have the following directory structure:

```
images
|-- cat.png (2MB)
|-- dog.png (1MB)
```

The proceeding code snippet demonstrates how to create a dataset artifact called `‘animals’`. (The specifics of how the following code snippet work are explained in greater detail in later sections).

```python
import wandb

run = wandb.init() # Initialize a W&B Run
artifact = wandb.Artifact('animals', type='dataset')
artifact.add_dir('images') # Adds multiple files to artifact
run.log_artifact(artifact) # Creates `animals:v0`
```

Weights and Biases automatically assigns a version `v0` and attaches an alias called `latest` when you create and log a new artifact object to Weights and Biases. An _alias_ is a human-readable name that you can give to an artifact version.

If you create another artifact with the same name, type, and contents (in other words, you create another version of the artifact), Weights and Biases will increase the version index by one. The alias `latest` is unassigned from artifact `v0` and assigned to the `v1` artifact.

Weights and Biases uploads files that were modified between artifacts versions. For more information about how artifacts are stored, see [Artifacts Storage](https://app.gitbook.com/o/-Lr2SEfv2R3GSuF1kZCt/s/-Lqya5RvLedGEWPhtkjU-1972196547/\~/changes/j1B9n6G73J5mTKwAVy6u/guides/artifacts/storage).

You can use either the index version or the alias to refer to a specific artifact.&#x20;

As an example, suppose you want to upload a new image, `bird.png`, to your dataset artifact. Continuing from the previous code example, your directory might look similar to the following:

```
images
|-- cat.png (2MB)
|-- dog.png (1MB)
|-- bird.png (3MB)
```

Re initialize the previous code snippet. This will produce a new artifact version `animals:v1`. Weights and Biases will automatically assign this version with the alias: `latest` . You can customize the aliases to apply to a version by passing in `aliases=['my-cool-alias']` to `log_artifact`. For more information about how to create new versions, see [Create a new artifact version](https://app.gitbook.com/o/-Lr2SEfv2R3GSuF1kZCt/s/-Lqya5RvLedGEWPhtkjU-1972196547/\~/changes/j1B9n6G73J5mTKwAVy6u/guides/artifacts/create-a-new-artifact-version).

To use the artifact, provide the name of the artifact along with the alias.&#x20;

```python
import wandb

run = wandb.init()
animals = run.use_artifact('animals:latest')
directory = animals.download()
```

For more information about how to download use artifacts, see [Use an artifact](https://app.gitbook.com/o/-Lr2SEfv2R3GSuF1kZCt/s/-Lqya5RvLedGEWPhtkjU-1972196547/\~/changes/j1B9n6G73J5mTKwAVy6u/guides/artifacts/use-an-artifact).

### How to get started

Depending on your use case, explore the following resources to get started with Weights and Biases Artifacts:

* If this is your first time using Weights and Biases Artifacts, we recommend you read the Quick Start. The [Quick Start](https://app.gitbook.com/o/-Lr2SEfv2R3GSuF1kZCt/s/-Lqya5RvLedGEWPhtkjU-1972196547/\~/changes/j1B9n6G73J5mTKwAVy6u/guides/artifacts/quick-start) walks you through setting up your first artifact
* Looking for a longer example with real model training? Try our [Guide to W\&B Artifacts](https://wandb.ai/wandb/arttest/reports/Guide-to-W-B-Artifacts--VmlldzozNTAzMDM).
* Explore topics about Artifacts in the Weights and Biases Developer Guide such as:
  * Create an artifact or a new artifact version.
  * Update an artifact.
  * Download and use an artifact.
  * Delete artifacts.
* Read the [Weights and Biases SDK Reference Guide](https://docs.wandb.ai/ref).

For a step-by-step video, see [Version Control Data and Model with W\&B Artifacts](https://www.youtube.com/watch?v=Hd94gatGMic\&ab\_channel=Weights%26Biases):&#x20;

{% embed url="https://www.youtube.com/watch?v=Hd94gatGMic" %}
