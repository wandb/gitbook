# Update an artifact

Pass desired values to update the `description`, `metadata`, and `alias` of an artifact. Call the `save()` method to update the artifact on the Weights and Biases servers. You can update an artifact during a Weights and Biases Run or outside of a Run.

Use the Weights and Biases Public API ([`wandb.Api`](https://docs.wandb.ai/ref/python/public-api/api)) to update an artifact outside of a run. Use the Artifact API ([`wandb.Artifact`](https://docs.wandb.ai/ref/python/artifact)) to update an artifact during a run.&#x20;

{% hint style="warning" %}
You can not update the alias of artifact that is linked to a model in Model Registry.
{% endhint %}

{% tabs %}
{% tab title="During a run" %}
The proceeding code example demonstrates how to update the description of an artifact using the [`wandb.Artifact`](https://docs.wandb.ai/ref/python/artifact) API:

```python
import wandb

run = wandb.init(project="<example>", job_type="<job-type>")
artifact = run.use_artifact('<artifact-name>:<alias>')

artifact = wandb.Artifact('')
run.use_artifact(artifact)
artifact.description = '<description>'
artifact.save()
```
{% endtab %}

{% tab title="Outside of a run " %}
The proceeding code example demonstrates how to update the description of an artifact using the `wandb.Api` API:

```python
import wandb

api = wandb.Api()

artifact = api.artifact('entity/project/artifact:alias')

# Update the description
artifact.description = "My new description"

# Selectively update metadata keys
artifact.metadata["oldKey"] = "new value"

# Replace the metadata entirely
artifact.metadata = {"newKey": "new value"}

# Add an alias
artifact.aliases.append('best')

# Remove an alias
artifact.aliases.remove('latest')

# Completely replace the aliases
artifact.aliases = ['replaced']

# Persist all artifact modifications
artifact.save()
```

For more information, see the Weights and Biases [Public Artifact API](https://docs.wandb.ai/ref/python/public-api/artifact).
{% endtab %}
{% endtabs %}
