# Delete artifacts

Delete artifacts interactively with the App UI or programmatically with the Weights & Biases SDK. Weights & Biases first checks if the artifact and its associated files are not used by a previous or subsequent artifact version before it deletes an artifact. You can delete a specific artifact version or delete the entire artifact.

You can delete aliases before you delete an artifact or you can delete an artifact and pass an additional flag to the API call. It is recommended that you remove aliases associated to the artifact you want to delete before you delete the artifact.

See the Update an artifact documentation for information on how to programmatically or interactively update an alias with the W\&B SDK or App UI, respectively.

### Delete an artifact version

To delete an artifact version:

1. Select the name of the artifact. This will expand the artifact view and list all the artifact versions associated with that artifact.
2. From the list of artifacts, select the artifact version you want to delete.
3. On the right hand side of the workspace, select the kebab dropdown.
4. Choose Delete.

### Delete multiple artifacts with aliases

The following code example demonstrates how to delete artifacts that have aliases associated with them. Provide the entity, project name, and run ID that created the artifacts.

```python
import wandb

run = api.run('entity/project/run_id')

for artifact in run.logged_artifacts():
    artifact.delete()
```

Set the `delete_aliases` parameter to the boolean value, `True` to delete aliases if the artifact has one or more aliases.

```python
import wandb

run = api.run('entity/project/run_id')

for artifact in run.logged_artifacts():
    # Set delete_aliases=True in order to delete artifacts with one more aliases
    artifact.delete(delete_aliases=True)
```

### Delete multiple artifact version with a specific alias

The proceeding code demonstrates how to delete multiple artifact versions that have a specific alias. Provide the entity, project name, and run ID that created the artifacts. Replace the deletion logic with your own:

```python
import wandb

runs = api.run('entity/project_name/run_id')

# Delete artifact ith alias 'v3' and 'v4
for artifact_version in runs.logged_artifacts():
  # Replace with your own deletion logic.
  if artifact_version.name[-2:] == 'v3' or artifact_version.name[-2:] == 'v4':
    artifact.delete(delete_aliases=True)
```

### Delete all versions of an artifact that do not have an alias

The following code snippet demonstrates how to delete all versions of an artifact that do not have an alias. Provide the name of the project and entity for the `project` and `entity` keys in `wandb.Api`, respectively:

```python
import wandb
# When using artifact api methods that don't have an entity or project
#  argument, you must provide that information when instantiating the wandb.Api
api = wandb.Api(overrides={
        "project": "project", 
        "entity": "entity"
        })

artifact_type, artifact_name = ... # fill in the desired type + name
for version in api.artifact_versions(artifact_type, artifact_name):
  # Clean up all versions that don't have an alias such as 'latest'.
	# NOTE: You can put whatever deletion logic you want here.
  if len(version.aliases) == 0:
      version.delete()
```
