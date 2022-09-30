# Create new artifact versions

Create a new artifact version using a single run, collaboratively using distributed writers, or as a patch against a prior version.

Create a new artifact version in one of three ways:

* **Simple**: A single run provides all the data for a new version. This is the most common case and is best suited for when the run fully recreates the needed data. For example: outputting saved models or model predictions in a table for analysis.
* **Collaborative**: A set of runs collectively provides all the data for a new version. This is best suited for distributed jobs which have multiple runs generating data, often in parallel. For example: evaluating a model in a distributed manner, and outputting the predictions.
* **Patch:** (coming soon) A single run provides a patch of the differences to be applied. This is best suited when a run wants to add data to an artifact without needing to recreate all the already existing data. For example: you have a golden dataset which is created by running a daily web scraper - in this case, you want the run to append new data to the dataset.

<figure><img src="../../../.gitbook/assets/image (186).png" alt=""><figcaption></figcaption></figure>

### Simple Mode

To log a new version of an Artifact with a single run that produces all the files in the artifact, use the Simple Mode:

```python
with wandb.init() as run:
    artifact = wandb.Artifact("artifact_name", "artifact_type")
    # Add Files and Assets to the artifact using 
    # `.add`, `.add_file`, `.add_dir`, and `.add_reference`
    artifact.add_file("image1.png")
    run.log_artifact(artifact)
```

Use the `Artifact.save()` to create the version without starting a run.

```python
artifact = wandb.Artifact("artifact_name", "artifact_type")
# Add Files and Assets to the artifact using 
# `.add`, `.add_file`, `.add_dir`, and `.add_reference`
artifact.add_file("image1.png")
artifact.save()
```

### Collaborative Mode

Use Collaborative Mode to allow a collection of runs to collaborate on a version before committing it. There are two key ideas to keep in mind when using Collaborative Mode:

1. Each Run in the collection needs to be aware of the same unique ID (called `distributed_id`) in order to collaborate on the same version. As a default, if present, Weights & Biases uses the run's `group` as set by `wandb.init(group=GROUP)` as the `distributed_id`.
2. There must be a final run that "commits" the version, permanently locking its state.

Consider the following example. Note that rather than using `log_artifact` we use `upsert_artifact` to add the the collaborative artifact and `finish_artifact` to finalize the commit:

#### Run 1:

```python
with wandb.init() as run:
    artifact = wandb.Artifact("artifact_name", "artifact_type")
    # Add Files and Assets to the artifact using 
    # `.add`, `.add_file`, `.add_dir`, and `.add_reference`
    artifact.add_file("image1.png")
    run.upsert_artifact(artifact, distributed_id="my_dist_artifact")
```

**Run 2:**

```python
with wandb.init() as run:
    artifact = wandb.Artifact("artifact_name", "artifact_type")
    # Add Files and Assets to the artifact using 
    # `.add`, `.add_file`, `.add_dir`, and `.add_reference`
    artifact.add_file("image2.png")
    run.upsert_artifact(artifact, distributed_id="my_dist_artifact")
```

#### **Run 3**

Must run after Run 1 and Run 2 complete. The Run that calls `finish_artifact` can include files in the artifact, but does not need to.

```python
with wandb.init() as run:
    artifact = wandb.Artifact("artifact_name", "artifact_type")
    # Add Files and Assets to the artifact uthonsing 
    # `.add`, `.add_file`, `.add_dir`, and `.add_reference`
    artifact.add_file("image3.png")
    run.finish_artifact(artifact, distributed_id="my_dist_artifact")
```
