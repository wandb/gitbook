# wandb.apis.public.Artifact

[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/v0.12.7/wandb/apis/public.py#L3309-L4124)

A wandb Artifact.

```python
Artifact(
    client, entity, project, name, attrs=None
)
```

An artifact that has been logged, including all its attributes, links to the runs that use it, and a link to the run that logged it.

#### Examples:

Basic usage

```python
api = wandb.Api()
artifact = api.artifact('project/artifact:alias')

# Get information about the artifact...
artifact.digest
artifact.aliases
```

Updating an artifact

```python
artifact = api.artifact('project/artifact:alias')

# Update the description
artifact.description = 'My new description'

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

Artifact graph traversal

```python
artifact = api.artifact('project/artifact:alias')

# Walk up and down the graph from an artifact:
producer_run = artifact.logged_by()
consumer_runs = artifact.used_by()

# Walk up and down the graph from a run:
logged_artifacts = run.logged_artifacts()
used_artifacts = run.used_artifacts()
```

Deleting an artifact

```python
artifact = api.artifact('project/artifact:alias')
artifact.delete()
```

| Attributes    |                                                                                                                                                                                                                 |
| ------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `aliases`     | The aliases associated with this artifact.                                                                                                                                                                      |
| `commit_hash` | Returns: (str): The artifact's commit hash which is used in http URLs                                                                                                                                           |
| `created_at`  | Returns: (datetime): The time at which the artifact was created.                                                                                                                                                |
| `description` | Returns: (str): Free text that offers a description of the artifact. The description is markdown rendered in the UI, so this is a good place to put links, etc.                                                 |
| `digest`      | Returns: (str): The artifact's logical digest, a checksum of its contents. If an artifact has the same digest as the current `latest` version, then `log_artifact` is a no-op.                                  |
| `entity`      | Returns: (str): The name of the entity this artifact belongs to.                                                                                                                                                |
| `id`          | Returns: (str): The artifact's ID                                                                                                                                                                               |
| `manifest`    | Returns: (ArtifactManifest): The artifact's manifest, listing all of its contents. You cannot add more files to an artifact once you've retrieved its manifest.                                                 |
| `metadata`    | Returns: (dict): Structured data associated with the artifact, for example class distribution of a dataset. This will eventually be queryable and plottable in the UI. There is a hard limit of 100 total keys. |
| `name`        | Returns: (str): The artifact's name                                                                                                                                                                             |
| `project`     | Returns: (str): The name of the project this artifact belongs to.                                                                                                                                               |
| `size`        | Returns: (int): The size in bytes of the artifact. Includes any references tracked by this artifact.                                                                                                            |
| `state`       | Returns: (str): The state of the artifact, which can be one of "PENDING", "COMMITTED", or "DELETED".                                                                                                            |
| `type`        | Returns: (str): The artifact's type                                                                                                                                                                             |
| `updated_at`  | Returns: (datetime): The time at which the artifact was last updated.                                                                                                                                           |
| `version`     | Returns: (int): The version of this artifact. For example, if this is the first version of an artifact, its `version` will be 'v0'.                                                                             |

## Methods

### `add` <a href="add" id="add"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.7/wandb/apis/public.py#L3660-L3661)

```python
add(
    obj, name
)
```

Adds wandb.WBValue `obj` to the artifact.

```
obj = artifact.get(name)
```

| Arguments |                                                                                                                                                                                   |
| --------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `obj`     | (wandb.WBValue) The object to add. Currently support one of Bokeh, JoinedTable, PartitionedTable, Table, Classes, ImageMask, BoundingBoxes2D, Audio, Image, Video, Html, Object3D |
| `name`    | (str) The path within the artifact to add the object.                                                                                                                             |

| Returns                 |                          |
| ----------------------- | ------------------------ |
| `ArtifactManifestEntry` | the added manifest entry |

#### Examples:

Basic usage

```python
artifact = wandb.Artifact('my_table', 'dataset')
table = wandb.Table(columns=["a", "b", "c"], data=[[i, i*2, 2**i]])
artifact.add(table, "my_table")

wandb.log_artifact(artifact)
```

Retrieving an object:

```python
artifact = wandb.use_artifact('my_table:latest')
table = artifact.get("my_table")
```

### `add_dir` <a href="add_dir" id="add_dir"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.7/wandb/apis/public.py#L3654-L3655)

```python
add_dir(
    path, name=None
)
```

Adds a local directory to the artifact.

| Arguments    |                                                                                                                                                  |
| ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| `local_path` | (str) The path to the directory being added.                                                                                                     |
| `name`       | (str, optional) The path within the artifact to use for the directory being added. Defaults to files being added under the root of the artifact. |

#### Examples:

Adding a directory without an explicit name:

```python
artifact.add_dir('my_dir/') # All files in `my_dir/` are added at the root of the artifact.
```

Adding a directory without an explicit name:

```python
artifact.add_dir('my_dir/', path='destination') # All files in `my_dir/` are added under `destination/`.
```

| Raises      |             |
| ----------- | ----------- |
| `Exception` | if problem. |

| Returns |   |
| ------- | - |
| None    |   |

### `add_file` <a href="add_file" id="add_file"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.7/wandb/apis/public.py#L3651-L3652)

```python
add_file(
    local_path, name=None, is_tmp=(False)
)
```

Adds a local file to the artifact.

| Arguments    |                                                                                                                     |
| ------------ | ------------------------------------------------------------------------------------------------------------------- |
| `local_path` | (str) The path to the file being added.                                                                             |
| `name`       | (str, optional) The path within the artifact to use for the file being added. Defaults to the basename of the file. |
| `is_tmp`     | (bool, optional) If true, then the file is renamed deterministically to avoid collisions. (default: False)          |

#### Examples:

Adding a file without an explicit name:

```python
artifact.add_file('path/to/file.txt') # Added as `file.txt'
```

Adding a file with an explicit name:

```python
artifact.add_file('path/to/file.txt', name='new/path/file.txt') # Added as 'new/path/file.txt'
```

| Raises      |            |
| ----------- | ---------- |
| `Exception` | if problem |

| Returns                 |                          |
| ----------------------- | ------------------------ |
| `ArtifactManifestEntry` | the added manifest entry |

### `add_reference` <a href="add_reference" id="add_reference"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.7/wandb/apis/public.py#L3657-L3658)

```python
add_reference(
    uri, name=None, checksum=(True), max_objects=None
)
```

Adds a reference denoted by a URI to the artifact. Unlike adding files or directories, references are NOT uploaded to W\&B. However, artifact methods such as `download()` can be used regardless of whether the artifact contains references or uploaded files.

By default, W\&B offers special handling for the following schemes:

* http(s): The size and digest of the file will be inferred by the `Content-Length` and the `ETag` response headers returned by the server.
* s3: The checksum and size will be pulled from the object metadata. If bucket versioning is enabled, then the version ID is also tracked.
* gs: The checksum and size will be pulled from the object metadata. If bucket versioning is enabled, then the version ID is also tracked.
* file: The checksum and size will be pulled from the file system. This scheme is useful if you have an NFS share or other externally mounted volume containing files you wish to track but not necessarily upload.

For any other scheme, the digest is just a hash of the URI and the size is left blank.

| Arguments     |                                                                                                                                                                                                                                                        |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `uri`         | (str) The URI path of the reference to add. Can be an object returned from Artifact.get\_path to store a reference to another artifact's entry.                                                                                                        |
| `name`        | (str) The path within the artifact to place the contents of this reference                                                                                                                                                                             |
| `checksum`    | (bool, optional) Whether or not to checksum the resource(s) located at the reference URI. Checksumming is strongly recommended as it enables automatic integrity validation, however it can be disabled to speed up artifact creation. (default: True) |
| `max_objects` | (int, optional) The maximum number of objects to consider when adding a reference that points to directory or bucket store prefix. For S3 and GCS, this limit is 10,000 by default but is uncapped for other URI schemes. (default: None)              |

| Raises      |             |
| ----------- | ----------- |
| `Exception` | If problem. |

| Returns                                                   |   |
| --------------------------------------------------------- | - |
| List\[ArtifactManifestEntry]: The added manifest entries. |   |

#### Examples:

Adding an HTTP link:

```python
# Adds `file.txt` to the root of the artifact as a reference
artifact.add_reference('http://myserver.com/file.txt')
```

Adding an S3 prefix without an explicit name:

```python
# All objects under `prefix/` will be added at the root of the artifact.
artifact.add_reference('s3://mybucket/prefix')
```

Adding a GCS prefix with an explicit name:

```python
# All objects under `prefix/` will be added under `path/` at the top of the artifact.
artifact.add_reference('gs://mybucket/prefix', name='path')
```

### `checkout` <a href="checkout" id="checkout"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.7/wandb/apis/public.py#L3780-L3795)

```python
checkout(
    root=None
)
```

Replaces the specified root directory with the contents of the artifact.

WARNING: This will DELETE all files in `root` that are not included in the artifact.

**Examples:**

```python
# Create a file that should be removed as part of checkout
os.makedirs(os.path.join(".", "artifacts", "mnist"))
with open(os.path.join(".", "artifacts", "mnist", "bogus"), "w") as f:
    f.write("deleting bogus file")

art = api.artifact("entity/project/mnist:v0", type="dataset")
path = art.checkout()
assert path == os.path.join(".", "artifacts", "mnist")
```

| Arguments |                                                                      |
| --------- | -------------------------------------------------------------------- |
| `root`    | (str, optional) The directory to replace with this artifact's files. |

| Returns                                      |   |
| -------------------------------------------- | - |
| (str): The path to the checked out contents. |   |

### `delete` <a href="delete" id="delete"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.7/wandb/apis/public.py#L3609-L3646)

```python
delete(
    delete_aliases=(False)
)
```

Delete an artifact and its files.

#### Examples:

Delete all the "model" artifacts a run has logged:

```python
runs = api.runs(path="my_entity/my_project")
for run in runs:
    for artifact in run.logged_artifacts():
        if artifact.type == "model":
            artifact.delete(delete_aliases=True)
```

| Arguments        |                                                                                                                                             |
| ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| `delete_aliases` | (bool) If true, deletes all aliases associated with the artifact. Otherwise, this raises an exception if the artifact has existing alaises. |

### `download` <a href="download" id="download"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.7/wandb/apis/public.py#L3743-L3778)

```python
download(
    root=None, recursive=(False)
)
```

Downloads the contents of the artifact to the specified root directory.

NOTE: Any existing files at `root` are left untouched. Explicitly delete root before calling `download` if you want the contents of `root` to exactly match the artifact.

**Examples:**

```python
api = wandb.Api()
artifact = api.artifact('entity/project/artifact:alias')
artifact_dir = artifact.download()
```

| Arguments   |                                                                                                                                             |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| `root`      | (str, optional) The directory in which to download this artifact's files.                                                                   |
| `recursive` | (bool, optional) If true, then all dependent artifacts are eagerly downloaded. Otherwise, the dependent artifacts are downloaded as needed. |

| Returns                                     |   |
| ------------------------------------------- | - |
| (str): The path to the downloaded contents. |   |

### `expected_type` <a href="expected_type" id="expected_type"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.7/wandb/apis/public.py#L3558-L3598)

```python
@staticmethod
expected_type(
    client, name, entity_name, project_name
)
```

Returns the expected type for a given artifact name and project

### `file` <a href="file" id="file"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.7/wandb/apis/public.py#L3829-L3850)

```python
file(
    root=None
)
```

Download a single file artifact to dir specified by the `root`.

| Arguments |                                                                                                         |
| --------- | ------------------------------------------------------------------------------------------------------- |
| `root`    | (str, optional) The root directory in which to place the file. Defaults to './artifacts/\<self.name>/'. |

| Returns                                      |   |
| -------------------------------------------- | - |
| (str): The full path of the downloaded file. |   |

### `from_id` <a href="from_id" id="from_id"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.7/wandb/apis/public.py#L3394-L3434)

```python
@classmethod
from_id(
    artifact_id, client
)
```

### `get` <a href="get" id="get"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.7/wandb/apis/public.py#L3715-L3741)

```python
get(
    name
)
```

Gets the WBValue object located at the artifact relative `name`.

NOTE: This will raise an error unless the artifact has been fetched using `use_artifact`, fetched using the API, or `wait()` has been called.

| Arguments |                                         |
| --------- | --------------------------------------- |
| `name`    | (str) The artifact relative name to get |

| Raises      |            |
| ----------- | ---------- |
| `Exception` | if problem |

#### Examples:

Basic usage

```python
# Run logging the artifact
with wandb.init() as r:
    artifact = wandb.Artifact('my_dataset', type='dataset')
    table = wandb.Table(columns=["a", "b", "c"], data=[[i, i*2, 2**i]])
    artifact.add(table, "my_table")
    wandb.log_artifact(artifact)

# Run using the artifact
with wandb.init() as r:
    artifact = r.use_artifact('my_dataset:latest')
    table = r.get('my_table')
```

### `get_path` <a href="get_path" id="get_path"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.7/wandb/apis/public.py#L3703-L3713)

```python
get_path(
    name
)
```

Gets the path to the file located at the artifact relative `name`.

NOTE: This will raise an error unless the artifact has been fetched using `use_artifact`, fetched using the API, or `wait()` has been called.

| Arguments |                                         |
| --------- | --------------------------------------- |
| `name`    | (str) The artifact relative name to get |

| Raises      |            |
| ----------- | ---------- |
| `Exception` | if problem |

#### Examples:

Basic usage

```python
# Run logging the artifact
with wandb.init() as r:
    artifact = wandb.Artifact('my_dataset', type='dataset')
    artifact.add_file('path/to/file.txt')
    wandb.log_artifact(artifact)

# Run using the artifact
with wandb.init() as r:
    artifact = r.use_artifact('my_dataset:latest')
    path = artifact.get_path('file.txt')

    # Can now download 'file.txt' directly:
    path.download()
```

### `json_encode` <a href="json_encode" id="json_encode"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.7/wandb/apis/public.py#L3867-L3868)

```python
json_encode()
```

### `logged_by` <a href="logged_by" id="logged_by"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.7/wandb/apis/public.py#L4085-L4118)

```python
logged_by()
```

Retrieves the run which logged this artifact

| Returns |                                       |
| ------- | ------------------------------------- |
| `Run`   | Run object which logged this artifact |

### `new_file` <a href="new_file" id="new_file"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.7/wandb/apis/public.py#L3648-L3649)

```python
new_file(
    name, mode=None
)
```

Open a new temporary file that will be automatically added to the artifact.

| Arguments |                                                             |
| --------- | ----------------------------------------------------------- |
| `name`    | (str) The name of the new file being added to the artifact. |
| `mode`    | (str, optional) The mode in which to open the new file.     |

#### Examples:

```python
artifact = wandb.Artifact('my_data', type='dataset')
with artifact.new_file('hello.txt') as f:
    f.write('hello!')
wandb.log_artifact(artifact)
```

| Returns                                                                                                               |   |
| --------------------------------------------------------------------------------------------------------------------- | - |
| (file): A new file object that can be written to. Upon closing, the file will be automatically added to the artifact. |   |

### `save` <a href="save" id="save"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.7/wandb/apis/public.py#L3870-L3908)

```python
save()
```

Persists artifact changes to the wandb backend.

**Examples:**

```python
api = wandb.Api()
artifact = api.artifact('mnist:v0')
# add aliases to artifacts programmatically
artifact.aliases.append('new-alias')
artifact.save()
```

### `used_by` <a href="used_by" id="used_by"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.7/wandb/apis/public.py#L4041-L4083)

```python
used_by()
```

Retrieves the runs which use this artifact directly

| Returns                                               |   |
| ----------------------------------------------------- | - |
| \[Run]: a list of Run objects which use this artifact |   |

### `verify` <a href="verify" id="verify"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.7/wandb/apis/public.py#L3797-L3827)

```python
verify(
    root=None
)
```

Verify that the actual contents of an artifact at a specified directory `root` match the expected contents of the artifact according to its manifest.&#x20;

All files in the directory are checksummed and the checksums are then cross-referenced against the artifact's manifest.

NOTE: References are not verified.

| Arguments |                                                                                                             |
| --------- | ----------------------------------------------------------------------------------------------------------- |
| `root`    | (str, optional) The directory to verify. If None artifact will be downloaded to './artifacts/\<self.name>/' |

| Raises                                                                                                                                 |   |
| -------------------------------------------------------------------------------------------------------------------------------------- | - |
| (ValueError): If the verification fails. In other words, if it finds an invalid file or digest it will raise a `ValueError` exception. |   |

### `wait` <a href="wait" id="wait"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.7/wandb/apis/public.py#L3910-L3911)

```python
wait()
```

Waits for this artifact to finish logging, if needed.

| Returns  |   |
| -------- | - |
| Artifact |   |

### `__getitem__` <a href="__getitem__" id="__getitem__"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.7/wandb/apis/public.py#L4123-L4124)

```python
__getitem__(
    name
)
```

Gets the WBValue object located at the artifact relative `name`.

NOTE: This will raise an error unless the artifact has been fetched using `use_artifact`, fetched using the API, or `wait()` has been called.

| Arguments |                                         |
| --------- | --------------------------------------- |
| `name`    | (str) The artifact relative name to get |

| Raises      |            |
| ----------- | ---------- |
| `Exception` | if problem |

#### Examples:

Basic usage

```python
artifact = wandb.Artifact('my_table', 'dataset')
table = wandb.Table(columns=["a", "b", "c"], data=[[i, i*2, 2**i]])
artifact["my_table"] = table

wandb.log_artifact(artifact)
```

Retrieving an object:

```python
artifact = wandb.use_artifact('my_table:latest')
table = artifact["my_table"]
```

| Class Variables |   |
| --------------- | - |
| `QUERY`         |   |
