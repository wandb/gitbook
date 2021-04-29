# wandb.apis.public.Artifact

[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/v0.10.28/wandb/apis/public.py#L2575-L3355)

A wandb Artifact.

```text
Artifact(
    client, entity, project, name, attrs=None
)
```

An artifact that has been logged, including all its attributes, links to the runs that use it, and a link to the run that logged it.

#### Examples:

Basic usage

```text
api = wandb.Api()
artifact = api.artifact('project/artifact:alias')

# Get information about the artifact...
artifact.digest
artifact.aliases
```

Updating an artifact

```text
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

```text
artifact = api.artifact('project/artifact:alias')

# Walk up and down the graph from an artifact:
producer_run = artifact.logged_by()
consumer_runs = artifact.used_by()

# Walk up and down the graph from a run:
logged_artifacts = run.logged_artifacts()
used_artifacts = run.used_artifacts()
```

Deleting an artifact

```text
artifact = api.artifact('project/artifact:alias')
artifact.delete()
```

| Attributes |  |
| :--- | :--- |
|  `aliases` |  The aliases associated with this artifact. |
|  `commit_hash` |  Returns: \(str\): The artifact's commit hash which is used in http URLs |
|  `created_at` |  Returns: \(datetime\): The time at which the artifact was created. |
|  `description` |  Returns: \(str\): Free text that offers a description of the artifact. The description is markdown rendered in the UI, so this is a good place to put links, etc. |
|  `digest` |  Returns: \(str\): The artifact's logical digest, a checksum of its contents. If an artifact has the same digest as the current `latest` version, then `log_artifact` is a no-op. |
|  `entity` |  Returns: \(str\): The name of the entity this artifact belongs to. |
|  `id` |  Returns: \(str\): The artifact's ID |
|  `manifest` |  Returns: \(ArtifactManifest\): The artifact's manifest, listing all of its contents. You cannot add more files to an artifact once you've retrieved its manifest. |
|  `metadata` |  Returns: \(dict\): Structured data associated with the artifact, for example class distribution of a dataset. This will eventually be queryable and plottable in the UI. There is a hard limit of 100 total keys. |
|  `name` |  Returns: \(str\): The artifact's name |
|  `project` |  Returns: \(str\): The name of the project this artifact belongs to. |
|  `size` |  Returns: \(int\): The size in bytes of the artifact. Includes any references tracked by this artifact. |
|  `state` |  Returns: \(str\): The state of the artifact, which can be one of "PENDING", "COMMITTED", or "DELETED". |
|  `type` |  Returns: \(str\): The artifact's type |
|  `updated_at` |  Returns: \(datetime\): The time at which the artifact was last updated. |
|  `version` |  Returns: \(int\): The version of this artifact. For example, if this is the first version of an artifact, its `version` will be 'v0'. |

## Methods

### `add` <a id="add"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.28/wandb/apis/public.py#L2895-L2896)

```text
add(
    obj, name
)
```

Adds wandb.WBValue `obj` to the artifact.

```text
obj = artifact.get(name)
```

| Arguments |  |
| :--- | :--- |
|  `obj` |  \(wandb.WBValue\) The object to add. Currently support one of Bokeh, JoinedTable, PartitionedTable, Table, Classes, ImageMask, BoundingBoxes2D, Audio, Image, Video, Html, Object3D |
|  `name` |  \(str\) The path within the artifact to add the object. |

| Returns |  |
| :--- | :--- |
|  `ArtifactManifestEntry` |  the added manifest entry |

#### Examples:

Basic usage

```text
artifact = wandb.Artifact('my_table', 'dataset')
table = wandb.Table(columns=["a", "b", "c"], data=[[i, i*2, 2**i]])
artifact.add(table, "my_table")

wandb.log_artifact(artifact)
```

Retrieving an object:

```text
artifact = wandb.use_artifact('my_table:latest')
table = artifact.get("my_table")
```

### `add_dir` <a id="add_dir"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.28/wandb/apis/public.py#L2889-L2890)

```text
add_dir(
    path, name=None
)
```

Adds a local directory to the artifact.

| Arguments |  |
| :--- | :--- |
|  `local_path` |  \(str\) The path to the directory being added. |
|  `name` |  \(str, optional\) The path within the artifact to use for the directory being added. Defaults to files being added under the root of the artifact. |

#### Examples:

Adding a directory without an explicit name:

```text
artifact.add_dir('my_dir/') # All files in `my_dir/` are added at the root of the artifact.
```

Adding a directory without an explicit name:

```text
artifact.add_dir('my_dir/', path='destination') # All files in `my_dir/<code> are added under </code>destination/`.
```

| Raises |  |
| :--- | :--- |
|  `Exception` |  if problem. |

| Returns |
| :--- |
|  None |

### `add_file` <a id="add_file"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.28/wandb/apis/public.py#L2886-L2887)

```text
add_file(
    local_path, name=None, is_tmp=(False)
)
```

Adds a local file to the artifact.

| Arguments |  |
| :--- | :--- |
|  `local_path` |  \(str\) The path to the file being added. |
|  `name` |  \(str, optional\) The path within the artifact to use for the file being added. Defaults to the basename of the file. |
|  `is_tmp` |  \(bool, optional\) If true, then the file is renamed deterministically to avoid collisions. \(default: False\) |

#### Examples:

Adding a file without an explicit name:

```text
artifact.add_file('path/to/file.txt') # Added as `file.txt'
```

Adding a file with an explicit name:

```text
artifact.add_file('path/to/file.txt', name='new/path/file.txt') # Added as 'new/path/file.txt'
```

| Raises |  |
| :--- | :--- |
|  `Exception` |  if problem |

| Returns |  |
| :--- | :--- |
|  `ArtifactManifestEntry` |  the added manifest entry |

### `add_reference` <a id="add_reference"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.28/wandb/apis/public.py#L2892-L2893)

```text
add_reference(
    uri, name=None, checksum=(True), max_objects=None
)
```

Adds a reference denoted by a URI to the artifact. Unlike adding files or directories, references are NOT uploaded to W&B. However, artifact methods such as `download()` can be used regardless of whether the artifact contains references or uploaded files.

By default, W&B offers special handling for the following schemes:

* http\(s\): The size and digest of the file will be inferred by the `Content-Length` and

    the `ETag` response headers returned by the server.

* s3: The checksum and size will be pulled from the object metadata. If bucket versioning

    is enabled, then the version ID is also tracked.

* gs: The checksum and size will be pulled from the object metadata. If bucket versioning

    is enabled, then the version ID is also tracked.

* file: The checksum and size will be pulled from the file system. This scheme is useful if

    you have an NFS share or other externally mounted volume containing files you wish to track

    but not necessarily upload.

For any other scheme, the digest is just a hash of the URI and the size is left blank.

| Arguments |  |
| :--- | :--- |
|  `uri` |  \(str\) The URI path of the reference to add. Can be an object returned from Artifact.get\_path to store a reference to another artifact's entry. |
|  `name` |  \(str\) The path within the artifact to place the contents of this reference |
|  `checksum` |  \(bool, optional\) Whether or not to checksum the resource\(s\) located at the reference URI. Checksumming is strongly recommended as it enables automatic integrity validation, however it can be disabled to speed up artifact creation. \(default: True\) |
|  `max_objects` |  \(int, optional\) The maximum number of objects to consider when adding a reference that points to directory or bucket store prefix. For S3 and GCS, this limit is 10,000 by default but is uncapped for other URI schemes. \(default: None\) |

| Raises |  |
| :--- | :--- |
|  `Exception` |  If problem. |

| Returns |
| :--- |
|  List\[ArtifactManifestEntry\]: The added manifest entries. |

#### Examples:

Adding an HTTP link:

```text
# Adds <code>file.txt</code> to the root of the artifact as a reference
artifact.add_reference('http://myserver.com/file.txt')
```

Adding an S3 prefix without an explicit name:

```text
# All objects under `prefix/` will be added at the root of the artifact.
artifact.add_reference('s3://mybucket/prefix')
```

Adding a GCS prefix with an explicit name:

```text
# All objects under `prefix/<code> will be added under </code>path/` at the top of the artifact.
artifact.add_reference('gs://mybucket/prefix', name='path')
```

### `checkout` <a id="checkout"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.28/wandb/apis/public.py#L3015-L3030)

```text
checkout(
    root=None
)
```

Replaces the specified root directory with the contents of the artifact.

WARNING: This will DELETE all files in `root` that are not included in the artifact.

| Arguments |  |
| :--- | :--- |
|  `root` |  \(str, optional\) The directory to replace with this artifact's files. |

| Returns |
| :--- |
|  \(str\): The path to the checked out contents. |

### `delete` <a id="delete"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.28/wandb/apis/public.py#L2866-L2881)

```text
delete()
```

Delete artifact and its files.

### `download` <a id="download"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.28/wandb/apis/public.py#L2978-L3013)

```text
download(
    root=None, recursive=(False)
)
```

Downloads the contents of the artifact to the specified root directory.

NOTE: Any existing files at `root` are left untouched. Explicitly delete root before calling `download` if you want the contents of `root` to exactly match the artifact.

| Arguments |  |
| :--- | :--- |
|  `root` |  \(str, optional\) The directory in which to download this artifact's files. |
|  `recursive` |  \(bool, optional\) If true, then all dependent artifacts are eagerly downloaded. Otherwise, the dependent artifacts are downloaded as needed. |

| Returns |
| :--- |
|  \(str\): The path to the downloaded contents. |

### `expected_type` <a id="expected_type"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.28/wandb/apis/public.py#L2824-L2864)

```text
@staticmethod
expected_type(
    client, name, entity_name, project_name
)
```

Returns the expected type for a given artifact name and project

### `file` <a id="file"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.28/wandb/apis/public.py#L3064-L3085)

```text
file(
    root=None
)
```

Download a single file artifact to dir specified by the 

| Arguments |  |
| :--- | :--- |
|  `root` |  \(str, optional\) The root directory in which to place the file. Defaults to './artifacts//'. |

| Returns |
| :--- |
|  \(str\): The full path of the downloaded file. |

### `from_id` <a id="from_id"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.28/wandb/apis/public.py#L2660-L2700)

```text
@classmethod
from_id(
    artifact_id, client
)
```

### `get` <a id="get"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.28/wandb/apis/public.py#L2950-L2976)

```text
get(
    name
)
```

Gets the WBValue object located at the artifact relative `name`.

NOTE: This will raise an error unless the artifact has been fetched using `use_artifact`, fetched using the API, or `wait()` has been called.

| Arguments |  |
| :--- | :--- |
|  `name` |  \(str\) The artifact relative name to get |

| Raises |  |
| :--- | :--- |
|  `Exception` |  if problem |

#### Examples:

Basic usage

```text
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

### `get_path` <a id="get_path"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.28/wandb/apis/public.py#L2938-L2948)

```text
get_path(
    name
)
```

Gets the path to the file located at the artifact relative `name`.

NOTE: This will raise an error unless the artifact has been fetched using `use_artifact`, fetched using the API, or `wait()` has been called.

| Arguments |  |
| :--- | :--- |
|  `name` |  \(str\) The artifact relative name to get |

| Raises |  |
| :--- | :--- |
|  `Exception` |  if problem |

#### Examples:

Basic usage

```text
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

### `logged_by` <a id="logged_by"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.28/wandb/apis/public.py#L3316-L3349)

```text
logged_by()
```

Retrieves the run which logged this artifact

| Returns |  |
| :--- | :--- |
|  `Run` |  Run object which logged this artifact |

### `new_file` <a id="new_file"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.28/wandb/apis/public.py#L2883-L2884)

```text
new_file(
    name, mode=None
)
```

Open a new temporary file that will be automatically added to the artifact.

| Arguments |  |
| :--- | :--- |
|  `name` |  \(str\) The name of the new file being added to the artifact. |
|  `mode` |  \(str, optional\) The mode in which to open the new file. |

#### Examples:

```text
artifact = wandb.Artifact('my_data', type='dataset')
with artifact.new_file('hello.txt') as f:
    f.write('hello!')
wandb.log_artifact(artifact)
```

| Returns |
| :--- |
|  \(file\): A new file object that can be written to. Upon closing, the file will be automatically added to the artifact. |

### `save` <a id="save"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.28/wandb/apis/public.py#L3102-L3140)

```text
save()
```

Persists artifact changes to the wandb backend.

### `used_by` <a id="used_by"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.28/wandb/apis/public.py#L3272-L3314)

```text
used_by()
```

Retrieves the runs which use this artifact directly

| Returns |
| :--- |
|  \[Run\]: a list of Run objects which use this artifact |

### `verify` <a id="verify"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.28/wandb/apis/public.py#L3032-L3062)

```text
verify(
    root=None
)
```

Verify that the actual contents of an artifact at a specified directory `root` match the expected contents of the artifact according to its manifest.

All files in the directory are checksummed and the checksums are then cross-referenced against the artifact's manifest.

NOTE: References are not verified.

| Arguments |  |
| :--- | :--- |
|  `root` |  \(str, optional\) The directory to verify. If None artifact will be downloaded to './artifacts//' |

| Raises |
| :--- |
|  \(ValueError\): If the verification fails. |

### `wait` <a id="wait"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.28/wandb/apis/public.py#L3142-L3143)

```text
wait()
```

Waits for this artifact to finish logging, if needed.

| Returns |
| :--- |
|  Artifact |

### `__getitem__` <a id="__getitem__"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.28/wandb/apis/public.py#L3354-L3355)

```text
__getitem__(
    name
)
```

Gets the WBValue object located at the artifact relative `name`.

NOTE: This will raise an error unless the artifact has been fetched using `use_artifact`, fetched using the API, or `wait()` has been called.

| Arguments |  |
| :--- | :--- |
|  `name` |  \(str\) The artifact relative name to get |

| Raises |  |
| :--- | :--- |
|  `Exception` |  if problem |

#### Examples:

Basic usage

```text
artifact = wandb.Artifact('my_table', 'dataset')
table = wandb.Table(columns=["a", "b", "c"], data=[[i, i*2, 2**i]])
artifact["my_table"] = table

wandb.log_artifact(artifact)
```

Retrieving an object:

```text
artifact = wandb.use_artifact('my_table:latest')
table = artifact["my_table"]
```

| Class Variables |  |
| :--- | :--- |
|  QUERY |  |

