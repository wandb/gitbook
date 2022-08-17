# wandb.Artifact

[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/latest/wandb/sdk/wandb\_artifacts.py#L79-L723)

Flexible and lightweight building block for dataset and model versioning.

```python
Artifact(
    name: str,
    type: str,
    description: Optional[str] = None,
    metadata: Optional[dict] = None,
    incremental: Optional[bool] = None,
    use_as: Optional[str] = None
) -> None
```

Constructs an empty artifact whose contents can be populated using its `add` family of functions. Once the artifact has all the desired files, you can call `wandb.log_artifact()` to log it.

| Arguments     |                                                                                                                                                                                                                                                             |
| ------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `name`        | (str) A human-readable name for this artifact, which is how you can identify this artifact in the UI or reference it in `use_artifact` calls. Names can contain letters, numbers, underscores, hyphens, and dots. The name must be unique across a project. |
| `type`        | (str) The type of the artifact, which is used to organize and differentiate artifacts. Common types include `dataset` or `model`, but you can use any string containing letters, numbers, underscores, hyphens, and dots.                                   |
| `description` | (str, optional) Free text that offers a description of the artifact. The description is markdown rendered in the UI, so this is a good place to place tables, links, etc.                                                                                   |
| `metadata`    | (dict, optional) Structured data associated with the artifact, for example class distribution of a dataset. This will eventually be queryable and plottable in the UI. There is a hard limit of 100 total keys.                                             |

#### Examples:

Basic usage

```
wandb.init()

artifact = wandb.Artifact('mnist', type='dataset')
artifact.add_dir('mnist/')
wandb.log_artifact(artifact)
```

| Raises      |             |
| ----------- | ----------- |
| `Exception` | if problem. |

| Returns               |   |
| --------------------- | - |
| An `Artifact` object. |   |

| Attributes    |                                                                                                                                                                                                                 |
| ------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `aliases`     | Returns: (list): A list of the aliases associated with this artifact. The list is mutable and calling `save()` will persist all alias changes.                                                                  |
| `commit_hash` | Returns: (str): The artifact's commit hash which is used in http URLs                                                                                                                                           |
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
| `version`     | Returns: (int): The version of this artifact. For example, if this is the first version of an artifact, its `version` will be 'v0'.                                                                             |

## Methods

### `add` <a href="#add" id="add"></a>

[View source](https://www.github.com/wandb/client/tree/latest/wandb/sdk/wandb\_artifacts.py#L463-L544)

```python
add(
    obj: data_types.WBValue,
    name: str
) -> ArtifactEntry
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

```
artifact = wandb.Artifact('my_table', 'dataset')
table = wandb.Table(columns=["a", "b", "c"], data=[[i, i*2, 2**i]])
artifact.add(table, "my_table")

wandb.log_artifact(artifact)
```

Retrieving an object:

```
artifact = wandb.use_artifact('my_table:latest')
table = artifact.get("my_table")
```

### `add_dir` <a href="#add_dir" id="add_dir"></a>

[View source](https://www.github.com/wandb/client/tree/latest/wandb/sdk/wandb\_artifacts.py#L394-L427)

```python
add_dir(
    local_path: str,
    name: Optional[str] = None
) -> None
```

Adds a local directory to the artifact.

| Arguments    |                                                                                                                                                  |
| ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| `local_path` | (str) The path to the directory being added.                                                                                                     |
| `name`       | (str, optional) The path within the artifact to use for the directory being added. Defaults to files being added under the root of the artifact. |

#### Examples:

Adding a directory without an explicit name:

```
artifact.add_dir('my_dir/') # All files in `my_dir/` are added at the root of the artifact.
```

Adding a directory without an explicit name:

```
artifact.add_dir('my_dir/', path='destination') # All files in `my_dir/` are added under `destination/`.
```

| Raises      |             |
| ----------- | ----------- |
| `Exception` | if problem. |

| Returns |   |
| ------- | - |
| None    |   |

### `add_file` <a href="#add_file" id="add_file"></a>

[View source](https://www.github.com/wandb/client/tree/latest/wandb/sdk/wandb\_artifacts.py#L373-L392)

```python
add_file(
    local_path: str,
    name: Optional[str] = None,
    is_tmp: Optional[bool] = (False)
) -> ArtifactEntry
```

Adds a local file to the artifact.

| Arguments    |                                                                                                                     |
| ------------ | ------------------------------------------------------------------------------------------------------------------- |
| `local_path` | (str) The path to the file being added.                                                                             |
| `name`       | (str, optional) The path within the artifact to use for the file being added. Defaults to the basename of the file. |
| `is_tmp`     | (bool, optional) If true, then the file is renamed deterministically to avoid collisions. (default: False)          |

#### Examples:

Adding a file without an explicit name:

```
artifact.add_file('path/to/file.txt') # Added as `file.txt'
```

Adding a file with an explicit name:

```
artifact.add_file('path/to/file.txt', name='new/path/file.txt') # Added as 'new/path/file.txt'
```

| Raises      |            |
| ----------- | ---------- |
| `Exception` | if problem |

| Returns                 |                          |
| ----------------------- | ------------------------ |
| `ArtifactManifestEntry` | the added manifest entry |

### `add_reference` <a href="#add_reference" id="add_reference"></a>

[View source](https://www.github.com/wandb/client/tree/latest/wandb/sdk/wandb\_artifacts.py#L429-L461)

```python
add_reference(
    uri: Union[ArtifactEntry, str],
    name: Optional[str] = None,
    checksum: bool = (True),
    max_objects: Optional[int] = None
) -> Sequence[ArtifactEntry]
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

```
# Adds `file.txt` to the root of the artifact as a reference
artifact.add_reference('http://myserver.com/file.txt')
```

Adding an S3 prefix without an explicit name:

```
# All objects under `prefix/` will be added at the root of the artifact.
artifact.add_reference('s3://mybucket/prefix')
```

Adding a GCS prefix with an explicit name:

```
# All objects under `prefix/` will be added under `path/` at the top of the artifact.
artifact.add_reference('gs://mybucket/prefix', name='path')
```

### `checkout` <a href="#checkout" id="checkout"></a>

[View source](https://www.github.com/wandb/client/tree/latest/wandb/sdk/wandb\_artifacts.py#L570-L576)

```python
checkout(
    root: Optional[str] = None
) -> str
```

Replaces the specified root directory with the contents of the artifact.

WARNING: This will DELETE all files in `root` that are not included in the artifact.

| Arguments |                                                                      |
| --------- | -------------------------------------------------------------------- |
| `root`    | (str, optional) The directory to replace with this artifact's files. |

| Returns                                      |   |
| -------------------------------------------- | - |
| (str): The path to the checked out contents. |   |

### `delete` <a href="#delete" id="delete"></a>

[View source](https://www.github.com/wandb/client/tree/latest/wandb/sdk/wandb\_artifacts.py#L627-L633)

```python
delete() -> None
```

Deletes this artifact, cleaning up all files associated with it.

NOTE: Deletion is permanent and CANNOT be undone.

| Returns |   |
| ------- | - |
| None    |   |

### `download` <a href="#download" id="download"></a>

[View source](https://www.github.com/wandb/client/tree/latest/wandb/sdk/wandb\_artifacts.py#L562-L568)

```python
download(
    root: str = None,
    recursive: bool = (False)
) -> str
```

Downloads the contents of the artifact to the specified root directory.

NOTE: Any existing files at `root` are left untouched. Explicitly delete root before calling `download` if you want the contents of `root` to exactly match the artifact.

| Arguments   |                                                                                                                                             |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| `root`      | (str, optional) The directory in which to download this artifact's files.                                                                   |
| `recursive` | (bool, optional) If true, then all dependent artifacts are eagerly downloaded. Otherwise, the dependent artifacts are downloaded as needed. |

| Returns                                     |   |
| ------------------------------------------- | - |
| (str): The path to the downloaded contents. |   |

### `finalize` <a href="#finalize" id="finalize"></a>

[View source](https://www.github.com/wandb/client/tree/latest/wandb/sdk/wandb\_artifacts.py#L668-L682)

```python
finalize() -> None
```

Marks this artifact as final, which disallows further additions to the artifact. This happens automatically when calling `log_artifact`.

| Returns |   |
| ------- | - |
| None    |   |

### `get` <a href="#get" id="get"></a>

[View source](https://www.github.com/wandb/client/tree/latest/wandb/sdk/wandb\_artifacts.py#L554-L560)

```python
get(
    name: str
) -> data_types.WBValue
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

```
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

### `get_added_local_path_name` <a href="#get_added_local_path_name" id="get_added_local_path_name"></a>

[View source](https://www.github.com/wandb/client/tree/latest/wandb/sdk/wandb\_artifacts.py#L643-L666)

```python
get_added_local_path_name(
    local_path: str
) -> Optional[str]
```

Get the artifact relative name of a file added by a local filesystem path.

| Arguments    |                                                                 |
| ------------ | --------------------------------------------------------------- |
| `local_path` | (str) The local path to resolve into an artifact relative name. |

| Returns |                             |
| ------- | --------------------------- |
| `str`   | The artifact relative name. |

#### Examples:

Basic usage

```
artifact = wandb.Artifact('my_dataset', type='dataset')
artifact.add_file('path/to/file.txt', name='artifact/path/file.txt')

# Returns `artifact/path/file.txt`:
name = artifact.get_added_local_path_name('path/to/file.txt')
```

### `get_path` <a href="#get_path" id="get_path"></a>

[View source](https://www.github.com/wandb/client/tree/latest/wandb/sdk/wandb\_artifacts.py#L546-L552)

```python
get_path(
    name: str
) -> ArtifactEntry
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

```
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

### `json_encode` <a href="#json_encode" id="json_encode"></a>

[View source](https://www.github.com/wandb/client/tree/latest/wandb/sdk/wandb\_artifacts.py#L684-L689)

```python
json_encode() -> Dict[str, Any]
```

### `link` <a href="#link" id="link"></a>

[View source](https://www.github.com/wandb/client/tree/latest/wandb/sdk/interface/artifacts.py#L676-L689)

```python
link(
    target_path: str,
    aliases: Optional[List[str]] = None
) -> None
```

Links this artifact to a Registered Model with optional aliases.

| Arguments     |                                                                                                                                                                                                                                                                                               |
| ------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `target_path` | <p>(str) The path to the Registered Model. Suppose we have a Registered Model named "XYZ". The argument <code>target_path</code> must take one of the following forms:<br>- <code>"XYZ"</code><br><code></code>- <code>"{project}/XYZ"</code> <br>- <code>"{entity}/{project}/XYZ"</code></p> |
| `aliases`     | (Optional\[List\[str]]) A list of strings which uniquely identifies the artifact inside the specified Registered Model.                                                                                                                                                                       |

| Returns |   |
| ------- | - |
| None    |   |

### `logged_by` <a href="#logged_by" id="logged_by"></a>

[View source](https://www.github.com/wandb/client/tree/latest/wandb/sdk/wandb\_artifacts.py#L345-L351)

```python
logged_by() -> "wandb.apis.public.Run"
```

Returns: (Run): The run that first logged this artifact.

### `new_file` <a href="#new_file" id="new_file"></a>

[View source](https://www.github.com/wandb/client/tree/latest/wandb/sdk/wandb\_artifacts.py#L353-L371)

```python
@contextlib.contextmanager
new_file(
    name: str,
    mode: str = "w",
    encoding: Optional[str] = None
) -> Generator[IO, None, None]
```

Open a new temporary file that will be automatically added to the artifact.

| Arguments  |                                                             |
| ---------- | ----------------------------------------------------------- |
| `name`     | (str) The name of the new file being added to the artifact. |
| `mode`     | (str, optional) The mode in which to open the new file.     |
| `encoding` | (str, optional) The encoding in which to open the new file. |

#### Examples:

```
artifact = wandb.Artifact('my_data', type='dataset')
with artifact.new_file('hello.txt') as f:
    f.write('hello!')
wandb.log_artifact(artifact)
```

| Returns                                                                                                               |   |
| --------------------------------------------------------------------------------------------------------------------- | - |
| (file): A new file object that can be written to. Upon closing, the file will be automatically added to the artifact. |   |

### `save` <a href="#save" id="save"></a>

[View source](https://www.github.com/wandb/client/tree/latest/wandb/sdk/wandb\_artifacts.py#L586-L625)

```python
save(
    project: Optional[str] = None,
    settings: Optional['wandb.wandb_sdk.wandb_settings.Settings'] = None
) -> None
```

Persists any changes made to the artifact. If currently in a run, that run will log this artifact. If not currently in a run, a run of type "auto" will be created to track this artifact.

| Arguments  |                                                                                                                                |
| ---------- | ------------------------------------------------------------------------------------------------------------------------------ |
| `project`  | (str, optional) A project to use for the artifact in the case that a run is not already in context                             |
| `settings` | (wandb.Settings, optional) A settings object to use when initializing an automatic run. Most commonly used in testing harness. |

| Returns |   |
| ------- | - |
| None    |   |

### `used_by` <a href="#used_by" id="used_by"></a>

[View source](https://www.github.com/wandb/client/tree/latest/wandb/sdk/wandb\_artifacts.py#L337-L343)

```python
used_by() -> List['wandb.apis.public.Run']
```

Returns: (list): A list of the runs that have used this artifact.

### `verify` <a href="#verify" id="verify"></a>

[View source](https://www.github.com/wandb/client/tree/latest/wandb/sdk/wandb\_artifacts.py#L578-L584)

```python
verify(
    root: Optional[str] = None
) -> bool
```

Verify that the actual contents of an artifact at a specified directory `root` match the expected contents of the artifact according to its manifest.

All files in the directory are checksummed and the checksums are then cross-referenced against the artifact's manifest.

NOTE: References are not verified.

| Arguments |                                                                                                             |
| --------- | ----------------------------------------------------------------------------------------------------------- |
| `root`    | (str, optional) The directory to verify. If None artifact will be downloaded to './artifacts/\<self.name>/' |

| Raises                                   |   |
| ---------------------------------------- | - |
| (ValueError): If the verification fails. |   |

### `wait` <a href="#wait" id="wait"></a>

[View source](https://www.github.com/wandb/client/tree/latest/wandb/sdk/wandb\_artifacts.py#L635-L641)

```python
wait() -> ArtifactInterface
```

Waits for this artifact to finish logging, if needed.

| Returns  |   |
| -------- | - |
| Artifact |   |

### `__getitem__` <a href="#__getitem__" id="__getitem__"></a>

[View source](https://www.github.com/wandb/client/tree/latest/wandb/sdk/wandb\_artifacts.py#L722-L723)

```python
__getitem__(
    name: str
) -> Optional[data_types.WBValue]
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

```
artifact = wandb.Artifact('my_table', 'dataset')
table = wandb.Table(columns=["a", "b", "c"], data=[[i, i*2, 2**i]])
artifact["my_table"] = table

wandb.log_artifact(artifact)
```

Retrieving an object:

```
artifact = wandb.use_artifact('my_table:latest')
table = artifact["my_table"]
```
