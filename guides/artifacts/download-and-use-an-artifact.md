# Download and use artifacts

Download and use an artifact that is already stored on the Weights and Biases server or construct an artifact object and pass it in to be deduplicated as necessary.

{% hint style="info" %}
Team members with view-only seats cannot download artifacts.
{% endhint %}

### Download and use an artifact stored on Weights and Biases

Download and use an artifact that is stored in Weights and Biases either inside or outside of a Weights and Biases Run. Use the Public API ([`wandb.Api`](https://docs.wandb.ai/ref/python/public-api/api)) to export (or update data) already saved in a Weights and Biases. For more information, see the Weights and Biases [Public API Reference guide](https://docs.wandb.ai/ref/python/public-api).

{% tabs %}
{% tab title="During a run" %}
First, import the Weights and Biases Python SDK. Next, create a Weights and Biases [run](https://docs.wandb.ai/ref/python/run):

```python
import wandb

run = wandb.init(project="<example>", job_type="<job-type>")
```

Indicate the artifact you want to use with the [`use_artifact`](https://docs.wandb.ai/ref/python/run#use\_artifact) method. This returns a run object. In the proceeding code snippet we specify an artifact called `'bike-dataset'` with alias `'latest'`:

```python
artifact = run.use_artifact('bike-dataset:latest')
```

Use the object returned to download all the contents of the artifact:

```python
datadir = artifact.download()
```

You can optionally pass a path to the root parameter to download the contents of the artifact to a specific directory. For more information, see the [Python SDK Reference Guide](https://docs.wandb.ai/ref/python/artifact#download).

Use the [`get_path`](https://docs.wandb.ai/ref/python/artifact#get\_path) method to download only subset of files:

```python
path = artifact.get_path(name)
```

This fetches only the file at the path `name`. It returns an `Entry` object with the following methods:

* `Entry.download`: Downloads file from the artifact at path `name`
* `Entry.ref`: If the entry was stored as a reference using `add_reference`, returns the URI

References that have schemes that Weights and Biases knows how to handle can be downloaded just like artifact files.  For more information, see [Track external files](https://docs.wandb.ai/guides/artifacts/track-external-files).
{% endtab %}

{% tab title="Outside of a run" %}
First, import the Weights and Biases SDK. Next, create an artifact from the Public API Class. Provide the entity, project, artifact, and alias associated with that artifact:

```python
import wandb

api = wandb.Api()
artifact = api.artifact('entity/project/artifact:alias')
```

Use the object returned to download the contents of the artifact:

```python
artifact.download()
```

You can optionally pass a path the `root` parameter to download the contents of the artifact to a specific directory. For more information, see \[LINK].
{% endtab %}

{% tab title="wandb CLI" %}
Use the `wandb artifact get` command to download an artifact from the Weights and Biases server.

```
$ wandb artifact get project/artifact:alias --root mnist/
```
{% endtab %}
{% endtabs %}

### Use an artifact from a different project

Specify the name of artifact along with its project name to reference an artifact. You can also reference artifacts across entities by specifying the name of the artifact with its entity name.

The following code example demonstrates how to query an artifact from another project as input to our current Weights and Biases run. &#x20;

```python
import wandb

run = wandb.init(project="<example>", job_type="<job-type>")
# Query W&B for an artifact from another project and mark it
# as an input to this run.
artifact = run.use_artifact('my-project/artifact:alias')

# Use an artifact from another entity and mark it as an input
# to this run.
artifact = run.use_artifact('my-entity/my-project/artifact:alias')
```

### Construct and use an artifact simultaneously

Simultaneously construct and use an artifact. Create an artifact object and pass it to use\_artifact. This will create an artifact in Weights and Biases if it does not exist yet. The [`use_artifact`](https://docs.wandb.ai/ref/python/run#use\_artifact) API is idempotent, so you can call it as many times as you like.

```python
import wandb
artifact = wandb.Artifact('reference model')
artifact.add_file('model.h5')
run.use_artifact(artifact)
```

For more information about constructing an artifact, see [Construct an artifact](https://docs.wandb.ai/guides/artifacts/construct-an-artifact).
