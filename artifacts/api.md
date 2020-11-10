# Artifacts API

Use W&B Artifacts for dataset tracking and model versioning. Initialize a run, create an artifact, and then use it in another part of your workflow. You can use artifacts to track and save files, or track external URIs.

This feature is available in the client starting from `wandb` version 0.9.0.

## 1. Initialize a run

To track a step of your pipeline, initialize a run in your script. Specify a string for **job\_type** to differentiate different pipeline steps— preprocessing, training, evaluation, etc. If you've never instrumented a run with W&B, we have more detailed guidance for experiment tracking in our [Python Library](../library/) docs.

```python
run = wandb.init(job_type='train')
```

## 2. Create an artifact

An artifact is like a folder of data, with contents that are actual files stored in the artifact or references to external URIs. To create an artifact, log it as the output of a run. Specify a string for **type** to differentiate different artifacts— dataset, model, result etc. Give this artifact a **name**, like `bike-dataset`, to help you remember what is inside the artifact. In a later step of your pipeline, you can use this name along with a version like `bike-dataset:v1` to download this artifact.

When you call **log\_artifact**, we check to see if the contents of the artifact has changed, and if so we automatically create a new version of the artifact: v0, v1, v2 etc.

**wandb.Artifact\(\)**

* **type \(str\)**: Differentiate kinds of artifacts, used for organizational purposes. We recommend sticking to "dataset", "model" and "result".
* **name \(str\)**: Give your artifact a unique name, used when you reference the artifact elsewhere. You can use numbers, letters, underscores, hyphens, and dots in the name.
* **description \(str, optional\)**: Free text displayed next to the artifact version in the UI
* **metadata \(dict, optional\)**: Structured data associated with the artifact, for example class distribution of a dataset. As we build out the web interface, you'll be able to use this data to query and make plots.

```python
artifact = wandb.Artifact('bike-dataset', type='dataset')

# Add a file to the artifact's contents
artifact.add_file('bicycle-data.h5')

# Save the artifact version to W&B and mark it as the output of this run
run.log_artifact(artifact)
```

## 3. Use an artifact

You can use an artifact as input to a run. For example, we could take `bike-dataset:v0` , the first version of `bike-dataset`, and use it in the next script in our pipeline. When you call **use\_artifact**, your script queries W&B to find that named artifact and marks it as input to the run.

```python
# Query W&B for an artifact and mark it as input to this run
artifact = run.use_artifact('bike-dataset:v0')

# Download the artifact's contents
artifact_dir = artifact.download()
```

**Using an artifact from a different project**  
You can freely reference artifacts from any project to which you have access by qualifying the name of the artifact with its project name. You can also reference artifacts across entities by further qualifying the name of the artifact with its entity name.

```python
# Query W&B for an artifact from another project and mark it
# as an input to this run.
artifact = run.use_artifact('my-project/bike-model:v0')

# Use an artifact from another entity and mark it as an input
# to this run.
artifact = run.use_artifact('my-entity/my-project/bike-model:v0')
```

**Using an artifact that has not been logged**  
You can also construct an artifact object and pass it to **use\_artifact**. We check if the artifact already exists in W&B, and if not we creates a new artifact. This is idempotent— you can pass an artifact to use\_artifact as many times as you like, and we'll deduplicate it as long as the contents stay the same.

```python
artifact = wandb.Artifact('bike-model', type='model')
artifact.add_file('model.h5')
run.use_artifact(artifact)
```

## Versions and aliases

When you log an artifact for the first time, we create version **v0**. When you log again to the same artifact, we checksum the contents, and if the artifact has changed we save a new version **v1**.

You can use aliases as pointers to specific versions. By default, run.log\_artifact adds the **latest** alias to the logged version.

You can fetch an artifact using an alias. For example, if you want your training script to always pull the most recent version of a dataset, specify **latest** when you use that artifact.

```python
artifact = run.use_artifact('bike-dataset:latest')
```

You can also apply a custom alias to an artifact version. For example, if you want to mark which model checkpoint is the best on the metric AP-50, you could add the string **best-ap50** as an alias when you log the model artifact.

```python
artifact = wandb.Artifact('run-3nq3ctyy-bike-model', type='model')
artifact.add_file('model.h5')
run.log_artifact(artifact, aliases=['latest','best-ap50'])
```

## Constructing artifacts

An artifact is like a folder of data. Each entry is either an actual file stored in the artifact, or a reference to an external URI. You can nest folders inside an artifact just like a regular filesystem. Construct new artifacts by initializing the `wandb.Artifact()` class.

You can pass the following fields to an `Artifact()` constructor, or set them directly on an artifact object:

* **type:** Should be ‘dataset’, ‘model’, or ‘result’
* **description**: Freeform text that will be displayed in the UI.
* **metadata**: A dictionary that can contain any structured data. You’ll be able to use this data for querying and making plots. E.g. you may choose to store the class distribution for a dataset artifact as metadata.

```python
artifact = wandb.Artifact('bike-dataset', type='dataset')
```

Use **name** to specify an optional file name, or a file path prefix if you're adding a directory.

```python
# Add a single file
artifact.add_file(path, name='optional-name')

# Recursively add a directory
artifact.add_dir(path, name='optional-prefix')

# Return a writeable file-like object, stored as <name> in the artifact
with artifact.new_file(name) as f:
    ...  # Write contents into the file 

# Add a URI reference
artifact.add_reference(uri, name='optional-name')
```

### Adding files and directories

For the following examples, assume we have a project directory with these files:

```text
project-directory
|-- images
|   |-- cat.png
|   +-- dog.png
|-- checkpoints
|   +-- model.h5
+-- model.h5
```

<table>
  <thead>
    <tr>
      <th style="text-align:left">API call</th>
      <th style="text-align:left">Resulting artifact contents</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">artifact.add_file(&apos;model.h5&apos;)</td>
      <td style="text-align:left">model.h5</td>
    </tr>
    <tr>
      <td style="text-align:left">artifact.add_file(&apos;checkpoints/model.h5&apos;)</td>
      <td style="text-align:left">model.h5</td>
    </tr>
    <tr>
      <td style="text-align:left">artifact.add_file(&apos;model.h5&apos;, name=&apos;models/mymodel.h5&apos;)</td>
      <td
      style="text-align:left">models/mymodel.h5</td>
    </tr>
    <tr>
      <td style="text-align:left">artifact.add_dir(&apos;images&apos;)</td>
      <td style="text-align:left">
        <p>cat.png</p>
        <p>dog.png</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">artifact.add_dir(&apos;images&apos;, name=&apos;images&apos;)</td>
      <td
      style="text-align:left">
        <p>images/cat.png</p>
        <p>images/dog.png</p>
        </td>
    </tr>
    <tr>
      <td style="text-align:left">artifact.new_file(&apos;hello.txt&apos;)</td>
      <td style="text-align:left">hello.txt</td>
    </tr>
  </tbody>
</table>

### Adding references

```python
artifact.add_reference(uri, name=None, checksum=True)
```

* **uri \(string\):** The reference URI to track.
* **name \(string\):** An optional name override. If not provided, a name is inferred from **uri**.
* **checksum \(bool\):** If true, the reference collects checksum information and metadata from **uri** for validation purposes.

You can add references to external URIs to artifacts, instead of actual files. If a URI has a scheme that wandb knows how to handle, the artifact will track checksums and other information for reproducibility. Artifacts currently support the following URI schemes:

* `http(s)://`: A path to a file accessible over HTTP. The artifact will track checksums in the form of etags and size metadata if the HTTP server supports the `ETag` and `Content-Length` response headers.
* `s3://`: A path to an object or object prefix in S3. The artifact will track checksums and versioning information \(if the bucket has object versioning enabled\) for the referenced objects. Object prefixes are expanded to include the objects under the prefix, up to a maximum of 10,000 objects.
* `gs://`: A path to an object or object prefix in GCS. The artifact will track checksums and versioning information \(if the bucket has object versioning enabled\) for the referenced objects. Object prefixes are expanded to include the objects under the prefix, up to a maximum of 10,000 objects.

For the following examples, assume we have an S3 bucket with these files:

```text
s3://my-bucket
|-- images
|   |-- cat.png
|   +-- dog.png
|-- checkpoints
|   +-- model.h5
+-- model.h5
```

<table>
  <thead>
    <tr>
      <th style="text-align:left">API call</th>
      <th style="text-align:left">Resulting artifact contents</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">artifact.add_reference(&apos;s3://my-bucket/model.h5&apos;)</td>
      <td style="text-align:left">model.h5</td>
    </tr>
    <tr>
      <td style="text-align:left">artifact.add_reference(&apos;s3://my-bucket/checkpoints/model.h5&apos;)</td>
      <td
      style="text-align:left">model.h5</td>
    </tr>
    <tr>
      <td style="text-align:left">artifact.add_reference(&apos;s3://my-bucket/model.h5&apos;, name=&apos;models/mymodel.h5&apos;)</td>
      <td
      style="text-align:left">models/mymodel.h5</td>
    </tr>
    <tr>
      <td style="text-align:left">artifact.add_reference(&apos;s3://my-bucket/images&apos;)</td>
      <td style="text-align:left">
        <p>cat.png</p>
        <p>dog.png</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">artifact.add_reference(&apos;s3://my-bucket/images&apos;, name=&apos;images&apos;)</td>
      <td
      style="text-align:left">
        <p>images/cat.png</p>
        <p>images/dog.png</p>
        </td>
    </tr>
  </tbody>
</table>

## Using and downloading artifacts

```python
run.use_artifact(artifact=None)
```

* Marks an artifact as an input to your run.

There are two patterns for using artifacts. You can use an artifact name that is explicitly stored in W&B, or you can construct an artifact object and pass it in to be deduplicated as necessary.

### Use an artifact stored in W&B

```python
artifact = run.use_artifact('bike-dataset:latest')
```

You can call the following methods on the returned artifact:

```python
datadir = artifact.download(root=None)
```

* Download all of the artifact’s contents that aren't currently present. This returns a path to a directory containing the artifact’s contents. You can explicitly specify the download destination by setting **root**.

```python
path = artifact.get_path(name)
```

* Fetches only the file at the path `name`. Returns an `Entry` object with the following methods:
  * **Entry.download\(\)**: Downloads file from the artifact at path `name`
  * **Entry.ref\(\)**: If the entry was stored as a reference using `add_reference`, returns the URI

References that have schemes that W&B knows how to handle can be downloaded just like artifact files. The consumer API is the same.

### Construct and use an artifact

You can also construct an artifact object and pass it to **use\_artifact**. This will create the artifact in W&B if it doesn’t exist yet. This is idempotent, so you can do it as many times as you like. The artifact will only be created once, as long as the contents of `model.h5` remain the same.

```python
artifact = wandb.Artifact('reference model')
artifact.add_file('model.h5')
run.use_artifact(artifact)
```

### Download an artifact outside of a run

```python
api = wandb.Api()
artifact = api.artifact('entity/project/artifact:alias')
artifact.download()
```

## Updating artifacts

You can update the `description`, `metadata`, and `aliases` of an artifact by just setting them to the desired values and then calling `save()`.

```python
api = wandb.Api()
artifact = api.artifact('bike-dataset:latest')

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

## Data privacy

Artifacts use secure API-level access control. Files are encrypted at rest and in transit. Artifacts can also track references to private buckets without sending file contents to W&B. For alternatives, contact us at contact@wandb.com to talk about private cloud and on-prem installations.

