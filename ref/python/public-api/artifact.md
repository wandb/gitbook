# Artifact



[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/v0.10.28/wandb/apis/public.py#L2575-L3355)




A wandb Artifact.

<pre><code>Artifact(
    client, entity, project, name, attrs=None
)</code></pre>




An artifact that has been logged, including all its attributes, links to the runs
that use it, and a link to the run that logged it.

#### Examples:

Basic usage
```
api = wandb.Api()
artifact = api.artifact('project/artifact:alias')

# Get information about the artifact...
artifact.digest
artifact.aliases
```

Updating an artifact
```
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
```
artifact = api.artifact('project/artifact:alias')

# Walk up and down the graph from an artifact:
producer_run = artifact.logged_by()
consumer_runs = artifact.used_by()

# Walk up and down the graph from a run:
logged_artifacts = run.logged_artifacts()
used_artifacts = run.used_artifacts()
```

Deleting an artifact
```
artifact = api.artifact('project/artifact:alias')
artifact.delete()
```




<!-- Tabular view -->
<table>
<tr><th>Attributes</th></tr>

<tr>
<td>
<code>aliases</code>
</td>
<td>
The aliases associated with this artifact.
</td>
</tr><tr>
<td>
<code>commit_hash</code>
</td>
<td>
Returns:
(str): The artifact's commit hash which is used in http URLs
</td>
</tr><tr>
<td>
<code>created_at</code>
</td>
<td>
Returns:
(datetime): The time at which the artifact was created.
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
an artifact has the same digest as the current <code>latest</code> version,
then <code>log_artifact</code> is a no-op.
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
<code>updated_at</code>
</td>
<td>
Returns:
(datetime): The time at which the artifact was last updated.
</td>
</tr><tr>
<td>
<code>version</code>
</td>
<td>
Returns:
(int): The version of this artifact. For example, if this
is the first version of an artifact, its <code>version</code> will
be 'v0'.
</td>
</tr>
</table>



## Methods

<h3 id="add"><code>add</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.28/wandb/apis/public.py#L2895-L2896">View source</a>

<pre><code>add(
    obj, name
)</code></pre>

Adds wandb.WBValue <code>obj</code> to the artifact.

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
(wandb.WBValue) The object to add. Currently support one of
Bokeh, JoinedTable, PartitionedTable, Table, Classes, ImageMask,
BoundingBoxes2D, Audio, Image, Video, Html, Object3D
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

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.28/wandb/apis/public.py#L2889-L2890">View source</a>

<pre><code>add_dir(
    path, name=None
)</code></pre>

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
artifact.add_dir('my_dir/', path='destination') # All files in `my_dir/<code> are added under </code>destination/`.
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

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.28/wandb/apis/public.py#L2886-L2887">View source</a>

<pre><code>add_file(
    local_path, name=None, is_tmp=(False)
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

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.28/wandb/apis/public.py#L2892-L2893">View source</a>

<pre><code>add_reference(
    uri, name=None, checksum=(True), max_objects=None
)</code></pre>

Adds a reference denoted by a URI to the artifact. Unlike adding files or directories,
references are NOT uploaded to W&B. However, artifact methods such as <code>download()</code> can
be used regardless of whether the artifact contains references or uploaded files.

By default, W&B offers special
handling for the following schemes:

- http(s): The size and digest of the file will be inferred by the `Content-Length` and
    the <code>ETag</code> response headers returned by the server.
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
# Adds <code>file.txt</code> to the root of the artifact as a reference
artifact.add_reference('http://myserver.com/file.txt')
```

Adding an S3 prefix without an explicit name:
```
# All objects under `prefix/` will be added at the root of the artifact.
artifact.add_reference('s3://mybucket/prefix')
```

Adding a GCS prefix with an explicit name:
```
# All objects under `prefix/<code> will be added under </code>path/` at the top of the artifact.
artifact.add_reference('gs://mybucket/prefix', name='path')
```


<h3 id="checkout"><code>checkout</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.28/wandb/apis/public.py#L3015-L3030">View source</a>

<pre><code>checkout(
    root=None
)</code></pre>

Replaces the specified root directory with the contents of the artifact.

WARNING: This will DELETE all files in <code>root</code> that are not included in the
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

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.28/wandb/apis/public.py#L2866-L2881">View source</a>

<pre><code>delete()</code></pre>

Delete artifact and its files.


<h3 id="download"><code>download</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.28/wandb/apis/public.py#L2978-L3013">View source</a>

<pre><code>download(
    root=None, recursive=(False)
)</code></pre>

Downloads the contents of the artifact to the specified root directory.

NOTE: Any existing files at <code>root</code> are left untouched. Explicitly delete
root before calling <code>download</code> if you want the contents of <code>root</code> to exactly
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



<h3 id="expected_type"><code>expected_type</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.28/wandb/apis/public.py#L2824-L2864">View source</a>

<pre><code>@staticmethod</code>
<code>expected_type(
    client, name, entity_name, project_name
)</code></pre>

Returns the expected type for a given artifact name and project


<h3 id="file"><code>file</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.28/wandb/apis/public.py#L3064-L3085">View source</a>

<pre><code>file(
    root=None
)</code></pre>

Download a single file artifact to dir specified by the <root>


<!-- Tabular view -->
<table>
<tr><th>Arguments</th></tr>

<tr>
<td>
<code>root</code>
</td>
<td>
(str, optional) The root directory in which to place the file. Defaults to './artifacts/<self.name>/'.
</td>
</tr>
</table>



<!-- Tabular view -->
<table>
<tr><th>Returns</th></tr>
<tr>
<td>
(str): The full path of the downloaded file.
</td>
</tr>

</table>



<h3 id="from_id"><code>from_id</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.28/wandb/apis/public.py#L2660-L2700">View source</a>

<pre><code>@classmethod</code>
<code>from_id(
    artifact_id, client
)</code></pre>




<h3 id="get"><code>get</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.28/wandb/apis/public.py#L2950-L2976">View source</a>

<pre><code>get(
    name
)</code></pre>

Gets the WBValue object located at the artifact relative <code>name</code>.

NOTE: This will raise an error unless the artifact has been fetched using
<code>use_artifact</code>, fetched using the API, or <code>wait()</code> has been called.

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


<h3 id="get_path"><code>get_path</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.28/wandb/apis/public.py#L2938-L2948">View source</a>

<pre><code>get_path(
    name
)</code></pre>

Gets the path to the file located at the artifact relative <code>name</code>.

NOTE: This will raise an error unless the artifact has been fetched using
<code>use_artifact</code>, fetched using the API, or <code>wait()</code> has been called.

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

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.28/wandb/apis/public.py#L3316-L3349">View source</a>

<pre><code>logged_by()</code></pre>

Retrieves the run which logged this artifact


<!-- Tabular view -->
<table>
<tr><th>Returns</th></tr>

<tr>
<td>
<code>Run</code>
</td>
<td>
Run object which logged this artifact
</td>
</tr>
</table>



<h3 id="new_file"><code>new_file</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.28/wandb/apis/public.py#L2883-L2884">View source</a>

<pre><code>new_file(
    name, mode=None
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

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.28/wandb/apis/public.py#L3102-L3140">View source</a>

<pre><code>save()</code></pre>

Persists artifact changes to the wandb backend.


<h3 id="used_by"><code>used_by</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.28/wandb/apis/public.py#L3272-L3314">View source</a>

<pre><code>used_by()</code></pre>

Retrieves the runs which use this artifact directly


<!-- Tabular view -->
<table>
<tr><th>Returns</th></tr>
<tr>
<td>
[Run]: a list of Run objects which use this artifact
</td>
</tr>

</table>



<h3 id="verify"><code>verify</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.28/wandb/apis/public.py#L3032-L3062">View source</a>

<pre><code>verify(
    root=None
)</code></pre>

Verify that the actual contents of an artifact at a specified directory
<code>root</code> match the expected contents of the artifact according to its
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

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.28/wandb/apis/public.py#L3142-L3143">View source</a>

<pre><code>wait()</code></pre>

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



<h3 id="__getitem__"><code>__getitem__</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/v0.10.28/wandb/apis/public.py#L3354-L3355">View source</a>

<pre><code>__getitem__(
    name
)</code></pre>

Gets the WBValue object located at the artifact relative <code>name</code>.

NOTE: This will raise an error unless the artifact has been fetched using
<code>use_artifact</code>, fetched using the API, or <code>wait()</code> has been called.

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






<!-- Tabular view -->
<table>
<tr><th>Class Variables</th></tr>

<tr>
<td>
QUERY<a id="QUERY"></a>
</td>
<td>

</td>
</tr>
</table>

