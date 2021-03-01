# Artifact

<!-- Insert buttons and diff -->


[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_artifacts.py#L51-L552)




Constructs an empty artifact whose contents can be populated using its

<pre><code>Artifact(
    name: str,
    type: str,
    description: Optional[str] = None,
    metadata: Optional[dict] = None
) -> None</code></pre>



<!-- Placeholder for "Used in" -->
`add` family of functions. Once the artifact has all the desired files,
you can call `wandb.log_artifact()` to log it.


<!-- Tabular view -->
<table>
<tr><th>Arguments</th></tr>

<tr>
<td>
<code>name</code>
</td>
<td>
(str) A human-readable name for this artifact, which is how you
can identify this artifact in the UI or reference it in `use_artifact`
calls. Names can contain letters, numbers, underscores, hyphens, and
dots. The name must be unique across a project.
</td>
</tr><tr>
<td>
<code>type</code>
</td>
<td>
(str) The type of the artifact, which is used to organize and differentiate
artifacts. Common types include `dataset` or `model`, but you can use any string
containing letters, numbers, underscores, hyphens, and dots.
</td>
</tr><tr>
<td>
<code>description</code>
</td>
<td>
(str, optional) Free text that offers a description of the artifact. The
description is markdown rendered in the UI, so this is a good place to place tables,
links, etc.
</td>
</tr><tr>
<td>
<code>metadata</code>
</td>
<td>
(dict, optional) Structured data associated with the artifact,
for example class distribution of a dataset. This will eventually be queryable
and plottable in the UI. There is a hard limit of 100 total keys.
</td>
</tr>
</table>



#### Examples:

Basic usage
```
wandb.init()

artifact = wandb.Artifact('mnist', type='dataset')
artifact.add_dir('mnist/')
wandb.log_artifact(artifact)
```



<!-- Tabular view -->
<table>
<tr><th>Raises</th></tr>

<tr>
<td>
<code>Exception</code>
</td>
<td>
if problem.
</td>
</tr>
</table>



<!-- Tabular view -->
<table>
<tr><th>Returns</th></tr>
<tr>
<td>
An `Artifact` object.
</td>
</tr>

</table>





<!-- Tabular view -->
<table>
<tr><th>Attributes</th></tr>

<tr>
<td>
<code>aliases</code>
</td>
<td>
Returns:
(list): A list of the aliases associated with this artifact. The list is
mutable and calling `save()` will persist all alias changes.
</td>
</tr><tr>
<td>
<code>description</code>
</td>
<td>
Returns:
(str): Free text that offers a description of the artifact. The
description is markdown rendered in the UI, so this is a good place
to put links, etc.
</td>
</tr><tr>
<td>
<code>digest</code>
</td>
<td>
Returns:
(str): The artifact's logical digest, a checksum of its contents. If
an artifact has the same digest as the current `latest` version,
then `log_artifact` is a no-op.
</td>
</tr><tr>
<td>
<code>distributed_id</code>
</td>
<td>

</td>
</tr><tr>
<td>
<code>entity</code>
</td>
<td>
Returns:
(str): The name of the entity this artifact belongs to.
</td>
</tr><tr>
<td>
<code>id</code>
</td>
<td>
Returns:
(str): The artifact's ID
</td>
</tr><tr>
<td>
<code>manifest</code>
</td>
<td>
Returns:
(ArtifactManifest): The artifact's manifest, listing all of its contents.
You cannot add more files to an artifact once you've retrieved its
manifest.
</td>
</tr><tr>
<td>
<code>metadata</code>
</td>
<td>
Returns:
(dict): Structured data associated with the artifact,
for example class distribution of a dataset. This will eventually be queryable
and plottable in the UI. There is a hard limit of 100 total keys.
</td>
</tr><tr>
<td>
<code>name</code>
</td>
<td>
Returns:
(str): The artifact's name
</td>
</tr><tr>
<td>
<code>project</code>
</td>
<td>
Returns:
(str): The name of the project this artifact belongs to.
</td>
</tr><tr>
<td>
<code>size</code>
</td>
<td>
Returns:
(int): The size in bytes of the artifact. Includes any references
tracked by this artifact.
</td>
</tr><tr>
<td>
<code>state</code>
</td>
<td>
Returns:
(str): The state of the artifact, which can be one of "PENDING",
"COMMITTED", or "DELETED".
</td>
</tr><tr>
<td>
<code>type</code>
</td>
<td>
Returns:
(str): The artifact's type
</td>
</tr><tr>
<td>
<code>version</code>
</td>
<td>
Returns:
(int): The version of this artifact. For example, if this
is the first version of an artifact, its `version` will
be 'v0'.
</td>
</tr>
</table>



## Methods

<h3 id="add"><code>add</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_artifacts.py#L392-L427">View source</a>

<pre><code>add(
    obj: WBValue,
    name: str
)</code></pre>

Adds `obj` to the artifact, where the object is a W&B histogram or
media type.

```
obj = artifact.get(name)
```

<!-- Tabular view -->
<table>
<tr><th>Arguments</th></tr>

<tr>
<td>
<code>obj</code>
</td>
<td>
(wandb.WBValue) The object to add.
</td>
</tr><tr>
<td>
<code>name</code>
</td>
<td>
(str) The path within the artifact to add the object.
</td>
</tr>
</table>



<!-- Tabular view -->
<table>
<tr><th>Returns</th></tr>

<tr>
<td>
<code>ArtifactManifestEntry</code>
</td>
<td>
the added manifest entry
</td>
</tr>
</table>



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


<h3 id="add_dir"><code>add_dir</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_artifacts.py#L328-L361">View source</a>

<pre><code>add_dir(
    local_path: str,
    name: Optional[str] = None
) -> None</code></pre>

Adds a local directory to the artifact.


<!-- Tabular view -->
<table>
<tr><th>Arguments</th></tr>

<tr>
<td>
<code>local_path</code>
</td>
<td>
(str) The path to the directory being added.
</td>
</tr><tr>
<td>
<code>name</code>
</td>
<td>
(str, optional) The path within the artifact to use for the directory being added. Defaults
to files being added under the root of the artifact.
</td>
</tr>
</table>



#### Examples:

Adding a directory without an explicit name:
```
artifact.add_dir('my_dir/') # All files in `my_dir/` are added at the root of the artifact.
```

Adding a directory without an explicit name:
```
artifact.add_dir('my_dir/', path='destination') # All files in `my_dir/` are added under `destination/`.
```



<!-- Tabular view -->
<table>
<tr><th>Raises</th></tr>

<tr>
<td>
<code>Exception</code>
</td>
<td>
if problem.
</td>
</tr>
</table>



<!-- Tabular view -->
<table>
<tr><th>Returns</th></tr>
<tr>
<td>
None
</td>
</tr>

</table>



<h3 id="add_file"><code>add_file</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_artifacts.py#L307-L326">View source</a>

<pre><code>add_file(
    local_path: str,
    name: Optional[str] = None,
    is_tmp: Optional[bool] = (False)
)</code></pre>

Adds a local file to the artifact.


<!-- Tabular view -->
<table>
<tr><th>Arguments</th></tr>

<tr>
<td>
<code>local_path</code>
</td>
<td>
(str) The path to the file being added.
</td>
</tr><tr>
<td>
<code>name</code>
</td>
<td>
(str, optional) The path within the artifact to use for the file being added. Defaults
to the basename of the file.
</td>
</tr><tr>
<td>
<code>is_tmp</code>
</td>
<td>
(bool, optional) If true, then the file is renamed deterministically to avoid collisions.
(default: False)
</td>
</tr>
</table>



#### Examples:

Adding a file without an explicit name:
```
artifact.add_file('path/to/file.txt') # Added as `file.txt'
```

Adding a file with an explicit name:
```
artifact.add_file('path/to/file.txt', name='new/path/file.txt') # Added as 'new/path/file.txt'
```



<!-- Tabular view -->
<table>
<tr><th>Raises</th></tr>

<tr>
<td>
<code>Exception</code>
</td>
<td>
if problem
</td>
</tr>
</table>



<!-- Tabular view -->
<table>
<tr><th>Returns</th></tr>

<tr>
<td>
<code>ArtifactManifestEntry</code>
</td>
<td>
the added manifest entry
</td>
</tr>
</table>



<h3 id="add_reference"><code>add_reference</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_artifacts.py#L363-L390">View source</a>

<pre><code>add_reference(
    uri: Union[ArtifactEntry, str],
    name: Optional[str] = None,
    checksum: bool = (True),
    max_objects: Optional[int] = None
)</code></pre>

Adds a reference denoted by a URI to the artifact. Unlike adding files or directories,
references are NOT uploaded to W&B. However, artifact methods such as `download()` can
be used regardless of whether the artifact contains references or uploaded files.

By default, W&B offers special
handling for the following schemes:

- http(s): The size and digest of the file will be inferred by the `Content-Length` and
    the `ETag` response headers returned by the server.
- s3: The checksum and size will be pulled from the object metadata. If bucket versioning
    is enabled, then the version ID is also tracked.
- gs: The checksum and size will be pulled from the object metadata. If bucket versioning
    is enabled, then the version ID is also tracked.
- file: The checksum and size will be pulled from the file system. This scheme is useful if
    you have an NFS share or other externally mounted volume containing files you wish to track
    but not necessarily upload.

For any other scheme, the digest is just a hash of the URI and the size is left blank.

<!-- Tabular view -->
<table>
<tr><th>Arguments</th></tr>

<tr>
<td>
<code>uri</code>
</td>
<td>
(str) The URI path of the reference to add. Can be an object returned from
Artifact.get_path to store a reference to another artifact's entry.
</td>
</tr><tr>
<td>
<code>name</code>
</td>
<td>
(str) The path within the artifact to place the contents of this reference
</td>
</tr><tr>
<td>
<code>checksum</code>
</td>
<td>
(bool, optional) Whether or not to checksum the resource(s) located at the
reference URI. Checksumming is strongly recommended as it enables automatic integrity
validation, however it can be disabled to speed up artifact creation. (default: True)
</td>
</tr><tr>
<td>
<code>max_objects</code>
</td>
<td>
(int, optional) The maximum number of objects to consider when adding a
reference that points to directory or bucket store prefix. For S3 and GCS, this limit
is 10,000 by default but is uncapped for other URI schemes. (default: None)
</td>
</tr>
</table>



<!-- Tabular view -->
<table>
<tr><th>Raises</th></tr>

<tr>
<td>
<code>Exception</code>
</td>
<td>
If problem.
</td>
</tr>
</table>



<!-- Tabular view -->
<table>
<tr><th>Returns</th></tr>
<tr>
<td>
List[ArtifactManifestEntry]: The added manifest entries.
</td>
</tr>

</table>



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


<h3 id="checkout"><code>checkout</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_artifacts.py#L453-L459">View source</a>

<pre><code>checkout(
    root: Optional[str] = None
) -> str</code></pre>

Replaces the specified root directory with the contents of the artifact.

WARNING: This will DELETE all files in `root` that are not included in the
artifact.

<!-- Tabular view -->
<table>
<tr><th>Arguments</th></tr>

<tr>
<td>
<code>root</code>
</td>
<td>
(str, optional) The directory to replace with this artifact's files.
</td>
</tr>
</table>



<!-- Tabular view -->
<table>
<tr><th>Returns</th></tr>
<tr>
<td>
(str): The path to the checked out contents.
</td>
</tr>

</table>



<h3 id="delete"><code>delete</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_artifacts.py#L477-L483">View source</a>

<pre><code>delete() -> None</code></pre>

Deletes this artifact, cleaning up all files associated with it.

NOTE: Deletion is permanent and CANNOT be undone.

<!-- Tabular view -->
<table>
<tr><th>Returns</th></tr>
<tr>
<td>
None
</td>
</tr>

</table>



<h3 id="download"><code>download</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_artifacts.py#L445-L451">View source</a>

<pre><code>download(
    root: str = None,
    recursive: bool = (False)
)</code></pre>

Downloads the contents of the artifact to the specified root directory.

NOTE: Any existing files at `root` are left untouched. Explicitly delete
root before calling `download` if you want the contents of `root` to exactly
match the artifact.

<!-- Tabular view -->
<table>
<tr><th>Arguments</th></tr>

<tr>
<td>
<code>root</code>
</td>
<td>
(str, optional) The directory in which to download this artifact's files.
</td>
</tr><tr>
<td>
<code>recursive</code>
</td>
<td>
(bool, optional) If true, then all dependent artifacts are eagerly
downloaded. Otherwise, the dependent artifacts are downloaded as needed.
</td>
</tr>
</table>



<!-- Tabular view -->
<table>
<tr><th>Returns</th></tr>
<tr>
<td>
(str): The path to the downloaded contents.
</td>
</tr>

</table>



<h3 id="finalize"><code>finalize</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_artifacts.py#L518-L532">View source</a>

<pre><code>finalize()</code></pre>

Marks this artifact as final, which disallows further additions to the artifact.
This happens automatically when calling `log_artifact`.


<!-- Tabular view -->
<table>
<tr><th>Returns</th></tr>
<tr>
<td>
None
</td>
</tr>

</table>



<h3 id="get"><code>get</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_artifacts.py#L437-L443">View source</a>

<pre><code>get(
    name: str
)</code></pre>

Gets the WBValue object located at the artifact relative `name`.

NOTE: This will raise an error unless the artifact has been fetched using
`use_artifact` or the API.

<!-- Tabular view -->
<table>
<tr><th>Arguments</th></tr>

<tr>
<td>
<code>name</code>
</td>
<td>
(str) The artifact relative name to get
</td>
</tr>
</table>



<!-- Tabular view -->
<table>
<tr><th>Raises</th></tr>

<tr>
<td>
<code>Exception</code>
</td>
<td>
if problem
</td>
</tr>
</table>



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


<h3 id="get_added_local_path_name"><code>get_added_local_path_name</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_artifacts.py#L493-L516">View source</a>

<pre><code>get_added_local_path_name(
    local_path: str
)</code></pre>

Get the artifact relative name of a file added by a local filesystem path.


<!-- Tabular view -->
<table>
<tr><th>Arguments</th></tr>

<tr>
<td>
<code>local_path</code>
</td>
<td>
(str) The local path to resolve into an artifact relative name.
</td>
</tr>
</table>



<!-- Tabular view -->
<table>
<tr><th>Returns</th></tr>

<tr>
<td>
<code>str</code>
</td>
<td>
The artifact relative name.
</td>
</tr>
</table>



#### Examples:

Basic usage
```
artifact = wandb.Artifact('my_dataset', type='dataset')
artifact.add_file('path/to/file.txt', name='artifact/path/file.txt')

# Returns `artifact/path/file.txt`:
name = artifact.get_added_local_path_name('path/to/file.txt')
```


<h3 id="get_path"><code>get_path</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_artifacts.py#L429-L435">View source</a>

<pre><code>get_path(
    name: str
)</code></pre>

Gets the path to the file located at the artifact relative `name`.

NOTE: This will raise an error unless the artifact has been fetched using
`use_artifact` or the API.

<!-- Tabular view -->
<table>
<tr><th>Arguments</th></tr>

<tr>
<td>
<code>name</code>
</td>
<td>
(str) The artifact relative name to get
</td>
</tr>
</table>



<!-- Tabular view -->
<table>
<tr><th>Raises</th></tr>

<tr>
<td>
<code>Exception</code>
</td>
<td>
if problem
</td>
</tr>
</table>



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


<h3 id="logged_by"><code>logged_by</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_artifacts.py#L284-L290">View source</a>

<pre><code>logged_by() -> "wandb.apis.public.Run"</code></pre>

Returns:
    (Run): The run that first logged this artifact.

<h3 id="new_file"><code>new_file</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_artifacts.py#L292-L305">View source</a>

<pre><code>@contextlib.contextmanager</code>
<code>new_file(
    name: str,
    mode: str = &#x27;w&#x27;
)</code></pre>

Open a new temporary file that will be automatically added to the artifact.


<!-- Tabular view -->
<table>
<tr><th>Arguments</th></tr>

<tr>
<td>
<code>name</code>
</td>
<td>
(str) The name of the new file being added to the artifact.
</td>
</tr><tr>
<td>
<code>mode</code>
</td>
<td>
(str, optional) The mode in which to open the new file.
</td>
</tr>
</table>



#### Examples:

```
artifact = wandb.Artifact('my_data', type='dataset')
with artifact.new_file('hello.txt') as f:
    f.write('hello!')
wandb.log_artifact(artifact)
```



<!-- Tabular view -->
<table>
<tr><th>Returns</th></tr>
<tr>
<td>
(file): A new file object that can be written to. Upon closing,
the file will be automatically added to the artifact.
</td>
</tr>

</table>



<h3 id="save"><code>save</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_artifacts.py#L469-L475">View source</a>

<pre><code>save() -> None</code></pre>

Persists any changes made to the artifact.


<!-- Tabular view -->
<table>
<tr><th>Returns</th></tr>
<tr>
<td>
None
</td>
</tr>

</table>



<h3 id="used_by"><code>used_by</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_artifacts.py#L276-L282">View source</a>

<pre><code>used_by() -> List['wandb.apis.public.Run']</code></pre>

Returns:
    (list): A list of the runs that have used this artifact.

<h3 id="verify"><code>verify</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_artifacts.py#L461-L467">View source</a>

<pre><code>verify(
    root: Optional[str] = None
)</code></pre>

Verify that the actual contents of an artifact at a specified directory
`root` match the expected contents of the artifact according to its
manifest.

All files in the directory are checksummed and the checksums are then
cross-referenced against the artifact's manifest.

NOTE: References are not verified.

<!-- Tabular view -->
<table>
<tr><th>Arguments</th></tr>

<tr>
<td>
<code>root</code>
</td>
<td>
(str, optional) The directory to verify. If None
artifact will be downloaded to './artifacts/<self.name>/'
</td>
</tr>
</table>



<!-- Tabular view -->
<table>
<tr><th>Raises</th></tr>
<tr>
<td>
(ValueError): If the verification fails.
</td>
</tr>

</table>



<h3 id="wait"><code>wait</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_artifacts.py#L485-L491">View source</a>

<pre><code>wait() -> ArtifactInterface</code></pre>

Waits for this artifact to finish logging, if needed.


<!-- Tabular view -->
<table>
<tr><th>Returns</th></tr>
<tr>
<td>
Artifact
</td>
</tr>

</table>





